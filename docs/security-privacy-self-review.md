

Responses to the [Self-Review Questionnaire: Security and Privacy](https://w3ctag.github.io/security-questionnaire/) for the [Web Share API](https://w3c.github.io/web-share/)


## 2.1 What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

Web Share enables data to be sent from web sites to native applications or potentially other installed web applications. It explicitly does not allow the web site to learn which share targets are available.

The API returns a rejected promise if no share targets are available, but also returns a rejected promise of the same type if the user cancels the share action. This intentionally does not reveal the state where no share targets are available.

## 2.2 Is this specification exposing the minimum amount of information necessary to power the feature?

The API allows testing whether content can be shared via `canShare()`; although this reveals information, it is important to users because it allow developers to not present a sharing option if the operation would immediately fail.

The `share()` API returns three states: content not supported (via `TypeError`), no sharing was attempted (via `AbortError`), sharing was attempted but failed (also via `AbortError`) or sharing was successful (no error). The first state reveals no more information than `canShare()`; the remaining states are necessary to inform the user of the result of the action.

For both `canShare()` and `share()`, revealing whether the specified content can be shared must be limited to the broad category of whether files can be shared or not (if specified) and whether the input url is parseable (if specified); implementations must not distinguish support for specific files or file types either by the platform or available share targets.

Implementors will want to carefully consider what information is revealed in the error message when `share()` is rejected. Even distinguishing between the case where no targets are available and user cancellation could reveal information about which apps are installed on the user's device. The previous version of the specification distinguished when sharing failed via `DataError` instead, which may have exposed unnecessary information.

## 2.3 How does this specification deal with personal information or personally-identifiable information or information derived thereof?

Any information shared was already exposed to the web site; no further personal information is revealed by the API to the web site.

The receiving share target selected by the user can be given arbitrary information by the web site via the `ShareData`'s properties. In some expected user interfaces, the user is not made aware of what data is being shared by the web site with the target. This could lead to a web site in posession of personal information sharing it with a target without the user's knowledge. One mitigation would be to have the user interface allow inspection of the data.

## 2.4 How does this specification deal with sensitive information?

As in the previous question, the specification does not limit what data is shared, and the data could include sensitive information already revealed to the web site. However, the user is in control of which, if any, target application will receive the information.

## 2.5 Does this specification introduce new state for an origin that persists across browsing sessions?

No.

## 2.6 What information from the underlying platform, e.g. configuration data, is exposed by this specification to an origin?

The presence of the API may reveal characteristics of the user's device beyond the user agent. For example, the user agent may only expose the `navigator.share()` API on "mobile" devices, revealing the type of device the user agent is executing on.

Revealing specific content types supported via `files` must not be done, as `canShare()` does not require
[transient activation](https://html.spec.whatwg.org/multipage/interaction.html#transient-activation). For example, if the `canShare()` API response was conditional on what native applications were installed on the device, this could be used to fingerprint the user.

## 2.7 Does this specification allow an origin access to sensors on a user’s device

No.

## 2.8 What data does this specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.

The data exposed to an origin is the presence of the API itself, whether sharing of files in general is supported by the user agent and/or platform, and the results of interacting with the API (error or success).

## 2.9 Does this specification enable new script execution/loading mechanisms?

No.

## 2.10 Does this specification allow an origin to access other devices?

No.

## 2.11 Does this specification allow an origin some measure of control over a user agent’s native UI?

The API provides for showing a target selection user interface, for example as a modal dialog on some platforms, or a context menu in other platforms. The presence and presentation of the interface remains entirely under the control of the user agent.

## 2.12 What temporary identifiers might this this specification create or expose to the web?

None.

## 2.13 How does this specification distinguish between behavior in first-party and third-party contexts?

First-party and third-party contexts are not distinguished. However, the working group is exploring limiting this API to first-party contexts and only allow the API to be exposed to third party contexts via feature policy. See [issue 51](https://github.com/w3c/web-share/issues/151) 

The use of the `share()` API is gated on [transient activation](https://html.spec.whatwg.org/multipage/interaction.html#transient-activation) and thus requires user interaction (or the equivalent) within a third-party context to be used.

## 2.14 How does this specification work in the context of a user agent’s Private Browsing or "incognito" mode?

Use of `share()` from a private browsing mode might leak private data to a third-party application that does not respect the user's privacy setting. User agents could present additional warnings or disable the feature entirely when in a private browsing mode, but this is not mandated as the chooser UI could be considered sufficient warning.

## 2.15 Does this specification have a "Security Considerations" and "Privacy Considerations" section?

Yes.

## 2.16 Does this specification allow downgrading default security characteristics?

No.

## 2.17 What should this questionnaire have asked?

Exfiltration of data already exposed from the web site to contexts beyond the control of the user agent - in this case, native applications - could be called out, with potential risks and mitigations enumerated.
