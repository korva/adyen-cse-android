# AdyenCSE-Android
This repository contains Adyen's Client Side Encryption (CSE) library for Android. With CSE card data is encrypted within the client, in this case the Android device, before you submit it via your own server to the Adyen API. By using CSE you reduce your scope of PCI compliance, because no raw card data travels through your server. This repository can be leveraged as a starting point to integrate Adyen's payment functionality fully in-app.

## Requirements
The AdyenCSE-Android library is written in Java and is compatible with apps supporting Android 4.4 and up. Looking for the Android or web equivalent? We have the CSE library also available written in Objective-C ([AdyenCSE-Android](https://github.com/Adyen/AdyenCSE-iOS)) and JavaScript ([AdyenCSE-web](https://github.com/Adyen/CSE-JS)).

All our CSE libraries rely on you [setting up your own server](https://docs.adyen.com/developers/easy-encryption#cardintegration) for communicating with the Adyen API. By using a server you ensure that API authentication credentials never get exposed. Please note that you need to have [signed up for an account at Adyen](https://www.adyen.com/signup) before you can send requests to the Adyen API.

## Example

For you convenience we've included an example app in this repository that can be used as a reference while integrating.

To run the example project, clone the repo and run it from your IDE.

## Installation

AdyenCSE is available through a gradle task. To install it, simply execute the following steps:

1. In your `build.gradle` of the root directory add the following line:
    
    ```json
    buildscript {
        repositories {
            ...
        }
        dependencies {
            ...
            classpath 'de.undercouch:gradle-download-task:2.1.0'
        }
    }
    ```
    This plugin will allow Gradle to download the AdyenCSE library.
    
2. In your `build.gradle` of the app module add the following task:

    ```json
    import de.undercouch.gradle.tasks.download.Download
    
    ...
    
    task downloadAdyenLibrary(type: Download) {
        src 'https://raw.githubusercontent.com/Adyen/AdyenCSE-Android/master/adyencse/adyencse-1.0.0.aar'
        dest('libs');
    }
    ```
   Once you run this task using `./gradlew downloadAdyenLibrary` command, the `adyencse` .aar file will be downloaded in you `libs` folder.
   
3. Next add the following snippet in your `build.gradle` of the app module:

    ```json
    repositories {
        flatDir {
            dirs 'libs'
        }
    }
    ```

## Usage

```java
//	Set public key
String publicKey = @"10001|B243E873CB9220BAFE71...";

//	Create card object
Card card = new Card();
card.setCardHolderName("John A...");
card.setCvc("737");
card.setExpiryMonth("08");
card.setExpiryYear("2018");
card.setGenerationTime(new Date());
card.setNumber("55551...");
return card;

//	Encrypt card data
String encryptedCard = card.serialize(publicKey);
```

Please note that you'll have to URL encode the `encryptedCard` before sending it from the app to your server, as the `encryptedCard` which is generated by the CSE library should be exactly the same as you send it from the server to the Adyen API.

## What's next?
After having developed your app, set up the merchant server and succesfully performed your first [test payment](https://docs.adyen.com/developers/test-cards-manual) it's time to complete your integration by registering for Aydyen's notification service. After each payment initiation we push a notification to your server with the authorisation reponse, so you can be sure whether you can start delivering your goods or services. To subscribe to and integrate with the notification service, please check our [notification manual](https://docs.adyen.com/developers/api-manual#notifications).

Succesfully subscribed to and integrated with our notification service? Congratulations, now it's time to start accepting payments for real! Assuming that you've been using your Adyen test account, and the Adyen API's test endpoints, you can now make use of your Adyen live account and Adyen API's [live endpoints](https://docs.adyen.com/developers/api-manual#apiendpoints). Questions? Contact your account manager or send your inquery to [support@adyen.com](mailto:support@adyen.com).

## License

AdyenCSE is available under the MIT license. See the LICENSE file for more info.
