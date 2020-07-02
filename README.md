# PI　第三方　APP
Overview
One of the core principles at Pi is to create simple user experience through progressive decentralization, which also applies to how we facilitate developers to build Pi Apps. This is the main reason for adopting industry standard technologies, such as JavaScript, HTML, CSS, and iframes, for developers to integrate with Pi, so that they can focus more on building awesome applications rather than getting complex technologies to work. Pi 3rd-party apps will initially be hitting Pi’s backend so that they can iterate on their app designs and establish their use cases and audiences (user bases). Once they are ready and the blockchain becomes live, their backend will be able to adapt to hit the Pi Testnet or Mainnet directly, eventually achieving decentralization. This ensures the development of utility and decentralization in parallel rather than in sequence, just like Pi Network itself.

Pi Apps are implemented as iframes that the Pi Network mobile app can load dynamically and display them to Pioneers in mobile webviews. The app’s frontend can instantiate the Pi JavaScript SDK for accomplishing Pi related activities, such as requesting Pi transfers. Technically, the Pi API allows developers to develop their applications in whichever programming language they prefer in their backend and integrate with the Pi platform interface through the Javascript SDK in the frontend. This release contains the frontend Javascript SDK.

It is actually possible for developers to make simple apps that are completely serverless (e.g. JavaScript games) with only static file hosting. More complex apps need servers. Later SDK versions will include endpoints that allow more diverse ways of Pi transfers on the platform, e.g. from an app to a Pioneer, and endpoints that improve the user experience, such as to send push notifications, interact with chat channels, or allow users to invite their friends to join a Pi app.

SDK Features
Authenticate user through Pi
Request transfer from users directly inside your app
Installing
Import the SDK in your index.html

<script src="https://downloads.minepi.com/sdk/v1/prod.js"></script>
You can create the Pi Network client in your app this way: <br/>

In Javascript

const PiNetworkClient = window.PiNetwork;
In Typescript

const PiNetworkClient: PiNetworkInstance = window.PiNetwork;
Usage
Authenticate the current user
In Javascript

try {
  const user = await PiNetworkClient.Authenticate()
  console.log(`Hello ${user.username}`)
} catch (err) {
  // Not able to fetch the user
}
In Typescript

try {
  const user: User = await PiNetworkClient.Authenticate()
  console.log(`Hello ${user.username}`)
} catch (err) {
  // Not able to fetch the user
}
Request a transfer
In your transfer request, a transfer of Pi from a Pioneer’s account will not be completed until the Pioneer explicitly confirms the transfer with Pi Apps Platform.

From your javascript code, you can request the user to send you a Pi transfer. A transfer request will be created from the current user executing the code to your app

In Javascript

try {
  const transferRequest = await PiNetworkClient.RequestTransfer(3.14, "Demo transfer request")
} catch(err) {
  // Technical problem (eg network failure). Please try again
}
In Typescript

try {
  const transferRequest: TransferRequest = await PiNetworkClient.RequestTransfer(3.14, "Demo transfer request")
} catch(err) {
  // Technical problem (eg network failure). Please try again
}
Calling this function will trigger a modal in the frontend in order to ask a confirmation to the user.

For privacy reason, you cannot get the reason of the transfer failure but it can be caused by:

The user not being allowed to perform transfers yet
A lack of funds
The user denying the transfer from the modal
Transfer request status
The transfer request promise resolving does not mean that the transfer succeeded.

You can check the transferRequest.status in order to get transfer status

transferRequest.status
This attribute can have three values: "succeeded", "failed", "requested".

requested: this is the initial state before a transfer request is accepted/rejected by the Pioneer.
succeeded: this means that the transfer request was accepted by the Pioneer and it the requested amount of Pi has been successfuly deposited into the app's wallet.
failed: this is the state when the transfer has failed. A transfer can fail for multiple reasons as described above.
