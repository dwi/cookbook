---
description: How to authorize the user in your application using Ronin address
---

# User Authorization

{% hint style="info" %}
Sky Mavis currently does not provide an easy way to do so, so here is a list of possible workarounds until some more official solution is introduced.
{% endhint %}

### MetaMask authorization with Ethereum address

{% hint style="warning" %}
User needs to have his Ethereum Address linked to his Ronin address on marketplace profile: [https://marketplace.axieinfinity.com/profile/settings/](https://marketplace.axieinfinity.com/profile/settings/)
{% endhint %}

Use operation `GetProfileByEthAddress` on Axie's GraphQL ([learn-about-graphql.md](learn-about-graphql.md "mention")) to retrieve Ronin address that have been linked to Ethereum address.

As usual, GitHub is your friend: [https://github.com/search?q=GetProfileByEthAddress\&type=code](https://github.com/search?q=GetProfileByEthAddress\&type=code)

####

### MetaMask connected to Ronin chain.

{% hint style="danger" %}
This workaround requires user to have his Ronin secret recovery phrase or private key imported in his MetaMask
{% endhint %}

With this method you can basically ask user to sign a message to verify his Ronin address (used in MetaMask) and then read all sort of data from a Ronin network using any web3 library.

Manually add Ronin network to MetaMask:

| Network Name       | Ronin                                                               |
| ------------------ | ------------------------------------------------------------------- |
| RPC URL            | https://api.roninchain.com/rpc                                      |
| Chain ID           | 2020                                                                |
| Currency Symbol    | RON                                                                 |
| Block Explorer URL | [https://explorer.roninchain.com/](https://explorer.roninchain.com) |

Or use MetaMask's addEthereumChain API: [https://docs.metamask.io/guide/rpc-api.html#wallet-addethereumchain](https://docs.metamask.io/guide/rpc-api.html#wallet-addethereumchain)

```
method: 'wallet_addEthereumChain',
params: [
    {
    chainId: '0x7E4',
    chainName: 'Ronin Network',
    nativeCurrency: {
        name: 'RON',
        symbol: 'RON',
        decimals: 18
    },
    blockExplorerUrls: ['https://explorer.roninchain.com/'],
    rpcUrls: ['https://api.roninchain.com/rpc'],
    },
]
```

####

### Official message signing solution by AxieDAO

A simple message signing SSO: TBD

### Usage

#### Cross-window messageListener

* Application is opened from another page. Signature JSON payload is received as a message event

```javascript
let RoninSignatureWindow = window.open('http://localhost:3000/?ref='+window.parent.location.href)
window.addEventListener('message', function receiveSig(event) {
	if (typeof event.data === 'object' && 'key' in event.data && event.data.key === 'signature') {
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
    "address": "0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B",
    "message": "AxieDAO SSO | 2022-02-08T15:56:45.798Z\n\nMyBuildersApp nonce: 420",
    "messageHash": "0x73b1…3f8f",
    "signature": "0x2e2874…f83f1b"
}
```
