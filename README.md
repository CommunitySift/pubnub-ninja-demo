# pubnub-ninja-demo
A simple demo app showing how to get Sift Ninja working with PubNub Blocks
 
## PubNub
This tutorial assumes you are familiar with PubNub and PubNub Blocks, and have already created an app and block before. 
If not, PubNub has some [great documentation](https://www.pubnub.com/docs/blocks/introduction) to get you started.

## Import the Block and Configure Sift Ninja
* Create or sign in to your [Sift Ninja](https://www.siftninja.com) account
* Select **Add Channels**, then **PubNub Block**
* Follow the on-screen instructions to import our PubNub Block template and connect it to Sift Ninja

## Building the Chat App
PubNub has a tutorial on building a chat app in 10 lines of code which we'll be building on top of:

[PubNub: 10Chat](https://www.pubnub.com/developers/demos/10chat)

Now that we have our chat app running, we'll need to show when Sift Ninja has found content to filter.

First, we'll need to change the format of the data going to the channel.

Replace the `keyup` event listener with the following:

```javascript
input.addEventListener('keyup', function(e) {
    if ((e.keyCode || e.charCode) === 13) {
        pubnub.publish({ channel : channel, message : { text: input.value }, x : (input.value='')});
    }
});
```
This will send the chat message as an object with a `text` attribute, which is the format expected by Sift Ninja.

Replace the `message` callback handler with the following:

```javascript
message: function(obj) {
    if (obj.message.sift_ninja && obj.message.sift_ninja.response === false) {
        box.innerHTML = '&uarr; <em>Sift ninja found something bad!</em>' + '<br>' + box.innerHTML;
    }
    box.innerHTML = (''+obj.message.text).replace( /[<>]/g, '' ) + '<br>' + box.innerHTML;
}});
```

This will check for the presence of a `sift_ninja` object containing a true/false value indicating whether
or not something objectionable was found.

Save and run your demo app, enter a swear word into the chat box, and you should see a message that
Sift Ninja found objectionable content.

You can configure your Sift Ninja channel to classify content that is vulgar, racist, bullying, fighting, or 
contains sexting, or PII (personally identifiable information). What your app does with content identified as objectionable
is up to you!