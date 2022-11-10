# Code snippets

### Generate random address+privkey combination

{% hint style="info" %}
Useful for generating individual deposit address for each individual player. Don't forgot to save the privateKey!
{% endhint %}

{% tabs %}
{% tab title="Web3.js" %}
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
{% endtab %}

{% tab title="Ethers.js" %}
```javascript
const ethers = require('ethers')

async function createAccount(){
    const wallet = ethers.Wallet.createRandom()
    console.log('Address:' + wallet.address);
    console.log('Private Key: ' + wallet.privateKey)
}

createAccount();
```
{% endtab %}
{% endtabs %}

### Generate subaccount(s) for given secret recovery phrase

{% hint style="info" %}
Generate infinite number of accounts derived from one specific secret recovery phrase. You can keep one seed in a secret place and store only `player_addres+account_nr` combination in the database.
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

### Monitor ERC-20 token transactions live

This node.js snippet uses the web3 library and monitors live ERC-20 token movements in the Ronin network.

{% hint style="danger" %}
* You must have WebSocket access to own node (In this example [run-a-ronin-node.md](../developers-cookbook/run-a-ronin-node.md "mention")locally - RPC is available on`http://localhost:8546`)
{% endhint %}

```javascript
const Web3 = require('web3')

const web3 = new Web3(new Web3.providers.WebsocketProvider('http://localhost:8546'));

const CONTRACT_ADDRESS = "0xa8754b9fa15fc18bb59458815510e40a12cd2014";
const CONTRACT_ABI = require('./abi.json'); // https://unpkg.com/erc-20-abi@1.0.0/src/abi.json
const contract = new web3.eth.Contract(CONTRACT_ABI, CONTRACT_ADDRESS);

function watchTokenTransfers() {
    // Filtering options
    const options = {
      filter: {
          // You can filter on specific receiving address
          to:    '0x097FAa854B87Fdebb538F1892760eA1b4F31fa41'
      },
      fromBlock: 'latest'
    }
  
    contract.events.Transfer(options, async (error, data_events) => {
      if (error) {
        console.log(error)
        return
      }

      let from = data_events['returnValues']['from'];
      let to = data_events['returnValues']['to'];
      let amount = data_events['returnValues']['value'];

      // condition to check 'to' equals your deposit address, then issue purchase to 'from' address
      console.log("From:", from, "To:", to, "Value:", amount);
    })
  }

  watchTokenTransfers()
```

### Get past ERC-20 transactions from previous blocks

This node.js snippet uses the web3 library and retrieves the transaction history of ERC-20 token transfers in the Ronin network from the given block history (30minutes in this example).

{% hint style="danger" %}
You must have RPC access to the Ronin node (runnin locally in this example on `http://localhost:8545`)
{% endhint %}

```javascript
const Web3 = require('web3')

const web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

const CONTRACT_ADDRESS = "0xa8754b9fa15fc18bb59458815510e40a12cd2014"; // SLP Address
const CONTRACT_ABI = require('./abi.json'); // https://unpkg.com/erc-20-abi@1.0.0/src/abi.json
const BLOCK_HISTORY = 600; // 1 block = 3 seconds
const contract = new web3.eth.Contract(CONTRACT_ABI, CONTRACT_ADDRESS);

async function getEvents(event_name) {
    let latest_block = await web3.eth.getBlockNumber();
    let historical_block = latest_block - BLOCK_HISTORY;
    console.log("latest: ", latest_block, "historical block: ", historical_block);

    // Filtering options
    const options = {
        filter: {
            // You can filter on specific receiving address
            to:    '0x097FAa854B87Fdebb538F1892760eA1b4F31fa41'
        },
        fromBlock: historical_block,
        toBlock: 'latest'
      }
    const events = await contract.getPastEvents(event_name, options);
    await getTransferDetails(events);
};

async function getTransferDetails(data_events) {
    for (i = 0; i < data_events.length; i++) {
        let block_number = data_events[i]['blockNumber'];
        let from = data_events[i]['returnValues']['from'];
        let to = data_events[i]['returnValues']['to'];
        let amount = data_events[i]['returnValues']['value'];
        // condition to check 'to' equals your deposit address, then issue purchase to 'from' address
        console.log("Block:",block_number,"From:", from, "- To:", to, "- Amount:", amount);
    };
};

getEvents('Transfer'); // Look for Transfer events
```

Sample output:

![](<../.gitbook/assets/image (2) (1).png>)
