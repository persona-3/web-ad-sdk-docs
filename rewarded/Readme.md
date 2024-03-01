# Persona Ad SDK

Persona Ad SDK is a JavaScript-based software development kit that enables web app developers to display digital advertisements on their web apps using Persona's ad network. This SDK provides seamless integration with major frontend frameworks such as React, Vue, Angular, and more.

## Integration

The SDK can be integrated in two ways:

1. Integrating through `<script>` tags using cdn
2. Integrating via **npm**

## 1. Integration via script tag

To load the Persona Ad SDK in your app and initialize an ad client, insert the following scripts in the `<head>` section of your app / page.

```
<script src="https://cdn.jsdelivr.net/npm/@personaxyz/ad-sdk@0.1.0-beta" crossorigin="anonymous"></script>
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

Now to render a rewarded ad, an ad slot needs to be present in the app where the ad will render.
For instance, to render a rewarded ad with **600x160** dimensions, add the following snippet inside the `<body>` tag of your app.

```
<div id="my-rewarded-ad" style="width: 600px; height:160px;"></div> // The ad will render here
<script>
// Set a unique identifier for the logged-in user (e.g., email, wallet address) using adClient.setUserId().
// This identifier is crucial for associating rewards with the user and must be set before invoking any of the following functions: showRewardedAd, getRewardsInfo, claimRewards.
    adClient.setUserId('janedoe@email.com')
    adClient.setUserEmail('janedoe@email.com') // If available in your app
    adClient.setWalletAddress("1Lbcfr7sAHT...."); // If available in your app
    adClient.showRewardedAd({
        adUnitId: "cb0decee-e710-422a-b9c8-522f7862e11a", // An ad unit id is provided by Persona for production environment
        containerId: "my-rewarded-ad" // HTML element's id where the ad needs to render on the app
    })
</script>
```

## Error Handling

The SDK also provides support for handling any unexpected errors while showing an ad. To handle errors on your end, you can provide an optional `callback` function to the `showRewardedAd` function as a second parameter. This function will be triggered whenever any error occurs in the process of showing an ad. A sample usage is shown below

```
<script>
adClient.showRewardedAd({
  adUnitId: "602d5d27-69ae-4cd9-9e3a-ad8f19ecd72a",
  containerId: "my-rewarded-ad-2"
}, (errorMessage) => {
  // You can handle error here.
})
</script>
```

**NOTE** : Please ensure that the showRewardedAd method is called after the HTML element being referred to in the method is rendered on the app.

For testing all available ad dimensions, the following ad unit ids can be used in the **staging** environment.

```
1. 600x160 - cb0decee-e710-422a-b9c8-522f7862e11a
2. 300x250 - b9800fb3-1b21-47ee-9074-c89b5afc7d2f
3. 970x90 - 602d5d27-69ae-4cd9-9e3a-ad8f19ecd72a
4. 321x101 - 5f71edc4-9549-448f-9c50-50d62087b101
```

**NOTE** : The above is just a sample implementation using values for **staging** environment.

## Fetching Rewards Info

To fetch the Rewards Info for a particular user in your app-
1. Initialize an ad client in the `<head>` section of your app / page as already shown above.
2. Set the userID using adClient.userId() function as already shown above.
3. Call the getRewardsInfo function as shown in the below snippet. Note that you can provide an optional `callback` function which will be triggered whenever any error occurs in the process of fetching the rewards.
```
<script>
async function fetchUserRewardsInfo() {
  const result = await adClient.getRewardsInfo((errorMessage) => {
    // You can handle error here.
  });
  console.log(result);
  // Do something with the result
}
  fetchUserRewardsInfo();
</script>
```
On a successful Response the **result** variable would store data of type **RewardsInfoResponse**-
```
interface RewardsInfo{
  year: number;
  month: number;
  claimable_rewards: number;
  claimed_rewards: number;
}

interface RewardsInfoResponse {
  data: {
    message: string;
    data: {
      user_id: string;
      rewards_info: RewardsInfo[]
    };
  };
}
```

On an unsuccessful response the **result** variable would be undefined. The error callback can be used to retrieve the error message.
## Claiming Rewards

To claim the Rewards earned by a particular user in your app-
1. Initialize an ad client in the `<head>` section of your app / page as already shown above.
2. Set the userID using adClient.userId() function as already shown above.
3. Call the claimRewards function as shown in the below snippet. Note that you can provide an optional `callback` function which will be triggered whenever any error occurs in the process of claiming the rewards.
```
<script>
  async function claimUserRewards() {
    const result = await adClient.claimRewards((errorMessage) => {
      // You can handle error here.
  });
    console.log(result);
    // Do something with the result
}
  claimUserRewards();
</script>
```

On a successful Response the **result** variable would store data of type **RewardsInfoResponse**-
```
interface RewardsInfo{
  year: number;
  month: number;
  claimable_rewards: number;
  claimed_rewards: number;
}

interface RewardsClaimResponse {
  data: {
    message: string;
    data: {
      user_id: string;
      total_claimed_amount: number;
      claimed_rewards: RewardsInfo[]
    };
  };
}
```

On an unsuccessful response the **result** variable would be undefined. The error callback can be used to retrieve the error message.

**For the production environment, an app needs to be registered with us which generates different api keys and ad unit ids.**

## 2. Integration via npm

## Installation

To install the sdk , run the following command

```
npm install @personaxyz/ad-sdk@0.1.0-beta
```

To integrate the sdk, follow these steps

1. Initialize the sdk
2. Initialize the client
3. Render a rewarded ad by calling `showRewardedAd` function

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

adClient.setUserId(<LOGGED_IN_USER_UNIQUE_ID>)
adClient.showRewardedAd({
    adUnitId: <AD_UNIT_ID>, // An ad unit id is generated when an ad unit is created for a registered app
    containerId: <DOM_ELEMENT_ID>, // HTML element's id where the ad needs to render on the app
})
```

