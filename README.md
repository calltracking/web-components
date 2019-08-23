# Example Web Components for CallTrackingMetrics

We offer a simple solution to embed our softphone into your website using [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

## One simple tag to embed the phone:

```html
<ctm-phone id='phone'></ctm-phone>
```

## Events to respond to the phone:

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

## And events you can send to the phone to control it for example outbound dial


```javascript
  document.getElementById('phone').dispatchEvent(new CustomEvent('dial', { detail: { phoneNumber: dialNumber } }));
```

