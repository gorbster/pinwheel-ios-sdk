# PinwheelSDK

## Usage

The Pinwheel iOS SDK's main interface is a UIViewController that you can integrate into your app as you would any UIViewController, e.g. presented as a modal, or used with a UINavigationController. Additionally, you can implement the `PinwheelDelegate` protocol to receive events throughout the `PinwheelViewController`'s lifecycle.

### Link Token

To initialize the `PinwheelViewController`, a short-lived Link token will need to be generated first. Your server can generate the Link token by sending a POST request to the `/v1/link_tokens` endpoint. Your mobile app should fetch the link token from your server. DO NOT ever send this request from the client side and publicly expose your api_secret.

The link token returned is valid for 15 minutes, after which it expires and can no longer be used to initialize the `PinwheelViewController`. The expiration time is returned as a unix timestamp.

### PinwheelViewController

The PinwheelViewController is a `UIViewController` that you can integrate into your app's flow like so:

```swift
import PinwheelSDK

let pinwheelVC = PinwheelViewController(token: linkToken, delegate: self)
self.present(pinwheelVC!, animated: true)
```

With the `PinwheelViewController`, end-users can select their employer, authenticate with their payroll platform login credentials, and authorize the direct deposit change. Throughout the authorization process, events will be emitted to the `onEvent` callback. Upon a successful authorization, the `onSuccess` callback will be called. `onExit` will be called when it is time to close the dialog, and you should remove the PinwheelLink component from your view hierarchy.

## PinwheelDelegate

### `onSuccess(_ event: PinwheelSuccessEvent)`

Callback whenever a user completes a Link flow successfully. Note: This is simply a front end callback only. If a user begins a job, closes the app, and the job completes successfully this callback will not be called.

### `onExit(_ event: PinwheelExitEvent)`

Callback whenever a user exits the modal either explicitly or if an error occurred that crashed the modal. Error codes can be seen [here](https://docs.getpinwheel.com/link/index.html#errors).

### `onEvent(_ event: PinwheelActionEvent)`

Callback whenever a user interacts with the modal (e.g. clicks something or types something). The [eventName](https://docs.getpinwheel.com/link/index.html#events) can be used to gain insight into what the user is doing.

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first. Then, add your API secret to the top of the LinkToken.swift file. In your app, you should fetch the Link token from your server, and you should never include your API secret in your app.

## Installation

PinwheelSDK is available using [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'PinwheelSDK'
```

## Author

[Pinwheel](https://getpinwheel.com)

## License

PinwheelSDK is available under the MIT license. See the LICENSE file for more info.
