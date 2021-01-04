# FingerprintJS Pro working in Vue

## Using create-vue-app:

First, create an app with the following command:

```bash
$ npx create-vue-app my-project
```

After the app is created, enter the directory:

```bash
$ cd my-project
```

and host a server locally:

```bash
$ yarn dev
```

If all goes well, you should be notified in terminal that your app is being hosted on port 4000.
Any changes you make to the source files will automatically update.

## Editing files

Inside the `src/components` folder, you will find a file called `App.vue`. This is the only file altered in order to use FingerprintJS Pro for this example.

1. First, you will want to install the npm package for FingerprintJS. Go to the console and run:

```bash
$ npm install @fingerprintjs/fingerprintjs-pro --save
```

You can now import the package at the top of the script section:

```javascript
// App.vue
import FingerprintJS from "@fingerprintjs/fingerprintjs-pro";
```

2. Using Tokens.

If you assign your tokens to variables directly in the file, make sure to protect them by whitelisting the domains in the customer dashboard. Another option is to keep these tokens in a `.env` file and reference them from there. For the server API, it is recommended to use basic auth in request headers instead of using a token. You can read more about it <a href="https://dev.fingerprintjs.com/docs/server-api#authentication" target="_blank"> here</a>.

3. Getting a visitor ID.

In order to get a visitor ID, use the FingerprintJS object:

```javascript
// App.vue
mounted: function () {
    FingerprintJS.load({
      token: BROWSERAPIKEY,
    })
      .then((fp) => fp.get())
      .then((result) => {
        this.visitorId = result.visitorId;
      });
},
```

In this example, I am using the pre-defined `mounted` method inherited from the Vue class. This method will be called when the window is loaded, making the request for the visitorID automatic. The value is then stored to a class attribute called `visitorId` which can then be referenced.

4. Querying the server API for visitor history:

The following function will query the server API. Please note that if your account is registered to the EU region, your base URL should be: https://eu.api.fpjs.io.

In the query below the visitorID and token is passed, as well as a "limitTo" variable, which will limit the amount of responses returned by the API. You can learn more about the query options <a href="https://dev.fingerprintjs.com/docs/server-api" target="_blank"> here</a>

```javascript
// App.vue
methods: {
    // When the button is clicked, this function will
    // run and get visitor history from the server
    callServerAPI: function () {
      fetch(
        `https://api.fpjs.io/visitors/${this.visitorId}?limit=${limitTo}&token=${SERVERAPIKEY}`
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.responseSummary = `Received history of ${data.visits.length} visits:`;
          data = JSON.stringify(data.visits, null, 4);
          this.serverData = data;
        });
    },
},
```

As you can see, a method has been saved for the class component and will be called from the `onclick` attribute in the html button :

```html
<button v-on:click="callServerAPI">Get Visit History</button>
```

## Further Steps

If you would like to learn more, please visit our [Documentation](https://dev.fingerprintjs.com/docs) to see best practices for using FingerprintJS and guides on how to implement them.

If you have any questions, please contact us at [support@fingerprintjs.com](support@fingerprintjs.com).
