---
description: Get details of individual Axie(s)
---

# Axie details

###

### GraphQL query

Use [`GetAxieBriefList`](https://axie-graphql.web.app/operations/getAxieBriefList\)) or [`GetAxieDetail`](https://axie-graphql.web.app/operations/getAxieDetail) orperation ([learn-about-graphql.md](learn-about-graphql.md "mention"))

{% embed url="https://docs.skymavis.com/mixer/genes" %}
[https://docs.skymavis.com/mixer/genes](https://docs.skymavis.com/mixer/genes)
{% endembed %}

####

### Community made API

Our community developers Explorer and MaxBrand99 developed a bunch of freely available API functions for anyone to use. All possible API calls are described in their documentation [here](https://maxbrand99.notion.site/Sky-Mavis-Game-API-Proxies-05870c1221cc4fd0b702e9d0ec7daa28):&#x20;

{% embed url="https://maxbrand99.notion.site/Sky-Mavis-Game-API-Proxies-V2-Explanations-d6e4c082ddb24620825e31334d4c39cd" %}
[https://maxbrand99.notion.site/Sky-Mavis-Game-API-Proxies-V2-Explanations-d6e4c082ddb24620825e31334d4c39cd](https://maxbrand99.notion.site/Sky-Mavis-Game-API-Proxies-V2-Explanations-d6e4c082ddb24620825e31334d4c39cd)
{% endembed %}

### Direct Ronin on-chain query

Last option is complete on-chain Axie gene parsing, suited only to experienced users.

1. Call <mark style="background-color:blue;">`axie(id)`</mark> function on Axie contract, you will receive a 512bit gene split in `x` and `y` values.
2.  Convert those two values into one 512bit gene string. Example:\


    ```javascript
    const axieState = await axieContract.methods.axie(4950464).call()

    const x = BigInt(axieState.genes.x).toString(2).padStart(256, '0')
    const y = BigInt(axieState.genes.y).toString(2).padStart(256, '0')
    const z = BigInt('0b' + (x + y)).toString(16)

    const axieGene = new AxieGene(z, HexType.Bit512)
    ```


3. Use official [Axie Infinity Mixer](https://docs.skymavis.com/mixer/mixer-sdk/javascript-guide) to decode the genes.

### Get existing Axie card/move names

The list of existing body parts with their ID, class, special type and name is available in this json:

{% embed url="https://raw.githubusercontent.com/freakitties/axieExt/master/body-parts.json" %}

The list of actual V2 cards and their stats/descriptions is here:

{% embed url="https://storage.googleapis.com/axie-cdn/game/cards/card-abilities.json" %}
