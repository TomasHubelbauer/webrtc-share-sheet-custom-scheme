# WebRTC ShareSheet Custom Scheme Signaling Channel

In this repository, I describe an idea on how to establish a peer connection via
WebRTC between an iOS app and a macOS app both running a web app in a WKWebView.

The steps to establish the connection are:

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

In this example, macOS is the offerer and iOS is the answerer.

After these 10 programmatic and 2 manual steps, the WebRTC connection should get
established.

## Disclaimer

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

## `ShareLink`

At first, I though I'd use the new SwiftUI `ShareLink` component to present the
Share Sheet on both macOS and iOS.
However, this component only seems to be present in SwiftUI for iOS, not macOS.

## `navigator.share`

The Web Share API, `navigator.share`, can be used to call up the Share Sheet,
but it will only allow sharing HTTP and HTTS URLs, not arbitrary custom scheme
URLs.

## Next steps

I could look into invoking the Share Sheet from Swift but without `ShareLink`
which is going to be cumbersome.

I could also continue looking for ways to allow `navigator.share` to work while
not in a secure context, but this might be impossible.

I could use a custom scheme with WKWebView which seems to work around `file:`
not being treated as secure context:
https://stackoverflow.com/a/70009737/2715716
