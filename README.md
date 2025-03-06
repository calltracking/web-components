## Phone Embedding API
The phone embed component allows you to embed the CallTrackingMetrics phone into your web application.
You will need to include our ctm-phone-embed javascript that exposes a web component ctm-phone-embed
tag for you to place and style in your application.
You will also need a server side component to get an access token securely from CallTrackingMetrics to authenticate the phone.

```
<script src="https://app.calltrackingmetrics.com/ctm-phone-embed-1.0.js"></script>
<ctm-phone-embed access="/ctm-phone-access"></ctm-phone-embed>
```

## Server Side Access
From your web application server you'll have to implement the /ctm-phone-access endpoint to respond to a POST request
with Content-Type application/json.
You'll need to make a POST request to https://app.calltrackingmetrics.com/api/v1/accounts/{accountId}/phone_access,
replacing accountId with the accountId you're authenticating too. You'll need to use our API key and secret to
authenticate with basic authentication over an TLS connection (e.g. https protocol).
The request should upload a JSON encoded body that includes the following properties:

email: the email address of the user authenticating to CTM.
first_name: the first name of the user.
last_name: the last name of the user.
session_id: a session id you use to uniquely identify the user. This can be used to also log the user out.
{"email":"you-user@example.com","first_name":"firstname","last_name":"lastname","session_id":"unique session id for your application"}
Your application should proxy/forward the response from CTM including in the response the additional fields: "sessionId" (note the camel case here), "email", "first_name", and "last_name" in the response.

{"status":"ok","token":"CTM generated token","valid_until": 600, "sessionId": "your session id", "email":"you-user@example.com","first_name":"firstname","last_name":"lastname","session_id":"unique session id for your application"}
Example Applications
DotNET C#: https://github.com/calltracking/ctm-csharp/tree/master/CTMPhoneExample

Node.js: https://github.com/calltracking/ctm-nodejs

## Attributes
access	A path the web component will request to get access to CTM services
auto	Should access path be fetched automatically on load.
popout	The device embed component will be loaded in a popout window on your domain this allows you to provide the full path. If you leave this attribute blank the embed device will be added automatically to the current page.
Methods
call(number:phone_number)	Start a call to the given phone_number.
hangup()	End the current call
hold()	toggle the hold status of all connected participants
mute()	toggle the mute status of the agent
transfer({what: 'receiving_number', dial: 'phone number'})	transfer the current call to another phone number
add({what: 'receiving_number', dial: 'phone number'})	add a participant to the current call. what can also be a "queue", "agent", "voice_menu"

## Events
| Event             | Description                                                         |
|-------------------|---------------------------------------------------------------------|
| ctm:ready         | Triggered when the component is connected to its device and is ready to be interacted with. |
| ctm:status        | Triggered when the agent status changes either by the agent or remotely. |
| ctm:live-activity | Triggered when the agent is live on an ongoing call or chat.        |
| ctm:accessRequest | When the phone requests access.                                    |
| ctm:end-activity  | Triggered when a call ends.                                         |
| ctm:resize        | Triggered when the phone embed is resized.                          |
| ctm:incomingCall  | Triggered when an incoming call is received.                        |
| ctm:connecting    | Triggered when a call starts to connect.                             |
| ctm:start         | Triggered when a call starts.                                       |
| ctm:failed        | Triggered if a call fails to connect.                               |
| ctm:recording_start| Triggered when a call recording starts.                             |
| ctm:recording_stop| Triggered when a call recording stops.                              |
| ctm:wrapup_start  | Triggered when an agent finishes a call and enters wrap-up.         |
| ctm:wrapup_end    | Triggered when an agent finishes their wrap-up.                     |
| ctm:access_denied | Triggered if the agent session expires and they are denied access.  |
| ctm:device_registered | Triggered when device registration completes.                   |
| ctm:task_assigned | Triggered when a task is assigned to the agent.                     |
| ctm:task_start | When a task is started by an agent.                                    |
| ctm:task_completed | When a task is completed by an agent                               |
| ctm:task_released | When a task is released by an admin from an agent                   |


## Javascript SDK
After you have included the embed javascript and implemented the server side authentication request you can use the client side SDK to interact with the web component.

```js
document.addEventListener('DOMContentLoaded', () => {
  // Add event handlers to the phone component
  const phone = document.getElementById('phone');

  // ready event may fire each time the phone device page is loaded
  phone.addEventListener('ctm:ready', (e) => {
    const agent = e.detail.agent;
    document.getElementById('status').innerHTML = 'Ready';
  });

  // each time the agent status changes the status event will fire.
  phone.addEventListener('ctm:status', (e) => {
    const status = e.detail.status;
    document.getElementById('status').innerHTML = status;
  });

  // when a phone call or activity is in progress this event fill trigger
  phone.addEventListener('ctm:live-activity', (e) => {
    const call = e.detail.activity;
  });

  // tell the device in to make a phone call when an element is clicked with a phone number - be sure the number is formatted with +E.164
  // by default the user's tracking number will be used as the caller id
  document.querySelectorAll('.call-button').forEach((el) => {
    el.addEventListener('click', (e) => {
      e.preventDefault();
      const number = e.currentTarget.getAttribute('href').replace('tel:', '');
      phone.call(number);
    });
  });
});
```

You can also apply styles to the phone emebed element for example:

```html
ctm-phone-embed {
  height: 750px;
  min-height: 750px;
  max-height: 750px;
  width: 450px;
  display: block;
  box-shadow: 0 1px 8px #ccc;
  margin: 0;
  transition: height 0.5s ;
  padding: 0;
  position: relative;
  z-index: 1;
}
```

