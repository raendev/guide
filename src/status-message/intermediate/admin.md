# Admin Round Two

Let's return to the admin panel this time for `intermediate.statusmessage.raendev.testnet`.

## Setting a status

Now there will two input boxes in the form:

[`raen.dev/admin/#/intermediate.statusmessage.raendev.testnet/SetStatus`](https://raen.dev/admin/#/intermediate.statusmessage.raendev.testnet/SetStatus)

## Get Status Message

Here is an example for eve.testnet:

[https://raen.dev/admin/#/intermediate.statusmessage.raendev.testnet/GetStatus?data=%257B%2522args%2522%253A%257B%2522account_id%2522%253A%2522eve.testnet%2522%257D%257D](https://raen.dev/admin/#/intermediate.statusmessage.raendev.testnet/GetStatus?data=%257B%2522args%2522%253A%257B%2522account_id%2522%253A%2522eve.testnet%2522%257D%257D)

Now the status should return a `String` of the JSON like

```json
"{\"title\": \"hello\", \"body\": \"world\"}"
```

## Next steps

Now let's take this a step further and use `Message` directly.