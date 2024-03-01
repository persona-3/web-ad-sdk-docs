# Persona Ad SDK

Persona Ad SDK is a JavaScript-based software development kit that enables web app developers to display digital advertisements on their web apps using Persona's ad network. This SDK provides seamless integration with major frontend frameworks such as React, Vue, Angular, and more.

## Integration

The SDK can be integrated in two ways:

1. Integrating through `<script>` tags using cdn
2. Integrating via **npm**

## 1. Integration via script tag

To load the Persona Ad SDK in your app and initialize an ad client, insert the following scripts in the `<head>` section of your app / page.

```
<script src="https://cdn.jsdelivr.net/npm/@personaxyz/ad-sdk@0.0.19" crossorigin="anonymous"></script>
<script type="text/javascript">
    var personaConfig = {
        apiKey: "XXXX_api_key_staging_XXXX", // An actual API key is generated once you register an app with us.
        environment: "staging", // use value 'production' when going live
    }
    var sdk = new PersonaAdSDK.PersonaAdSDK(personaConfig);
    var adClient = sdk.getClient()
</script>
```

**Note:** Remember to update the configuration values when going live

Now to render a banner ad, an ad slot needs to be present in the app where the ad will render.
For instance, to render a banner ad with **600x160** dimensions , add the following snippet inside the `<body>` tag of your app.

```
<div id="my-banner-ad" style="width: 600px; height:160px;"></div> // The ad will render here
<script>
    adClient.setUserEmail('janedoe@email.com') // If available in your app
    adClient.setWalletAddress("1Lbcfr7sAHT...."); // If available in your app
    adClient.showBannerAd({
        adUnitId: "cf20c750-2fe4-4761-861f-b73b2247fd4d", // An ad unit id is provided by Persona for production environment
        containerId: "my-banner-ad" // HTML element's id where the ad needs to render on the app
    })
</script>
```

Similarly, more banner ads can be rendered using the above snippet with correct **adUnitIds and containerId**. One such implementation for an ad with **970x90** dimensions is as follows

```
<div id="my-banner-ad-2" style="width: 970px; height:90px;"></div> // The ad will render here
<script>
    adClient.setUserEmail('janedoe@email.com') // If available in your app
    adClient.setWalletAddress("1Lbcfr7sAHT...."); // If available in your app
    adClient.showBannerAd({
        adUnitId: "3a094192-4c7b-4761-a50c-bd9b6a67e987", // An ad unit id is provided by Persona for production environment
        containerId: "my-banner-ad-2" // HTML element's id where the ad needs to render on the app
    })
</script>
```

## Error Handling

The SDK also provides support for handling any unexpected errors while showing an ad. To handle errors on your end, you can provide an optional `callback` function to the `showBannerAd` function as a second parameter. This function will be triggered whenever any error occurs in the process of showing an ad. A sample usage is shown below

```
<script>
adClient.showBannerAd({
  adUnitId: "3a094192-4c7b-4761-a50c-bd9b6a67e987",
  containerId: "my-banner-ad-2"
}, (errorMessage) => {
  // You can handle error here.
})
</script>
```

**NOTE** : Please ensure that the showBannerAd method is called after the HTML element being referred to in the method is rendered on the app.

For testing all available ad dimensions, the following ad unit ids can be used in the **staging** environment.

```
1. 600x160 - cf20c750-2fe4-4761-861f-b73b2247fd4d
2. 300x250 - bf498e26-eb16-4e35-8954-e65690f28819
3. 970x90 - 3a094192-4c7b-4761-a50c-bd9b6a67e987
4. 321x101 - e6b82a11-6a94-46c0-a9d2-cf730159a5e6
```

**NOTE** : The above is just a sample implementation using values for **staging** environment.

**For the production environment, an app needs to be registered with us which generates different api keys and ad unit ids.**

## 2. Integration via npm

## Installation

To install the sdk , run the following command

```
npm install @personaxyz/ad-sdk@0.0.19
```

To integrate the sdk, follow these steps

1. Initialize the sdk
2. Initialize the client
3. Render a banner ad by calling `showBannerAd` function

### Initialize the sdk

Once the sdk is installed, you can initialize the sdk by creating a file `adConfig.js` and paste the following snippet in the `adConfig.js` file.

```
import { PersonaAdSDK } from '@personaxyz/ad-sdk';

const personaConfig = {
    apiKey: 'XXXX_api_key_staging_XXXX', // An actual API key is generated once you register an app with us.
    environment: 'staging', // use value 'production' when going live
}

const sdk = new PersonaAdSDK(personaConfig)
```

### Initialize the client

Once the sdk is initialized following the above step, we need to initialize a client.
To initialize a client, add the following snippet to the same file.

