---
description: Technical implementation solutions
---

# Technical PoV



{% hint style="danger" %}
SLP / AXS payment monitoring / verification methods require knowledge of the Ronin blockchain and how to monitor transactions / events on chain.
{% endhint %}

Payment solutions can be done in roughly two ways.

### Credit system

The player deposits SLP / AXS into his game account by sending tokens to the given Ronin address and after the transaction is verified in the backend then tokens are "virtually" credited to player's account as a SLP, AXS or any other token of developers choice (gold, diamonds, credits, etc...). He can then use these tokens to pay tournament entry fees or buy other things within the game / application - all without interacting with the blockchain.

#### Pros

Very user friendly. The player "charges his in-game wallet" with one transaction and then doesn't have to worry about anything else.

#### Cons

The developer has to build a robust backend system. They must monitor SLP / AXS deposits, credit in-game accounts, manage SLP burning (or sending to treasury in the case of AXS), and process player payouts.

All this has to be built in a secure way so player's deposit funds are safe and whole mechanism is protected from various attack vectors (duplicating, unauthorized deposits / withdrawals / transfers, phising protections, etc...)



### On-demand payments

The player is asked to pay (send to a given address) the SLP / AXS every time he is required to do so, such as buying a in-game buff, paying the tournament entry fee, etc...

This method is less user-friendly than the "Credit System", but also requires slightly less security. However, it still requires the developer to create a backend system to securely track SLP / AXS payments (deposits) and give players purchased benefits (tournament entries, game buffs or items, ...) when their transaction is successfully processed on chain.



### (bonus option) Payment gateway

TBA
