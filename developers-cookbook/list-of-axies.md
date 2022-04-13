---
description: What Axies certain address holds?
---

# List of Axies

### GraphQL query

Use [`GetAxieBriefList`](https://axie-graphql.web.app/operations/getAxieBriefList) operation ([learn-about-graphql.md](learn-about-graphql.md "mention"))

{% hint style="info" %}
Oneliner example: [https://graphql-gateway.axieinfinity.com/graphql?query={axies(from:0,size:100,sort:IdAsc,owner:%220x3e429e444a8cb033c41e27f2752cf3ac54e39be1%22){total,results{id\}}}](https://graphql-gateway.axieinfinity.com/graphql?query={axies\(from:0,size:100,sort:IdAsc,owner:%220x3e429e444a8cb033c41e27f2752cf3ac54e39be1%22\){total,results{id\}}})

_**This is not the recommended way to work with GraphQL**_
{% endhint %}

### Different API provider

For example [CovalentHQ](https://www.covalenthq.com). Full documentation [here](https://www.covalenthq.com/docs/api/#/overview) (Ronin chain\_id: 2020)

Recommended to sign up on the site and generate your own unique API key. It is for free.

{% hint style="info" %}
Oneliner example: [https://api.covalenthq.com/v1/2020/address/0x3e429e444a8cb033c41e27f2752cf3ac54e39be1/balances\_v2/?quote-currency=USD\&format=JSON\&nft=true\&no-nft-fetch=true\&key=ckey\_docs](https://api.covalenthq.com/v1/2020/address/0x3e429e444a8cb033c41e27f2752cf3ac54e39be1/balances\_v2/?quote-currency=USD\&format=JSON\&nft=true\&no-nft-fetch=true\&key=ckey\_docs)
{% endhint %}
