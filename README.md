# WebRTC ShareSheet Custom Scheme Signalling Channel

In this repository, I describe an idea on how to use the iOS 16 / macOS Ventura
`ShareLink` SwiftUI component for bringing up the Share Sheet in combination
with AirDrop to exchange custom scheme URIs containing offer & answer SDP with
built-in non-trickle ICE candidates in them.

In this example, macOS is the offerer and iOS is the answerer.

1. Offerer creates the offer SDP
2. Offerer collects ICE candidates
3. Offerer builds a custom scheme URI containing the offer and the candidates
4. Offerer presents Share Sheet with the iOS answerer custom scheme URI
5. User uses AirDrop to drop the offer onto iOS answerer
6. Answerer receives the offer SDP with ICE candidates
7. Answerer creates the answer SDP
8. Answerer collects ICE candidates
9. Answerer builds a custom scheme URI containing the answer and the candidates
10. Answerer presents Share Sheet with the macOS offerer custom scheme URI
11. User uses AirDrop to drop the answer onto macOS offerer
12. Offerer receives the answer SDP with ICE candidates

After these 10 programmatic and 2 manual steps, the WebRTC connection gets
established.

This is meant for local network peer to peer connection establishment, even
though technically the collected candidates can be of type that allows peer
connection across networks.

The main drawback of this approach is that ICE cannot be renegotiated after
connectivity loss without the devices being in close proximity physically,
because of AirDrop operating over Wi-Fi.

My application is two distinct applications, one for iOS and one for macOS, each
with its own local storage (using `FileManager`) syncing every now and then to
keep their data in sync.

The synchronization is something that doesn't have to be happening continuously,
instead, synchronization is carried out manually whenever transferring from one
device to another and wanting to continue working on the data originating from
the other device.

To work, this will need iOS 16 and macOS Ventura.
Both of these OSs release on 2022-10-22.

These OSs ship with SwiftUI component named `ShareLink` that makes it easier to
present the Share Sheet with a custom URI than the current option which is too
complex to bother with it.

Once I upgrade my iPhone to iOS 16, my Mac to macOS Ventura and install XCode 14
to get the SwiftUI version with `ShareLink`, I will attempt to implement this
and will capture the steps here if successful.
