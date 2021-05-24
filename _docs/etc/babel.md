---
layout: default
title: Babel
parent: 기타
---

- babel 설정 파일에서 console.log를 개발자 모드환경에서만 동작하도록 할 수 있다.

```js
module.exports = function(api) {
	const env = api.env();
	let plugins = [];

	if (env === 'production') {
		plugins.push.apply(plugins, ['transform-remove-console']);
	}

	return {
		plugins
	};
};
```