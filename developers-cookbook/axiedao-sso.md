# AxieDAO SSO



### Official message signing solution by AxieDAO

A simple message signing SSO: [https://ronin.axiedao.org/sso/](https://ronin.axiedao.org/sso/)

Live example: [https://axie.live/sso.html](https://axie.live/sso.html)

####

#### Cross-window messageListener

* Application is opened from another page. Signature JSON payload is received as a message event

```javascript
let RoninSignatureWindow = window.open('https://ronin.axiedao.org/sso/?ref=' + window.parent.location.href)
window.addEventListener('message', function receiveSig(event) {
  if (
    typeof event.data === 'object' &&
    'key' in event.data &&
    event.data.key === 'signature' &&
    event.origin === 'https://ronin.axiedao.org'
  ) {
    window.removeEventListener('message', receiveSig, false); // Remove listener
    const signature = event.data.message; // Actual signature payload
  }
});
```

`ref` parameter might be mandatory if referrer is not properly attached.

#### Callback URL

* Signature JSON payload is sent to specified URL callback endpoint (POST method). Optional `redirectURL` can be specified to redirect

### Parameters

| Parameter     | type    | description                                                                                                                            |
| ------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `message`     | string  | Message to be signed, should act as a nonce from a developer to verify validity of signature                                           |
| `ref`         | url     | Specifies the origin referring page (usually `window.parent.location.href`) in case if address is not available as `document.referrer` |
| `callbackURL` | url     | Destination endpoint where to send the signature JSON payload. Mandatory for Callback method.                                          |
| `redirectURL` | url     | Optional destination where the page redirects to after successful submission to the `callbackURL`                                      |
| `autoclose`   | Boolean | Try to close the window when finished and no `redirectURL` was specified.                                                              |

### Response format

Response JSON payload:

```json
{
  "message": {
    "title": "'AxieDAO SSO",
    "timestamp": "2022-03-17T10:52:30.957Z",
    "callbackURL": "https://my.custom.dapp/loginCallback",
    "message": "Custom message to sign / nonce"
  },
  "messageHash": "0x73b1…3f8f",
  "signature": "0x2e2874…f83f1b",
  "address": "0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B"
}
```

