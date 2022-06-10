# Getting a blank status

Let's return to the admin panel this time for `simple.statusmessage.raendev.testnet`.

First, let's try getting the status message for an account that definitely won't have one.  We should get back `null`, JavaScript's version of `Option::None`. This

Use the following link:

[`raen.dev/admin/#/simple.statusmessage.raendev.testnet/GetStatus?data=%257B%2522args%2522%253A%257B%2522account_id%2522%253A%2522aa%2522%257D%257D`](https://raen.dev/admin/#/simple.statusmessage.raendev.testnet/GetStatus?data=%257B%2522args%2522%253A%257B%2522account_id%2522%253A%2522aa%2522%257D%257D)

You'll notice that the link has some extra args at the end. This is the form data that will automatically fill in the form and since this is a view method it execute the call immediately.

## Setting a status

[`raen.dev/admin/#/simple.statusmessage.raendev.testnet/SetStatus`](https://raen.dev/admin/#/simple.statusmessage.raendev.testnet/SetStatus)

Next, return to [`get_status`](https://raen.dev/admin/#/simple.statusmessage.raendev.testnet/GetStatus), but this time with your own testnet account.
