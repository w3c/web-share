# Web Share API

**[Web Share](https://w3c.github.io/web-share/)** is a Web platform API for sharing text, URLs and images to an
arbitrary destination of the user's choice:

```js
navigator.share({title: 'Example Page', url: 'https://example.com'});
```

## Implementation status

The API is shipping in [numerous browsers](https://caniuse.com/web-share).

## Links and Historical

* [Explainer document](docs/explainer.md), a high-level overview of the proposal.
* [Specification](https://w3c.github.io/web-share/).
* [Native integration survey](docs/native.md), for platform-specific matters.
* [Web platform
  tests](https://github.com/web-platform-tests/wpt/tree/master/web-share), for
  automatic and manual testing against an implementation.

This API was derived from Chromium's [Ballista
project](https://github.com/chromium/ballista), which explored
website-to-website and website-to-native interoperability.

## Licensing and contributions

See [LICENSE](LICENSE.md) and [CONTRIBUTING](CONTRIBUTING.md).
