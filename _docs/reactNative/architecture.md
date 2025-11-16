---
layout: default
title: RN 아키텍처
parent: React Native
---

# React Native Architecture
{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## 1. Legacy vs New Architecture 핵심 비교

|      구분       |  Legacy Bridge  |   New Architecture    |
| :-------------: | :-------------: | :-------------------: |
|  **개발 기간**  | 2015년 - 2025년 |     2018년 - 현재     |
|   **안정화**    |     2015년      | 2024년 10월 (RN 0.76) |
|  **통신 계층**  |   JSON Bridge   |       JSI (C++)       |
|  **통신 방식**  |   비동기 전용   |      동기/비동기      |
|   **렌더러**    | Legacy Renderer |        Fabric         |
| **Native 모듈** | Native Modules  |     Turbo Modules     |
| **데이터 전달** |   JSON 직렬화   |   Direct Reference    |
|  **타입 체크**  |     런타임      |  Codegen (빌드 타임)  |
|   **초기화**    | 앱 시작 시 전체 |   지연 로딩 (Lazy)    |
| **메모리 효율** |      낮음       |         높음          |
|  **C++ 지원**   |      없음       |       완전 지원       |
|   **디버깅**    | Chrome DevTools | React Native DevTools |

---

## 2. Legacy vs New Architecture 상세 비교

### 2.1 아키텍처 구조 비교

#### Legacy Bridge Architecture

```
┌─────────────────────────────────────────────┐
│         JavaScript Thread                   │
│  (React Components, Business Logic)         │
└──────────────────┬──────────────────────────┘
                   │
                   │ JSON 직렬화
                   ▼
            ┌──────────────┐
            │    Bridge    │  ← 병목 지점!
            │ (비동기 큐)   │
            └──────┬───────┘
                   │ JSON 역직렬화
                   ▼
┌──────────────────┴──────────────────────────┐
│         Native Threads                      │
├─────────────────┬───────────────────────────┤
│ Shadow Thread   │   Main Thread (UI)        │
│ (레이아웃 계산)  │   (네이티브 모듈 실행)    │
└─────────────────┴───────────────────────────┘
```

**문제점:**

- ❌ 모든 통신이 Bridge를 경유 (Single Point of Failure)
- ❌ JSON 직렬화/역직렬화 오버헤드
- ❌ 비동기 통신만 가능 (동기 호출 불가)
- ❌ 앱 시작 시 모든 Native Module 로드

#### New Architecture

```
┌─────────────────────────────────────────────┐
│         JavaScript Thread                   │
│       (Hermes Engine + JSI)                 │
└──────────────────┬──────────────────────────┘
                   │
                   │ JSI (C++ Direct Call)
                   ▼
            ┌──────────────┐
            │     JSI      │  ← No Bridge!
            │ (Direct Ref) │
            └──────┬───────┘
                   │
┌──────────────────┴──────────────────────────┐
│              C++ Layer                      │
├─────────────────┬───────────────────────────┤
│  Turbo Modules  │      Fabric Renderer      │
│  (Native Logic) │      (UI Components)      │
└─────────────────┴───────────────────────────┘
```

**개선점:**

- ✅ Bridge 제거 → 직접 호출
- ✅ Zero-copy 데이터 전달
- ✅ 동기/비동기 모두 지원
- ✅ 지연 로딩 (Lazy Initialization)

**코드 참고:**

- [JSI 인터페이스 구현](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/jsi)

### 2.2 통신 방식 비교

#### Legacy: Bridge를 통한 비동기 통신

```javascript
// JavaScript
import { NativeModules } from "react-native";
const { LocationModule } = NativeModules;

// 1️⃣ 비동기만 가능
LocationModule.getCurrentPosition().then((position) => {
  // JSON 파싱 후 데이터 수신
  console.log(position.latitude, position.longitude);
});

// 2️⃣ 동기 호출 불가능
// const position = LocationModule.getCurrentPositionSync(); // ❌ 불가능!

// 3️⃣ 타입 안전성 없음
LocationModule.setSpeed("fast"); // 런타임 에러 가능
```

**통신 과정:**

```
JavaScript
    ↓ [1. Method Call]
    ↓ [2. JSON.stringify]
Bridge Queue
    ↓ [3. Queue에서 대기]
    ↓ [4. Native로 전송]
Native
    ↓ [5. JSON.parse]
    ↓ [6. 실제 함수 실행]
    ↓ [7. JSON.stringify]
Bridge Queue
    ↓ [8. JavaScript로 전송]
JavaScript
    ↓ [9. JSON.parse]
    ↓ [10. Callback 실행]
```

#### New Architecture: JSI를 통한 직접 호출

```typescript
// JavaScript (TypeScript)
import LocationModule from "./specs/NativeLocationModule";

// 1️⃣ 동기 호출 가능
const position = LocationModule.getCurrentPositionSync();
console.log(position.latitude, position.longitude); // 즉시 실행

// 2️⃣ 비동기도 가능
const position2 = await LocationModule.getCurrentPosition();

// 3️⃣ 타입 안전성 보장
LocationModule.setSpeed(50); // ✅ number
// LocationModule.setSpeed('fast'); // ❌ TypeScript 컴파일 에러
```

**통신 과정:**

```
JavaScript
    ↓ [1. JSI Function Call]
C++ (JSI)
    ↓ [2. Direct Memory Access]
Native
    ↓ [3. 실제 함수 실행]
    ↓ [4. Direct Return]
JavaScript
    ↓ [5. 값 즉시 사용]
```

**코드 참고:**

- [TurboModule 구현 예시](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/react/nativemodule/core)
- [TurboModuleRegistry](https://github.com/facebook/react-native/blob/main/packages/react-native/Libraries/TurboModule/TurboModuleRegistry.js)

### 2.3 Native Module 생성 비교

#### Legacy Native Module

**JavaScript**

```javascript
// LocationModule.js
import { NativeModules } from "react-native";
const { LocationModule } = NativeModules;

export default LocationModule;
```

**iOS (Objective-C / Swift)**

**Objective-C 방식 (전통적)**

```objc
// RCTLocationModule.m
#import <React/RCTBridgeModule.h>
#import <CoreLocation/CoreLocation.h>

@interface RCTLocationModule : NSObject <RCTBridgeModule>
@end

@implementation RCTLocationModule

RCT_EXPORT_MODULE(LocationModule);

// 비동기만 가능
RCT_EXPORT_METHOD(getCurrentPosition:(RCTPromiseResolveBlock)resolve
                  rejecter:(RCTPromiseRejectBlock)reject) {
  CLLocationManager *manager = [[CLLocationManager alloc] init];
  CLLocation *location = manager.location;

  // Dictionary로 변환 (JSON 직렬화를 위해)
  NSDictionary *result = @{
    @"latitude": @(location.coordinate.latitude),
    @"longitude": @(location.coordinate.longitude)
  };
  resolve(result);
}

@end
```

**Swift 방식 (최신)**

```swift
// LocationModule.swift
import Foundation
import CoreLocation

@objc(LocationModule)
class LocationModule: NSObject {

  @objc
  static func requiresMainQueueSetup() -> Bool {
    return false
  }

  @objc
  func getCurrentPosition(
    _ resolve: @escaping RCTPromiseResolveBlock,
    rejecter reject: @escaping RCTPromiseRejectBlock
  ) {
    let manager = CLLocationManager()
    guard let location = manager.location else {
      reject("ERROR", "Location not available", nil)
      return
    }

    // Dictionary로 변환 (JSON 직렬화를 위해)
    let result: [String: Any] = [
      "latitude": location.coordinate.latitude,
      "longitude": location.coordinate.longitude
    ]
    resolve(result)
  }
}
```

```objc
// LocationModule.m (Bridge 파일 - Swift와 함께 필요)
#import <React/RCTBridgeModule.h>

@interface RCT_EXTERN_MODULE(LocationModule, NSObject)

RCT_EXTERN_METHOD(getCurrentPosition:(RCTPromiseResolveBlock)resolve
                  rejecter:(RCTPromiseRejectBlock)reject)

@end
```

**Android (Java / Kotlin)**

**Java 방식 (전통적)**

```java
// LocationModule.java
package com.myapp;

import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;
import com.facebook.react.bridge.Promise;
import com.facebook.react.bridge.WritableMap;
import com.facebook.react.bridge.Arguments;

public class LocationModule extends ReactContextBaseJavaModule {
    LocationModule(ReactApplicationContext context) {
        super(context);
    }

    @Override
    public String getName() {
        return "LocationModule";
    }

    @ReactMethod
    public void getCurrentPosition(Promise promise) {
        // WritableMap으로 변환 (JSON 직렬화를 위해)
        WritableMap result = Arguments.createMap();
        result.putDouble("latitude", 37.5665);
        result.putDouble("longitude", 126.9780);
        promise.resolve(result);
    }
}
```

**Kotlin 방식 (최신, 권장)**

```kotlin
// LocationModule.kt
package com.myapp

import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.bridge.ReactContextBaseJavaModule
import com.facebook.react.bridge.ReactMethod
import com.facebook.react.bridge.Promise
import com.facebook.react.bridge.WritableMap
import com.facebook.react.bridge.Arguments

class LocationModule(reactContext: ReactApplicationContext) :
    ReactContextBaseJavaModule(reactContext) {

    override fun getName() = "LocationModule"

    @ReactMethod
    fun getCurrentPosition(promise: Promise) {
        try {
            // WritableMap으로 변환 (JSON 직렬화를 위해)
            val result = Arguments.createMap().apply {
                putDouble("latitude", 37.5665)
                putDouble("longitude", 126.9780)
            }
            promise.resolve(result)
        } catch (e: Exception) {
            promise.reject("ERROR", e.message, e)
        }
    }
}
```

#### Turbo Module (New Architecture)

**TypeScript Spec (Codegen)**

```typescript
// NativeLocationModule.ts
import type { TurboModule } from "react-native";
import { TurboModuleRegistry } from "react-native";

export interface Spec extends TurboModule {
  // 타입 정의로 Native 코드 자동 생성
  getCurrentPositionSync(): {
    latitude: number;
    longitude: number;
  };

  getCurrentPosition(): Promise<{
    latitude: number;
    longitude: number;
  }>;

  setSpeed(speed: number): void;
}

export default TurboModuleRegistry.getEnforcing<Spec>("LocationModule");
```

**iOS - Objective-C++**

```objc
// RCTLocationModule.mm
#import "RCTLocationModule.h"
#import <CoreLocation/CoreLocation.h>
#import "NativeLocationModuleSpec.h" // Codegen이 생성

@interface RCTLocationModule () <NativeLocationModuleSpec>
@end

@implementation RCTLocationModule

RCT_EXPORT_MODULE(LocationModule);

- (std::shared_ptr<TurboModule>)getTurboModule:(const ObjCTurboModule::InitParams &)params {
  return std::make_shared<NativeLocationModuleSpecJSI>(params);
}

// 동기 함수 가능!
- (NSDictionary *)getCurrentPositionSync {
  CLLocationManager *manager = [[CLLocationManager alloc] init];
  CLLocation *location = manager.location;

  return @{
    @"latitude": @(location.coordinate.latitude),
    @"longitude": @(location.coordinate.longitude)
  };
}

- (void)getCurrentPosition:(RCTPromiseResolveBlock)resolve
                  rejecter:(RCTPromiseRejectBlock)reject {
  resolve([self getCurrentPositionSync]);
}

- (void)setSpeed:(double)speed {
  // 구현
}

@end
```

**iOS - Swift (최신, 권장) 🆕**

```swift
// LocationModule.swift
import Foundation
import CoreLocation

@objc(LocationModule)
class LocationModule: NSObject, NativeLocationModuleSpec {

    // Turbo Module 등록
    @objc
    static func moduleName() -> String {
        return "LocationModule"
    }

    @objc
    static func requiresMainQueueSetup() -> Bool {
        return false
    }

    // 동기 함수 가능!
    @objc
    func getCurrentPositionSync() -> [String: Double] {
        let manager = CLLocationManager()
        guard let location = manager.location else {
            return ["latitude": 0.0, "longitude": 0.0]
        }

        return [
            "latitude": location.coordinate.latitude,
            "longitude": location.coordinate.longitude
        ]
    }

    @objc
    func getCurrentPosition(
        _ resolve: @escaping RCTPromiseResolveBlock,
        rejecter reject: @escaping RCTPromiseRejectBlock
    ) {
        let position = getCurrentPositionSync()
        resolve(position)
    }

    @objc
    func setSpeed(_ speed: Double) {
        // 구현
        print("Speed set to: \(speed)")
    }
}
```

```objc
// LocationModule.mm (Bridge 파일 - Swift Turbo Module용)
#import <React/RCTBridgeModule.h>
#import "NativeLocationModuleSpec.h"

@interface RCT_EXTERN_REMAP_MODULE(LocationModule, LocationModule, NSObject)

RCT_EXTERN_METHOD(getCurrentPositionSync)
RCT_EXTERN_METHOD(getCurrentPosition:(RCTPromiseResolveBlock)resolve
                  rejecter:(RCTPromiseRejectBlock)reject)
RCT_EXTERN_METHOD(setSpeed:(double)speed)

@end
```

**Android - Kotlin (최신, 권장) 🆕**

```kotlin
// LocationModule.kt
package com.myapp

import android.location.LocationManager
import android.content.Context
import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.bridge.ReactMethod
import com.facebook.react.bridge.Promise
import com.facebook.react.bridge.WritableMap
import com.facebook.react.bridge.Arguments
import com.myapp.NativeLocationModuleSpec // Codegen이 생성

class LocationModule(reactContext: ReactApplicationContext) :
    NativeLocationModuleSpec(reactContext) {

    override fun getName() = "LocationModule"

    // 동기 함수 지원!
    @ReactMethod(isBlockingSynchronousMethod = true)
    override fun getCurrentPositionSync(): WritableMap {
        return Arguments.createMap().apply {
            putDouble("latitude", 37.5665)
            putDouble("longitude", 126.9780)
        }
    }

    @ReactMethod
    override fun getCurrentPosition(promise: Promise) {
        try {
            promise.resolve(getCurrentPositionSync())
        } catch (e: Exception) {
            promise.reject("ERROR", e.message, e)
        }
    }

    @ReactMethod
    override fun setSpeed(speed: Double) {
        // 구현
        println("Speed set to: $speed")
    }
}
```

**Android - Java (호환성 유지용)**

```java
// LocationModule.java
package com.myapp;

import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactMethod;
import com.facebook.react.bridge.Promise;
import com.facebook.react.bridge.WritableMap;
import com.facebook.react.bridge.Arguments;
import com.myapp.NativeLocationModuleSpec;

public class LocationModule extends NativeLocationModuleSpec {

    LocationModule(ReactApplicationContext context) {
        super(context);
    }

    @Override
    public String getName() {
        return "LocationModule";
    }

    // 동기 함수 지원!
    @ReactMethod(isBlockingSynchronousMethod = true)
    public WritableMap getCurrentPositionSync() {
        WritableMap result = Arguments.createMap();
        result.putDouble("latitude", 37.5665);
        result.putDouble("longitude", 126.9780);
        return result;
    }

    @ReactMethod
    public void getCurrentPosition(Promise promise) {
        try {
            promise.resolve(getCurrentPositionSync());
        } catch (Exception e) {
            promise.reject("ERROR", e.getMessage(), e);
        }
    }

    @ReactMethod
    public void setSpeed(double speed) {
        // 구현
        System.out.println("Speed set to: " + speed);
    }
}
```

**코드 참고:**

- [Turbo Module 예제](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/turbo-modules.md)
- [Swift Turbo Module](https://github.com/react-native-community/RNNewArchitectureLibraries)
- [Kotlin Turbo Module](https://github.com/facebook/react-native/tree/main/packages/rn-tester)
- [Codegen 설정](https://github.com/facebook/react-native/tree/main/packages/react-native-codegen)

### 2.4 렌더링 시스템 비교: Legacy Renderer vs Fabric

#### Legacy Renderer 동작 방식

```
┌─────────────────────────────────────────────┐
│     JavaScript Thread                       │
│  <View style={% raw %}{{width: 100}}{% endraw %}> → Virtual DOM      │
└──────────────┬──────────────────────────────┘
               │ [1. JSON 직렬화]
               ▼
        ┌─────────────┐
        │   Bridge    │
        └──────┬──────┘
               │ [2. 메시지 큐]
               ▼
┌──────────────────────────────────────────────┐
│     Shadow Thread (Yoga Layout)             │
│  - Flexbox 계산                              │
│  - 위치/크기 결정                            │
└──────────────┬───────────────────────────────┘
               │ [3. JSON 직렬화]
               ▼
        ┌─────────────┐
        │   Bridge    │
        └──────┬──────┘
               │ [4. 메시지 큐]
               ▼
┌──────────────────────────────────────────────┐
│     Main Thread (Native UI)                 │
│  - UIView (iOS) / View (Android) 생성        │
│  - 실제 렌더링                               │
└──────────────────────────────────────────────┘

평균 렌더링 시간: 31ms (목표: 16ms/60fps)
→ 프레임 드롭 발생 ⚠️
```

**문제점:**

- Bridge를 2번 통과 (JS → Shadow → Native)
- 각 단계마다 JSON 직렬화/역직렬화
- 우선순위 없음 (모든 업데이트 동일 처리)
- 동기 레이아웃 측정 불가

#### Fabric Renderer 동작 방식

```
┌─────────────────────────────────────────────┐
│     JavaScript Thread (JSI)                 │
│  <View style={% raw %}{{width: 100}}{% endraw %}> → React Tree        │
└──────────────┬──────────────────────────────┘
               │ [JSI Direct Call]
               ▼
┌──────────────────────────────────────────────┐
│     C++ Fabric Core                         │
│  ┌────────────────┐  ┌──────────────────┐   │
│  │ Shadow Tree    │  │ Priority Queue   │   │
│  │ (Yoga Layout)  │  │ (High/Low)       │   │
│  └────────┬───────┘  └──────────────────┘   │
└───────────┼──────────────────────────────────┘
            │ [Direct Memory Access]
            ▼
┌──────────────────────────────────────────────┐
│     Main Thread (Native UI)                 │
│  - 최소한의 변경만 적용                      │
│  - Batch Updates                            │
└──────────────────────────────────────────────┘

평균 렌더링 시간: 25ms
+ 우선순위 렌더링으로 체감 성능 ↑↑
```

**개선점:**

- ✅ Bridge 제거 → JSI 직접 통신
- ✅ 우선순위 기반 렌더링 (사용자 인터랙션 우선)
- ✅ 동기 레이아웃 측정 가능
- ✅ Concurrent Rendering 지원

#### 코드 비교: 레이아웃 측정

**Legacy: 비동기만 가능**

```javascript
import { UIManager, findNodeHandle } from "react-native";

const viewRef = useRef(null);

// 비동기 콜백 필요
UIManager.measure(findNodeHandle(viewRef.current), (x, y, width, height) => {
  console.log("Width:", width);
  // 여기서만 값 사용 가능
});

// 즉시 사용 불가 ❌
// const width = ???
```

**Fabric: 동기 가능**

```javascript
import { useRef } from "react";

const viewRef = useRef(null);

// 동기로 즉시 측정
const layout = viewRef.current?.measureInWindow();
console.log("Width:", layout.width); // 즉시 사용 가능 ✅

// 또는 비동기도 가능
viewRef.current?.measureInWindow((x, y, width, height) => {
  console.log("Width:", width);
});
```

#### Fabric Component 예시

**TypeScript Spec**

```typescript
// MyCardNativeComponent.ts
import type { ViewProps } from "react-native";
import type { HostComponent } from "react-native";
import codegenNativeComponent from "react-native/Libraries/Utilities/codegenNativeComponent";

interface NativeProps extends ViewProps {
  title: string;
  subtitle?: string;
  elevation: number;
  onCardPress?: () => void;
}

export default codegenNativeComponent<NativeProps>(
  "MyCard"
) as HostComponent<NativeProps>;
```

**iOS 구현**

```swift
// MyCardView.swift
import UIKit

@objc(MyCardView)
class MyCardView: UIView {

    @objc var title: NSString = "" {
        didSet { updateUI() }
    }

    @objc var subtitle: NSString = "" {
        didSet { updateUI() }
    }

    @objc var elevation: NSNumber = 0 {
        didSet {
            layer.shadowRadius = CGFloat(truncating: elevation)
            layer.shadowOpacity = 0.3
        }
    }

    @objc var onCardPress: RCTDirectEventBlock?

    private let titleLabel = UILabel()
    private let subtitleLabel = UILabel()

    override init(frame: CGRect) {
        super.init(frame: frame)
        setupUI()
    }

    private func setupUI() {
        backgroundColor = .white
        layer.cornerRadius = 12

        addSubview(titleLabel)
        addSubview(subtitleLabel)

        let tapGesture = UITapGestureRecognizer(
            target: self,
            action: #selector(handleTap)
        )
        addGestureRecognizer(tapGesture)
    }

    private func updateUI() {
        titleLabel.text = title as String
        subtitleLabel.text = subtitle as String
    }

    @objc func handleTap() {
        onCardPress?([:])
    }
}
```

**Android 구현**

```kotlin
// MyCardView.kt
package com.myapp

import android.content.Context
import android.widget.LinearLayout
import android.widget.TextView
import com.facebook.react.bridge.Arguments
import com.facebook.react.uimanager.ThemedReactContext
import com.facebook.react.uimanager.events.RCTEventEmitter

class MyCardView(context: Context) : LinearLayout(context) {

    private var reactContext: ThemedReactContext? = null
    private val titleView = TextView(context)
    private val subtitleView = TextView(context)

    var title: String = ""
        set(value) {
            field = value
            titleView.text = value
        }

    var subtitle: String = ""
        set(value) {
            field = value
            subtitleView.text = value
        }

    var elevation: Float = 0f
        set(value) {
            field = value
            this.elevation = value
        }

    init {
        orientation = VERTICAL
        addView(titleView)
        addView(subtitleView)

        setOnClickListener {
            reactContext?.getJSModule(RCTEventEmitter::class.java)
                ?.receiveEvent(id, "onCardPress", Arguments.createMap())
        }
    }
}
```

**JavaScript 사용**

```javascript
import MyCard from './MyCardNativeComponent';

function App() {
  return (
    <MyCard
      title="Hello Fabric"
      subtitle="New Architecture"
      elevation={8}
      onCardPress={() => console.log('Card pressed!')}
      style={% raw %}{{ margin: 16 }}{% endraw %}
    />
  );
}
```

**코드 참고:**

- [Fabric Renderer 구현](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/react/renderer)
- [Fabric Component 예제](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/fabric-native-components.md)
- [Yoga Layout Engine](https://github.com/facebook/yoga)

---

## 3. New Architecture 고급 기능

### 3.1 React Concurrent 기능 지원

New Architecture는 React의 Concurrent 기능을 완벽히 지원합니다.

#### Concurrent Features

```javascript
import { useTransition, Suspense } from "react";

function App() {
  const [isPending, startTransition] = useTransition();
  const [searchTerm, setSearchTerm] = useState("");

  const handleSearch = (text) => {
    // 낮은 우선순위 업데이트
    startTransition(() => {
      setSearchTerm(text);
    });
  };

  return (
    <View>
      <TextInput onChangeText={handleSearch} />
      {isPending && <ActivityIndicator />}
      <Suspense fallback={<Loading />}>
        <SearchResults query={searchTerm} />
      </Suspense>
    </View>
  );
}
```

#### Automatic Batching

```javascript
// Legacy Architecture: 3번 렌더링
function handleClick() {
  setCount((c) => c + 1); // Re-render
  setFlag((f) => !f); // Re-render
  setText("Updated"); // Re-render
}

// New Architecture: 1번만 렌더링 (자동 배칭)
function handleClick() {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  setText("Updated");
  // → 한 번에 처리!
}
```

### 3.2 동기 레이아웃 측정

#### Legacy의 문제점

```javascript
// Legacy: 깜빡임 발생
function Tooltip() {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  return (
    <View
      onLayout={(e) => {
        // ❌ 비동기로 실행 → 화면에 먼저 그려진 후 위치 변경
        setPosition({
          x: e.nativeEvent.layout.x,
          y: e.nativeEvent.layout.y,
        });
      }}
    >
      <Text>Tooltip</Text>
    </View>
  );
}
```

#### New Architecture 해결

```javascript
import { useLayoutEffect, useRef } from 'react';

function Tooltip() {
  const viewRef = useRef(null);
  const [position, setPosition] = useState({ x: 0, y: 0 });

  // ✅ 동기 실행 → 화면에 그리기 전에 위치 결정
  useLayoutEffect(() => {
    if (viewRef.current) {
      viewRef.current.measure((x, y, width, height) => {
        setPosition({ x, y });
      });
    }
  }, []);

  return (
    <View ref={viewRef} style={% raw %}{{ left: position.x, top: position.y }}{% endraw %}>
      <Text>Tooltip</Text>
    </View>
  );
}
```

### 3.3 DOM Node APIs (React Native 0.82+)

React Native 0.82부터 DOM 스타일의 Node API를 지원합니다.

```javascript
// 새로운 DOM 스타일 API
import { findNodeHandle } from "react-native";

function MyComponent() {
  const viewRef = useRef(null);

  const inspectNode = () => {
    const node = findNodeHandle(viewRef.current);

    // DOM 트리 탐색
    const parent = node.parentNode;
    const children = node.childNodes;
    const firstChild = node.firstChild;

    // 스타일 정보
    const styles = node.computedStyle;

    // 레이아웃 정보
    const bounds = node.getBoundingClientRect();
    console.log("Position:", bounds.x, bounds.y);
    console.log("Size:", bounds.width, bounds.height);
  };

  return <View ref={viewRef}>...</View>;
}
```

---

## 4. 참고 자료

### 4.1 공식 문서

**New Architecture**
- [Architecture Overview](https://reactnative.dev/architecture/overview)
- [About the New Architecture](https://reactnative.dev/architecture/landing-page)

**JSI (JavaScript Interface)**
- [JSI 소스 코드](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/jsi)
- [JSI API 레퍼런스](https://github.com/facebook/react-native/blob/main/packages/react-native/ReactCommon/jsi/jsi/jsi.h)

**Turbo Modules**
- [Turbo Modules 생성 가이드](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/turbo-modules.md)
- [Turbo Module 소스 코드](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/react/nativemodule/core)
- [TurboModuleRegistry](https://github.com/facebook/react-native/blob/main/packages/react-native/Libraries/TurboModule/TurboModuleRegistry.js)

**Fabric Renderer**
- [Fabric Component 생성 가이드](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/fabric-native-components.md)
- [Fabric Renderer 소스 코드](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/react/renderer)
- [Fabric Components 소스 코드](https://github.com/facebook/react-native/tree/main/packages/react-native/ReactCommon/react/renderer/components)
- [Yoga Layout Engine](https://github.com/facebook/yoga)

**Codegen**
- [Codegen 소스 코드](https://github.com/facebook/react-native/tree/main/packages/react-native-codegen)


### 4.2 커뮤니티 & 학습 리소스

**Working Group**
- [New Architecture Working Group](https://github.com/reactwg/react-native-new-architecture)
- [Working Group Discussions](https://github.com/reactwg/react-native-new-architecture/discussions)

**예제 프로젝트**
- [React Native 테스트 앱 (RNTester)](https://github.com/facebook/react-native/tree/main/packages/rn-tester)

**라이브러리 호환성**
- [React Native Directory - New Architecture 필터](https://reactnative.directory/?newArchitecture=true)
