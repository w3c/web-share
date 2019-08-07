# Web Share API

**[Web Share](https://w3c.github.io/web-share/)** is a Web platform API for sharing text, URLs and images to an
arbitrary destination of the user's choice:

```js
navigator.share({title: 'Example Page', url: 'https://example.com'});
```

## Implementation status

* The Level 1 API (images/files not supported) is shipping in:
  * Google Chrome 61 on Android.
  * [Safari 66](https://developer.apple.com/safari/technology-preview/release-notes/#r66).
* The Level 2 API is currently [being implemented](https://www.chromestatus.com/feature/4777349178458112) in Chrome.

## Links

* [Explainer document](docs/explainer.md), a high-level overview of the proposal.
* [Specification - Level 1](https://w3c.github.io/web-share/).
* [Specification - Level 2](https://w3c.github.io/web-share/level-2/).
* [Native integration survey](docs/native.md), for platform-specific matters.
* [Web platform
  tests](https://github.com/web-platform-tests/wpt/tree/master/web-share), for
  automatic and manual testing against an implementation.

This is a product of the [Ballista
project](https://github.com/chromium/ballista), which aims to explore
website-to-website and website-to-native interoperability.

## Licensing and contributions

See [LICENSE](LICENSE.md) and [CONTRIBUTING](CONTRIBUTING.md).
