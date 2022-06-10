# Admin Round Three

Let's return to setting a message and see what's different.

[`raen.dev/admin/#/advanced.statusmessage.raendev.testnet/SetStatus`](https://raen.dev/admin/#/advanced.statusmessage.raendev.testnet/SetStatus)

Here is where raen really raen really shines.

The method's documentation and the types documentation are available.  You can think of this as interactive documentation.

## Getting Status

Next since we are using `AccountId` the type is available to the form. Meaning the the form can validate an account id before submitting it, preventing the call from failing on the NEAR node:

[`raen.dev/admin/#/advanced.statusmessage.raendev.testnet/SetStatus`](https://raen.dev/admin/#/advanced.statusmessage.raendev.testnet/SetStatus)

You will also notice that now we don't get a string but a JSON object.