### Error Handling

The SDK also provides support for handling any unexpected errors while showing an ad. To handle errors on your end, you can provide an optional `callback` function to the `showRewardedAd` function as a second parameter. This function will be triggered whenever any error occurs in the process of showing an ad. A sample usage is shown below

```
import { adClient } from '/path/to/adConfig.js'

adClient.showRewardedAd({
  adUnitId: <AD_UNIT_ID>,
  containerId: <DOM_ELEMENT_ID>
}, (errorMessage) => {
  // You can handle error here.
})
```
### Fetching Rewards Info
To fetch the Rewards Info for a particular user in your app-
1. Initialize an ad client in the `<head>` section of your app / page as already shown above.
2. Set the userID using adClient.userId() function as already shown above.
3. Call the getRewardsInfo function as shown in the below snippet. Note that you can provide an optional `callback` function which will be triggered whenever any error occurs in the process of fetching the rewards.

```
import { adClient } from '/path/to/adConfig.js'

async function fetchUserRewardsInfo() {
  const result = await adClient.getRewardsInfo((errorMessage) => {
    // You can handle error here.
  });
  console.log(result);
  // Do something with the result
}
  fetchUserRewardsInfo();
```

On a successful Response the **result** variable would store data of type **RewardsInfoResponse**-
```
interface RewardsInfo{
  year: number;
  month: number;
  claimable_rewards: number;
  claimed_rewards: number;
}

interface RewardsInfoResponse {
  data: {
    message: string;
    data: {
      user_id: string;
      rewards_info: RewardsInfo[]
    };
  };
}
```

On an unsuccessful response the **result** variable would be undefined. The error callback can be used to retrieve the error message.
## Claiming Rewards
To claim the Rewards earned by a particular user in your app-
1. Initialize an ad client in the `<head>` section of your app / page as already shown above.
2. Set the userID using adClient.userId() function as already shown above.
3. Call the claimRewards function as shown in the below snippet. Note that you can provide an optional `callback` function which will be triggered whenever any error occurs in the process of claiming the rewards.
```
import { adClient } from '/path/to/adConfig.js'

async function claimUserRewards() {
  const result = await adClient.claimRewards((errorMessage) => {
    // You can handle error here.
  });
  console.log(result);
  // Do something with the result
}
  claimUserRewards();
```

On a successful Response the **result** variable would store data of type **RewardsInfoResponse**-
```
interface RewardsInfo{
  year: number;
  month: number;
  claimable_rewards: number;
  claimed_rewards: number;
}

interface RewardsClaimResponse {
  data: {
    message: string;
    data: {
      user_id: string;
      total_claimed_amount: number;
      claimed_rewards: RewardsInfo[]
    };
  };
}
```

On an unsuccessful response the **result** variable would be undefined. The error callback can be used to retrieve the error message.

## Additional Features

#### Supported rewarded Ad Formats

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
1. 600x160 - cb0decee-e710-422a-b9c8-522f7862e11a
2. 300x250 - b9800fb3-1b21-47ee-9074-c89b5afc7d2f
3. 970x90 - 602d5d27-69ae-4cd9-9e3a-ad8f19ecd72a
4. 321x101 - 5f71edc4-9549-448f-9c50-50d62087b101
```

## Sample React.js Implementation

Once we have exported the `adClient` object from `adConfig.js` file, we can create a reusable Rewarded Ad component as shown below

```
import { useEffect } from 'react';
import { adClient } from "/path/to/adConfig.js";

export const RewardedAd = (props) => {
    const { id, adUnitId, width, height } = props;

    useEffect(() => {
        adClient.setUserId('janedoe@email.com')
        adClient.setWalletAddress("1Lbcfr7sAHT....")
        adClient.setUserEmail("janedoe@email.com")
        adClient.showRewardedAd({
            adUnitId,
            containerId: id,
        })
    }, [adUnitId,id])

    return <div id={id} style={{ width, height }}></div>
}
```

The above component can now be used just like any other React component in your app as shown below

```
<RewardedAd id="my-rewarded-ad" adUnitId="cb0decee-e710-422a-b9c8-522f7862e11a" width="600" height="160"/>
```

## Sample Vue.js Implementation

Once we have exported the `adClient` object from `adConfig.js` file, we can create a reusable Rewarded Ad component as shown below

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
    userId() {
      // Compute the user ID dynamically
      // Replace the return statement with your actual computation logic
      return 'janedoe@email.com';
    },
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
    adClient.setUserId(this.userId);
    adClient.setWalletAddress(this.walletAddress);
    adClient.setUserEmail(this.userEmail);
    adClient.showRewardedAd({
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
    <RewardedAd id="rewarded-ad" adUnitId="b9800fb3-1b21-47ee-9074-c89b5afc7d2f" width="300px" height="250px"/>
  </div>
</template>

<script>
import RewardedAd from '/path/to/RewardedAdVueComponent';

export default {
  components: {
    RewardedAd,
  },
};
</script>
```

**Note** : The above is just a sample implementation using values for staging environment. For the production environment, an app needs to be registered with us which generates different api keys and ad unit ids.
