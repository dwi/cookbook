---
description: Get Axie's assets to use them in your application/game
---

# Axie models and animations



{% hint style="info" %}
All Axie models are currently designed in **Spine 3.7.93** (Spine 3.8 have new config structure so keep the 3.7x version).

Models can change to Spine 3.8+ in the future.
{% endhint %}

* Textures: [https://assets.axieinfinity.com/axies/3212/axie/axie.png](https://assets.axieinfinity.com/axies/3212/axie/axie.png)
* Spine: [https://assets.axieinfinity.com/axies/3212/axie/axie.json](https://assets.axieinfinity.com/axies/3212/axie/axie.json)
* Atlas: [https://assets.axieinfinity.com/axies/3212/axie/axie.atlas](https://assets.axieinfinity.com/axies/3212/axie/axie.atlas)
* Complete Axie image: [https://assets.axieinfinity.com/axies/3212/axie/axie-full-transparent.png](https://assets.axieinfinity.com/axies/3212/axie/axie-full-transparent.png)



#### Example how to use it in [Pixi.js spine](https://github.com/pixijs/spine):&#x20;

{% embed url="https://github.com/freakitties/freakitties.github.io/blob/master/axie/pixi/imageTest.html" %}
credits to OG legend freak
{% endembed %}

#### Tutorial on how to import Axies into Unity dynamically

{% embed url="https://youtu.be/TTYloKBOJ4A" %}
Credits to Zimmah & mrHazan
{% endembed %}

Script `AxieManager.cs` used in the video: [https://www.codepile.net/pile/J0Yx4e3R](https://www.codepile.net/pile/J0Yx4e3R)

{% code title="AxieManager.cs" %}
```csharp
using Spine.Unity;
using System.Collections;
using System.IO;
using TMPro;
using UnityEditor;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Serialization;

[RequireComponent(typeof(SkeletonAnimation))]
public class AxieManager : MonoBehaviour
{
    #region Fields

    public string axieID = "";
    public TMP_InputField axieIDInputField;

    private SkeletonAnimation _skeletonAnimation;

    #endregion Fields

    #region Editor

    [SerializeField]
    [FormerlySerializedAs("Atlas Json File")]
    private TextAsset _atlasJsonFile;

    [SerializeField]
    [FormerlySerializedAs("Skeleton Json File")]
    private TextAsset _skeletonJsonFile;

    [SerializeField]
    [FormerlySerializedAs("Atlas Materials")]
    private Material[] _atlasMaterials;

    [SerializeField]
    [FormerlySerializedAs("Image")]
    private Texture texture;

    #endregion Editor

    #region Methods

    private void Awake()
    {
        _skeletonAnimation = GetComponent<SkeletonAnimation>();
    }

    public void LoadAxie()
    {
        axieID = axieIDInputField.text;
        StartCoroutine(GetAtlas());
    }
    private void Start()
    {
    }

    public void FetchAxie()
    {
        Material mat = new Material(Shader.Find("Spine/Skeleton"));
        mat.mainTexture = texture;
        mat.mainTexture.name = "axie";
        
        _atlasMaterials[0] = mat;

        var atlasAsset = ScriptableObject.CreateInstance<SpineAtlasAsset>();
        atlasAsset.atlasFile = _atlasJsonFile;
        atlasAsset.materials = _atlasMaterials;

        var skeletonDataAsset = ScriptableObject.CreateInstance<SkeletonDataAsset>();
        skeletonDataAsset.atlasAssets = new[] { atlasAsset };
        skeletonDataAsset.skeletonJSON = _skeletonJsonFile;
        skeletonDataAsset.scale = 0.01f;

        
        _skeletonAnimation.skeletonDataAsset = skeletonDataAsset;
        _skeletonAnimation.timeScale = 1f;
        _skeletonAnimation.loop = true;
        
        
        _skeletonAnimation.Initialize(true);

        _skeletonAnimation.timeScale = 1f;
        _skeletonAnimation.loop = false;
        _skeletonAnimation.AnimationName = "action/evolve";

        var trackEntry = _skeletonAnimation.state.AddAnimation(0, "action/idle", true, 0);
        trackEntry.MixDuration = 0.5f;
    }

    private IEnumerator GetAtlas()
    {
        UnityWebRequest www = UnityWebRequest.Get("https://assets.axieinfinity.com/axies/" + axieID + "/axie/axie.atlas");
        yield return www.SendWebRequest();

        if (www.result != UnityWebRequest.Result.Success)
        {
            Debug.Log(www.error);
        }
        else
        {
            TextAsset axie = new TextAsset(www.downloadHandler.text);
            _atlasJsonFile = axie;
            StartCoroutine(GetAnim());
        }
    }
    private IEnumerator GetAnim()
    {
        UnityWebRequest www = UnityWebRequest.Get("https://assets.axieinfinity.com/axies/" + axieID + "/axie/axie.json");
        yield return www.SendWebRequest();

        if (www.result != UnityWebRequest.Result.Success)
        {
            Debug.Log(www.error);
        }
        else
        {
            _skeletonJsonFile = new TextAsset(www.downloadHandler.text);
            StartCoroutine(GetPNG());
        }
    }
    private IEnumerator GetPNG()
    {
        UnityWebRequest www = UnityWebRequestTexture.GetTexture("https://assets.axieinfinity.com/axies/" + axieID + "/axie/axie.png");
        yield return www.SendWebRequest();

        if (www.result != UnityWebRequest.Result.Success)
        {
            Debug.Log(www.error);
        }
        else
        {
            texture = ((DownloadHandlerTexture)www.downloadHandler).texture;
            FetchAxie();
        }
    }

    public void FaceRight()
    {
        _skeletonAnimation.initialFlipX = true;
        _skeletonAnimation.Initialize(true);
        _skeletonAnimation.timeScale = 1f;
        _skeletonAnimation.loop = false;
        _skeletonAnimation.AnimationName = "action/move-forward";

        var trackEntry = _skeletonAnimation.state.AddAnimation(0, "action/idle", true, 0);
        trackEntry.MixDuration = 0.5f;
        
    }
    public void FaceLeft()
    {
        _skeletonAnimation.initialFlipX = false;
        _skeletonAnimation.Initialize(true);

        _skeletonAnimation.timeScale = 1f;
        _skeletonAnimation.loop = false;
        _skeletonAnimation.AnimationName = "action/move-forward";

        var trackEntry = _skeletonAnimation.state.AddAnimation(0, "action/idle", true, 0);
        trackEntry.MixDuration = 0.5f;
        
    }
    #endregion Methods
}
```
{% endcode %}