```
export const adClient = sdk.getClient()
```

### Render an ad

Now that the client is initiated, we can use the `adClient` object anywhere in the app to render an ad.

```
import { adClient } from '/path/to/adConfig.js'

adClient.showBannerAd({
    adUnitId: <AD_UNIT_ID>, // An ad unit id is generated when an ad unit is created for a registered app
    containerId: <DOM_ELEMENT_ID>, // HTML element's id where the ad needs to render on the app
})
```

### Error Handling

The SDK also provides support for handling any unexpected errors while showing an ad. To handle errors on your end, you can provide an optional `callback` function to the `showBannerAd` function as a second parameter. This function will be triggered whenever any error occurs in the process of showing an ad. A sample usage is shown below

```
import { adClient } from '/path/to/adConfig.js'

adClient.showBannerAd({
  adUnitId: <AD_UNIT_ID>,
  containerId: <DOM_ELEMENT_ID>
}, (errorMessage) => {
  // You can handle error here.
})
```

## Additional Features

#### Supported banner Ad Formats

1. 600x160
2. 300x250
3. 970x90
4. 321x101

**NOTE**: An ad's dimensions can be configured while creating an ad unit

#### Ad personalization

If your app supports integrating user's wallet address or email, you can use the `adClient` object to set wallet address or email respectively. This will help in increasing the maximum earned CPM by personalizing the ads.

```
adClient.setWalletAddress('1Lbcfr7sAHT....')

adClient.setUserEmail('janedoe@email.com')
```

**We recommend setting up these information as early in the app as possible for a rich ad experience**

# Usage Examples

The following examples include sample **React.js** and **Vue.js** implementations.

## Staging Keys

```
Api Key - XXXX_api_key_staging_XXXX
Ad Unit Ids -
1. 600x160 - cf20c750-2fe4-4761-861f-b73b2247fd4d
2. 300x250 - bf498e26-eb16-4e35-8954-e65690f28819
3. 970x90 - 3a094192-4c7b-4761-a50c-bd9b6a67e987
4. 321x101 - e6b82a11-6a94-46c0-a9d2-cf730159a5e6
```

## Sample React.js Implementation

Once we have exported the `adClient` object from `adConfig.js` file, we can create a reusable Banner Ad component as shown below

```
import { useEffect } from 'react';
import { adClient } from "/path/to/adConfig.js";

export const BannerAd = (props) => {
    const { id, adUnitId, width, height } = props;

    useEffect(() => {
        adClient.setWalletAddress("1Lbcfr7sAHT....")
        adClient.setUserEmail("janedoe@email.com")
        adClient.showBannerAd({
            adUnitId,
            containerId: id,
        })
    }, [adUnitId,id])

    return <div id={id} style={{ width, height }}></div>
}
```

The above component can now be used just like any other React component in your app as shown below

```
<BannerAd id="my-banner-ad" adUnitId="cf20c750-2fe4-4761-861f-b73b2247fd4d" width="600" height="160"/>
```

## Sample Vue.js Implementation

Once we have exported the `adClient` object from `adConfig.js` file, we can create a reusable Banner Ad component as shown below

```
<template>
  <div :id="id" :style="{ width, height }"></div>
</template>

<script>
import { adClient } from '/path/to/adConfig.js';

export default {
  props: {
    id: {
      type: String,
      required: true,
    },
    adUnitId: {
      type: String,
      required: true,
    },
    width: {
      type: String,
      required: true,
    },
    height: {
      type: String,
      required: true,
    },
  },
  computed: {
    walletAddress() {
      // Compute the wallet address dynamically
      // Replace the return statement with your actual computation logic
      return '1Lbcfr7sAHT....';
    },
    userEmail() {
      // Compute the user email dynamically
      // Replace the return statement with your actual computation logic
      return 'janedoe@email.com';
    },
  },
  mounted(){
    adClient.setWalletAddress(this.walletAddress);
    adClient.setUserEmail(this.userEmail);
    adClient.showBannerAd({
        adUnitId: this.adUnitId,
        containerId: this.id,
    });
  }
}
</script>
```

The above component can now be used just like any other Vue.js component in your app as shown below

```
<template>
  <div>
    <BannerAd id="banner-ad" adUnitId="bf498e26-eb16-4e35-8954-e65690f28819" width="300px" height="250px"/>
  </div>
</template>

<script>
import BannerAd from '/path/to/BannerAdVueComponent';

export default {
  components: {
    BannerAd,
  },
};
</script>
```

**Note** : The above is just a sample implementation using values for staging environment. For the production environment, an app needs to be registered with us which generates different api keys and ad unit ids.
