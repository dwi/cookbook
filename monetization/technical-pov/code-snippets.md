# Code snippets

#### Generate random address+privkey combination (web3.js)

{% hint style="info" %}
Useful for generating individual deposit address for players.
{% endhint %}

```javascript
const Web3 = require('web3')

const web3 = new Web3(new Web3.providers.HttpProvider("https://api.roninchain.com/rpc"));

async function createAccount(){
    var wallet = await web3.eth.accounts.create();
    console.log('Address:' + wallet.address);
    console.log('Private Key: ' + wallet.privateKey)
}

createAccount();
```

#### Generate subaccount(s) for given secret recovery phrase (web3.js+ethers.js)

{% hint style="info" %}
Generate infinite number of accounts derived from one specific secret recovery phrase
{% endhint %}

```javascript
const Web3 = require('web3')
const ethers = require('ethers')

const web3 = new Web3(new Web3.providers.HttpProvider("https://api.roninchain.com/rpc"));

getWalletFromMnemonic = (mnemonic, account_nr = 0) => {
  var path = `m/44'/60'/0'/0/${account_nr}`;
  var walletMnemonic = ethers.Wallet.fromMnemonic(mnemonic, path);
  var wallet = web3.eth.accounts.privateKeyToAccount(walletMnemonic.privateKey)
  console.log('Address:' + wallet.address);
  console.log('Private Key: ' + wallet.privateKey)
}

getWalletFromMnemonic("flock smart cost pumpkin wise machine grant picnic palace blanket hard turn", 30)

```
