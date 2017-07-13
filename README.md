# Web Share API

**Written**: 2016-06-08, **Updated**: 2017-07-13

**Web Share** is a Web platform API for sharing text, URLs and images to an
arbitrary destination of the user's choice:

```js
navigator.share({title: 'Example Page', url: 'https://example.com'});
```

## Implementation status

* The API is shipping in Google Chrome 61 on Android (images are not supported).

## Links

* [Explainer document](docs/explainer.md), a high-level overview of the proposal.
* [Specification](https://wicg.github.io/web-share/).
* [Native integration survey](docs/native.md), for platform-specific matters.
* [Web platform
  tests](https://github.com/w3c/web-platform-tests/tree/master/web-share), for
  automatic and manual testing against an implementation.

This is a product of the [Ballista
project](https://github.com/chromium/ballista), which aims to explore
website-to-website and website-to-native interoperability.

## Licensing and contributions

See [LICENSE](LICENSE.md) and [CONTRIBUTING](CONTRIBUTING.md).
