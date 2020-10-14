# rn-sms-retriever

React Native implementation of Android SMS Retriever. No READ_SMS permisssion is required now, instead we have to use [SMS retriever](https://developers.google.com/identity/sms-retriever/overview).

For better understanding, please refer this [article](https://medium.com/android-dev-hacks/autofill-otp-verification-with-latest-sms-retriever-api-73c788636783). 

## Installation

```sh
npm install rn-sms-retriever
```

## Usage

```js
import RnSmsRetriever from "rn-sms-retriever";

export default function App() {

  const SMS_EVENT = "me.furtado.smsretriever:SmsEvent"

  React.useEffect(() => {
    let smsListener: undefined | EmitterSubscription;
    async function innerAsync() {
      // get list of available phone numbers
      await RnSmsRetriever.requestPhoneNumber();
      // get App Hash
      const hash = await RnSmsRetriever.getAppHash();
      console.log('Your App Hash is : ' + hash);
      // set Up SMS Listener;
      smsListener = DeviceEventEmitter.addListener(SMS_EVENT, (data: any) => {
        console.log(data, 'SMS value');
      });
      // start Retriever;
      await RnSmsRetriever.startSmsRetriever();
    }
    // only to be used with Android
    if (Platform.OS == "android")
      innerAsync();
    return () => {
      // remove the listsner on unmount
      smsListener?.remove();
    };
  }, []);
  //....
}
```

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT
