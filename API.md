```js
partial interface Navigator {
  Promise<any> connect(DOMString url, MessagePort port);
};
```

The ```navigator.connect``` attempts to establish a connection with a service. A service could be represented by a Service Worker. The ```port``` parameter is a MessagePort that is transferred to the service.

The service is identified by the ```url``` parameter. 

For example, here's how I would try to talk to the **socialnetwork.com**'s Service Worker:

```js
var contactsChannel = new MessageChannel();
var contacts = contactsChannel.port1;
contacts.onmessage = function(e) {
  // I WUV CONTACTS!!
}

navigator.connect('https://socialnetwork.com', contactsChannel.port2).then(function() {
  contacts.postMessage('gimmeSomeContacts');
}, function() {
  console.log('we failed to connect, maybe next time?');
});
```

The failure of promise indicates that the connection is not possible. The actual reason (denied, service unavailable) is not communicated back.

## Receving/Handling Connect Requests

If a Service Worker is installed and is able to handle the **url** within its scope, it receives a ```connect``` event that uses the ```ConnectEvent``` interface:

```js
[Constructor]
interface ConnectEvent : Event {
  readonly attribute DOMString origin;
  Promise<MessagePort> accept();
  void reject();
};
```

For example, here's how the attempt above to connect to the **socialnetwork.com** would be handled by its Service Worker:

```js
/// in https://socialnetwork.com Service Worker
this.addEventListener('connect', function(e) {
  if (e.origin == 'https://happycustomer.com') {
    // yes, I will be happy to handle your request.
    e.accept().then(port) {
      // port is given to you only after you accept the request.
      port.onmessage = function() {
        // I am listening...
      });
    });
  } else {
    // no, you evil basterd.
    e.reject();
  }
});
```

