# Example Web Components for CallTrackingMetrics

We offer a simple solution to embed our softphone into your website using [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

## One simple tag to embed the phone:

```html
<html>
<head>  
  <script src="https://app.calltrackingmetrics.com/softphone-component.js"></script>
</head>
<body>
  <ctm-phone></ctm-phone>
</body>
</html>
```

## Events to respond to the phone:

Event	Description

* **resize**
    Sent when the soft phone is requesting to resize. you can subscribe to this event to allow the phone to expand or shrink depending on the content being displayed inside the phone.
    *event.detail.data*:
      ```{width: 350, height: 900}```
* **incoming**
    Sent when the soft phone starts ringing, so an agent can answer the phone. You may use this to ensure the phone is visible to the agent.
    *event.detail.data*:
      ```{ sid: 'CA…', from: '+1dddddddddd' }```
* **answered**
    When an agent answers the incoming phone call this event will trigger letting you know which call the agent has answered.
    *event.detail.data*:
      ```{ call: { .. }, callId: 1337}```
* **loaded**
    When the call record is loaded in the agents phone. This event will include full details of the answered call.
    *event.detail.data*:
      ```{ call: { .. }, callId: 1337}```
* **end**
    When the call the agent answered ends this event is triggered.
    *event.detail.data*:
    ```{ callId: 1337}```
* **login**
    When soft phone requires a user login
    *event.detail.data*:
      ```{ }```

```javascript
  $('#phone').on('resize', function(e) {
    //console.log("requested to resize the phone element", e.detail);
  });
  $('#phone').on('end', function(e) {
    console.log("phone call or chat has ended", e.detail);
  });
  $('#phone').on('answered', function(e) {
    console.log("phone call or chat has been answered", e.detail);
  });
  $('#phone').on('loaded', function(e) {
    console.log("information about the incoming phone call or chat has been loaded", e.detail);
  });

```

## Methods available to control the phone

The element has methods that allow you to control the phone externally.  You might use these methods to add additional functionality or click to dial type features.

* dial(phoneNumber, trackingNumber) make a phone call to the given phoneNumber from the given trackingNumber (optional).  
  * The phoneNumber and trackingNumber should be in +E.164 format. 
  * If the optional trackingNumber is not passed, the first tracking number found in the account will be used as the **outbound caller id**.
* muteCall() toggle the agents microphone.
* answerCall() answer an incoming phone call.
* hangupCall() hangup a live call.
* sendDigit(digit) send a digit from the dial pad to the phone e.g. 1, 2, 3, 4, *, #, etc…

## Events

You can also send the phone events

```javascript
  document.getElementById('phone').dispatchEvent(new CustomEvent('dial', { detail: { phoneNumber: dialNumber } }));
```

