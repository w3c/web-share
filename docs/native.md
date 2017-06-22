# Web Share API: Native Integration Survey

**Date**: 2016-06-06 (authored), 2017-06-22 (updated)

This document is an informal and incomplete survey of various operating systems'
sharing systems, for exploring how a user agent might automatically map the
[Share API](explainer.md) into the native system.

*Note:* I (mgiuca@chromium.org) am not very familiar with these details. I
gathered this information from reading the online documentation and
experimenting with apps, and have not had experience programming against these
APIs. I would appreciate being informed of any errors.

## Android

* Apps can send *intents* to the system which can be delivered to an app of the
  user's choice.
* It is [pretty
  straightforward](http://developer.android.com/training/sharing/send.html) to
  send an
  [`ACTION_SEND`](http://developer.android.com/reference/android/content/Intent.html#ACTION_SEND)
  intent. Use the system intent picker to choose a target app.

## iOS

* Apps can send a share event to the system. The dialog for picking a share
  receiver is the "Share sheet".
* Unclear what the data format of the share object is, but it at least allows
  sharing of text, URLs and images.
* The [UIActivityViewController
  class](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIActivityViewController_Class/index.html)
  triggers the share dialog and you can pass a number of data items to it. The
  docs are very hard to understand; this [StackOverflow
  answer](http://stackoverflow.com/a/13499204/368821) explains it succinctly.
  You can share an array of items that implement the
  [UIActivityItemSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIActivityItemSource_protocol/index.html)
  protocol.

## macOS

* [NSSharingService](https://developer.apple.com/documentation/appkit/nssharingservice)
  allows apps to trigger a share dialog, with a similar API to iOS.
* Available in macOS 10.8 and up.

## Windows

* Apps can initiate a "Share contract" to share to another Windows app. UX:
  Shows a modal share target picker on the right hand side of the screen.
* [Share
  API](https://msdn.microsoft.com/en-us/windows/uwp/app-to-app/share-data):
  Create a DataRequest object and put data into it (multiple data types can be
  placed into the request object). Can optionally negotiate with the receiver
  about which data to send (Share API implementations would likely ignore this
  and just put all the data from the requester into the object).
* Available in Windows 10 and up.
* Available only to [Universal Windows Platform
  (UWP)](https://msdn.microsoft.com/en-us/windows/uwp/get-started/whats-a-uwp)
  apps. This API is **not** available to normal Win32 .exe applications. This
  will only be available to user agents that are UWP apps (e.g., Microsoft Edge
  is, Google Chrome isn't).

## Others

* No known native share mechanism on Linux (Desktop), Chrome OS.
