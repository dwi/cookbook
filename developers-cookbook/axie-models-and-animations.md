---
description: Get Axie's assets to use them in your application/game
---

# Axie models and animations



{% hint style="success" %}
Follow [official Axie Mixer documentation](https://docs.skymavis.com/mixer/introduction)
{% endhint %}

{% embed url="https://docs.skymavis.com/mixer/introduction" %}
[https://docs.skymavis.com/mixer/introduction](https://docs.skymavis.com/mixer/introduction)
{% endembed %}

### Getting image using Python

{% hint style="info" %}
This method requires node and python, credits to [Maxbrand99](https://twitter.com/maxbrand99)
{% endhint %}

Install Axie Mixer: `npm i @axieinfinity/mixer`

Create a file app.js

{% code title="app.js" %}
```javascript
const {AxieMixer} = require('@axieinfinity/mixer');
const axieGene512 = process.argv[2]
const mixer = new AxieMixer(axieGene512, skipAnimation=true)
const assets2 = mixer.getAvatarLayers()
console.log(JSON.stringify(assets2))
```
{% endcode %}

Create a python file that will generate the image from the genes

{% code title="generate.py" %}
```python
from subprocess import check_output
from PIL import Image
import json

AXIE_GENES = "0x200000000000030102C160810304000000010484108081040001049020008004000104800840C40400010594080085060001049428A0C1060001048008004004"
p = check_output(['node', './app.js', AXIE_GENES])
json_data = json.loads(str(p)[2:-3])
finalImage = Image.new("RGBA", (1024, 768))
for part in json_data:
    image = Image.open("stuff-v2/axie-2d-v3-stuff/" + part['imagePath'])
    finalImage.paste(image, (int(part['px']),int(part['py'])), image)
finalImage.save("merged_image.png","PNG")
```
{% endcode %}
