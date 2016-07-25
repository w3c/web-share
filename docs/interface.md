# Web Share API Interface

**Date**: 2016-05-30

This document is a rough spec (i.e., *not* a formal web standard draft) of the
Web Share API. The basic Share API only allows share requests to be sent (this
API does not provide the capability to receive share requests). For a follow-up
plan to have websites receive share requests from the system, or other websites,
see the [Share Target API](https://github.com/mgiuca/web-share-target).

Examples of using the Share API for sharing can be seen in the
[explainer document](explainer.md).

**Note**: The Web Share API is the first concrete proposal of the [Ballista
project](https://github.com/chromium/ballista), which aims to explore
website-to-website and website-to-native interoperability.

## navigator.share

The `navigator.share` function (available from both foreground pages and
workers) is the main method of the interface:

```WebIDL
partial interface Navigator {
  [SecureContext] Promise<void> share(ShareData data);
};

partial interface WorkerNavigator {
  [SecureContext] Promise<void> share(ShareData data);
};

dictionary ShareData {
  DOMString? title;
  DOMString? text;
  DOMString? url;
};
```

The `share` method takes one argument: the object that will be delivered to the
handler. It contains the data being shared between applications.

The `data` object may have any of (and should have at least one of) the
following optional fields:

* `title` (string): The title of the document being shared. May be ignored by
  the handler.
* `text` (string): Arbitrary text that forms the body of the message being
  shared.
* `url` (string): A URL or URI referring to a resource being shared.

**TODO**: Expand this to allow image data and/or file blobs.

`share` always shows some form of UI, to give the user a choice of application
and get their approval to invoke and send data to a potentially native
application (which carries a security risk). UX mocks are shown
[here](explainer.md#user-flow).

`share`'s promise is resolved if the user chooses a target application,
and that application accepts the data without error. The promise may be rejected
in the following cases (it is possible to distinguish between these four failure
modes, but not learn the identity of the chosen application):

* The share was invalid (e.g., inappropriate fields in the `data` parameter).
* There were no apps available to handle sharing.
* The user cancelled the picker dialog instead of picking an app.
* The data could not be delivered to the target app (e.g., the chosen app could
  not be launched), or the target app explicitly rejected the share event.

In a user agent that will never provide any share targets (e.g., on a platform
that does not support sharing) `navigator.share` SHOULD be `undefined`, so that
sites can use feature detection to avoid showing sharing UI.

## Share handlers

The list of share targets or handlers can be populated from a variety of
sources, depending on the user agent and underlying OS:

* Built-in service (e.g., "copy to clipboard").
* Native applications.
* Web applications registered using the [Web Share Target
  API](https://github.com/mgiuca/web-share-target).

The user agent can support any or all of the above (for example, on some
platforms, there is no system for native apps to receive share data; some user
agents may not support the Share Target API).

The user agent may either present its own picker UI and then forward the share
data to the chosen app, or simply forward the share data to the system's native
app picking system (e.g., Android, iOS and Windows 10 all [support this concept
natively](native.md)) and let the OS do the work.

When forwarding to a website using the Share Target API, the `ShareData` object
is simply cloned. When forwarding to a native app, the user agent should do its
best to map the fields onto the equivalent concepts. For example, on Android,
when a share is sent to a native application, the user agent may create an
[Intent](http://developer.android.com/reference/android/content/Intent.html)
object with `ACTION_SEND`, setting the `EXTRA_SUBJECT` to `title`. Since Android
intents do not have a URL field, `EXTRA_TEXT` would be set to `text` and `url`
concatenated together.

## Security considerations

Implementations should observe the following security and privacy advice.

Web Share enables data to be sent from websites to native applications. While
this ability is not unique to Web Share, it does come with a number of potential
security issues that may vary in severity (depending on the underlying
platform).

* User agents MUST NOT allow the website to learn which apps are installed, or
  which app was chosen from `navigator.share`. This information could be used
  for fingerprinting, as well as leaking details about the user's device.
* On every call to `navigator.share`, the user MUST be presented with a dialog
  asking them to select a target application (even if there is only one possible
  target). This surface serves as a security confirmation, ensuring that
  websites cannot silently send data to native applications.
* Due to the capabilities of the API surface, `navigator.share` is restricted to
  secure browsing contexts (such as `https://` schemes).
* Use of `navigator.share` from a [private browsing
  mode](https://en.wikipedia.org/wiki/Privacy_mode) may leak private data to a
  third-party application that does not respect the user's privacy setting.
  User agents should consider presenting additional warnings or disabling the
  feature entirely when in a private browsing mode.
* The data passed to `navigator.share` may be used to exploit buffer overflow
  or other remote code execution vulnerabilities in native applications that
  receive shares. There is no general way to guard against this, but
  implementors should be aware that it is a possibility. User agents SHOULD NOT
  require target applications to opt in to receiving Web Shares (as that would
  severely limit the usefulness of Web Share).
