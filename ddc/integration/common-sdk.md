# Web

## Installation


### Option 1: CDN

Add the Cere Web SDK directly to your HTML to load the library.

```javascript
<script src="https://sdk.dev.cere.io/v5.3.2/web.js"></script>
```

### Option 2: NPM

If your web application uses NPM, you can add our SDK as a dependency.

```shell
npm install --save @cere/sdk-js@5.3.2
```

Then, you can use import it in your code:
```typescript
import { cereWebSDK } from "@cere/sdk-js/dist/web";
```

### Initial SDK setup

Once the Cere Web SDK is installed on your web application, initialize the library with `APP_ID` and `YOUR_USER_ID`.

You can find your APP_ID on the RXB Dashboard

User ID is what should be set for each of your users and be unchangeable.


```javascript
let appId = 242;
let yourUserId = 'd65e211e-047b-4c0d-8c8c-933e3c04abe1';

try {
   const sdk = cereWebSDK(appId, userId);
} catch {
   console.log('Could not initialize cereSDK.');
}
```
App ID and User ID should be provided, otherwise, an error will be thrown.

You can also specify the container element to where you want the in-app messages will be attached. By default, in-app messages are attached to the `<body>`.

```javascript
let containerForInAppMessages = document.getElementById('container');
try {
const sdk = cereWebSDK(appId, yourUserId, containerForInAppMessages);
} catch {
console.log('Could not initialize cereSDK.');
}
```

Besides, one more important parameter that can be set during the SDK initialization is `API_KEY`.

## Auth methods supported

Initialisation of SDK can be handled in a variety of methods.

For all invocations `APP_ID`, `USER_ID`, `REACT_APP_API_KEY` - parameters described previously.

Auth methods supported list is placed below:

* OAUTH_APPLE
* OAUTH_FACEBOOK
* OAUTH_GOOGLE
* FIREBASE
* TRUSTED_3RD_PARTY
* EMAIL

```javascript
let sdk = cereWebSDK(APP_ID, USER_ID, {
 token: REACT_APP_API_KEY,
 authMethod: {
   type: OAUTH_FACEBOOK,
   accessToken: AccessTokenValue,
 }});
```

```javascript
let sdk = cereWebSDK(APP_ID, USER_ID, {
 token: REACT_APP_API_KEY,
 authMethod: {
   type: 'TRUSTED_3RD_PARTY',
   externalUserId: EXTERNAL_USER_ID
   token: TOKEN
 }});
```


```javascript
let sdk = cereWebSDK(APP_ID, USER_ID, {
 token: REACT_APP_API_KEY,
 authMethod: {
   type: 'EMAIL',
   email: email@email.com,
   password: EmailPassword,
 }});
```


## Events
### Default Handlers

#### Render template

If you pass a container div upon initialization of the SDK, the SDK will render templates passed via WebSocket automatically.

#### Reward Click

In order to track rewards clicked by the user, your template must use an action-click class and set the data-event-type parameter to `REWARD_CLICKED`. Any clicks inside of this div will be registered in Cere's DDC automatically as reward clicks. You can pass data about the reward inside of data-event-value parameter.

```html
 <div class="action-click"
      data-event-type="REWARD_CLICKED"
      data-event-value="reward1">
           Click me to get a reward!
 </div>            
```


#### Dismiss Event

Similarly, users can dismiss an event, which you can inject automatically using the data-event-type of `DISMISS`. This is best practice for determining if a user actively dismissed an engagement offering. It will remove the entire engagement from the container div by default.

```html
<div class="action-click" data-event-type="DISMISS">

```


#### Impression Event

Every time an engagement is displayed to the user, an impression event is registered in Cere DDC.

### Custom Handlers


You can also register custom handlers for when an engagement is received by the websocket, or when a user dismisses an engagement. These functions are onEngagement and `onDismissEngagement`, `onGetUserKeypair`

```javascript
let apiKey = 'secret';
const sdk = await cereWebSDK(appId, yourUserId, null, apiKey);
sdk.onEngagement((template) => {
    myCustomContainer.setInnerHtml(template);
}

sdk.onDismissEngagement(() => {
    myCustomContainer.setInnerHtml('');
}

sdk.onGetUserKeypair((keyPair) => {
    //use of publicKey, privateKey
});
```


### Custom Events

In addition to the default events like IMPRESSION and REWARD_CLICKED, you can record events in the Cere platform to learn more about your app’s usage and then use that data to understand users’ patterns to segment them and set up unique campaigns to deliver personalized product offerings.

```javascript
let eventName = 'YOUR_EVENT_NAME';
let eventPayload = {'id': 26, booked: true}; // payload is optional argument
sdk.sendEvent(eventName, eventPayload);
```

## CDN + ReactJS app

For ReactJS library without NPM there is a possibility to add web.js script 

1. There is a way for adding external javascript file into react js application. If you want to deal with this case please take a look at link Option 1: CDN
2. There are different ways to add external javascript files in react. Some of them are listed below.

### React-script-tag

React-script-tag is an npm package that provides a React <script> tag that supports universal rendering. All standard <script> attributes like async, src, type, and defer are supported, including onLoad and onError callbacks.

```javascript
import ScriptTag from 'react-script-tag';

const Demo = props => (
<ScriptTag type="text/javascript" src="/path/to/resource.js" />
)
```

### React Helmet

Helmet is a React component that manages all your changes to the document head. It is another simple, beginner-friendly package that supports both server-side and client-side rendering.

Helmet takes plain HTML tags and outputs plain HTML tags.

```javascript
import {Helmet} from "react-helmet";

const Demo = props => (
<div className="application">
            <Helmet>
              <script src="/path/to/resource.js" type="text/javascript" />
            </Helmet>
            ...
        </div>
);
```


### DOM Method

Though the above solutions are simple to achieve, it requires us to add additional packages that might bulk up our application. If you have some experience coding, then you can do:

```javascript
componentDidMount () {
    const script = document.createElement("script");
    script.src = "/path/to/resource.js";
    script.async = true;
    document.body.appendChild(script);
}
```


### React Hooks

useEffect is a great way to append external JS files

Sample application code:

```javascript
useEffect(() => {
   const script = document.createElement('script');
   script.src = "https://sdk.dev.cere.io/v4.3.1/web.js";
   script.async = true;
   script.addEventListener('load', (event) => {
       try {
           sdk = window.CereSDK.web.cereWebSDK(applicationId, userId,
           authMethod: {
            //Parameters needed
        });
       } catch {
           console.log('SDK initialisation failed');
       }
       sdk.onEngagement((template) => {
//Processing of a template
       });
   });


   document.head.appendChild(script)
   return () => {
       document.body.removeChild(script);
   }
}, []);
```

## In-app messages

In-app messages are a good way to tell your users about your new app features or deliver a special personalized offer.

