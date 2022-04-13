---
description: >-
  Smart contract for sending multiple ERC721 tokens at once to one or more
  recipients.
---

# ERC721 Batch Transfer

{% hint style="success" %}
#### Contract Address: <mark style="color:blue;">**`0x2368dfED532842dB89b470fdE9Fd584d48D4F644`**</mark>
{% endhint %}

### Functions

#### safeBatchTransfer(address \_tokenContract, uint256\[] \_ids, address \_recipient)

* Function Selector: ** `0xf9fd92b9`**
* Signature: `safeBatchTransfer(address,uint256[],address)`
* This method can be used to transfer multiple NFTs with the same contract to one recipient.
* Requirements: `msg.sender` has to call setApprovalForAll on `_tokenContract` to authorize this contract.

#### safeBatchTransfer(address \_tokenContract, uint256\[] \_ids, address\[] \_recipients)

* Function Selector: ** `0xd56ad454`**
* Signature: `safeBatchTransfer(address,uint256[],address[])`
* This method can be used to transfer multiple NFTs with the same contract to multiple recipients.
* Requirements: `msg.sender` has to call setApprovalForAll on `_tokenContract` to authorize this contract.

### ABI

{% code title="ERC721Batch.json" %}
```json
[
    {
        "inputs": [
            {
                "internalType": "contract IERC721",
                "name": "_tokenContract",
                "type": "address"
            },
            {
                "internalType": "uint256[]",
                "name": "_ids",
                "type": "uint256[]"
            },
            {
                "internalType": "address[]",
                "name": "_recipients",
                "type": "address[]"
            }
        ],
        "name": "safeBatchTransfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IERC721",
                "name": "_tokenContract",
                "type": "address"
            },
            {
                "internalType": "uint256[]",
                "name": "_ids",
                "type": "uint256[]"
            },
            {
                "internalType": "address",
                "name": "_recipient",
                "type": "address"
            }
        ],
        "name": "safeBatchTransfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
{% endcode %}
