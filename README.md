# Some 3D Software
A certain app for resin printing is available in free and premium versions - the premium version is reserved for paying users and has some additional features. Users can try these features with a trial license valid for a limited number of days. Users must create an account with a valid email address, and use it to sign in before being able to use the app.

## Objectives
1-Have all features available to a non paying user.

## Behaviour
On start up, users must first log in, then they must provide a license number.
The logic that determines whether it is possible to use the premium version of the app involves working with/comparing two global variables where the value of the first one comes from network operations while the value of the second one comes from, at least partially, a local file. This local file contains two items, one of which identifies the user's hardware. The file contents are decrypted, decoded, and finally hashed before being compared to the data coming from the net.

The file is stored in "C:\Users\xxx\AppData\Local\Some3DSoftware".

If one speculates, one might think that when a user enters a license number, the app sends to the net not only the serial number but also the user's account and hardware id; while the server, after sucessfull validations, registers the serial number to the account and hardware id; then, the app encrypts and stores the hardware id and serial number in the local file. Subsequently, when the user starts up the app, the app sends the account (and possibly the hardware id) to some server and gets back a hash of the associated hardware id and serial number, then it decrypts the local file and compares the hashes: if they match, the app loads.

This speculated flow does not cover certain license validations nor the handling of hardware identifier changes, but the essence of the actual license validation process is probably not that much different. The app uses symmetric encryption when encrypting the local file (two consecutive xor operations).

## Work
1-Patch the code that works with the two hashes to have it report a satisfactory result: this makes the rest of the app believe validation was sucessfull, and premium version features become available without even the need for the user to log in.

2-Add outbound firewall rules to the binaries to prevent them from communicating with the internet.
