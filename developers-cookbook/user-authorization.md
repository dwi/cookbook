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

