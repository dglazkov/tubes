```js
partial interface Navigator {
  Promise<any> connect(DOMString url, DOMString type, MessagePort port);
};
```

The ```navigator.connect``` attempts to establish a connection with a service.

The service **instance** is identified by the ```url``` parameter, and the type of the service is identified by the ```type``` parameter. The ```port``` parameter is a MessagePort that is transferred to the service.

For example, here's how I would try to talk to the ```socialnetwork.com```'s Service Worker:

```js
var contactsChannel = new MessageChannel();
var contacts = contactsChannel.port1;
navigator.connect('http://socialnetwork.com', 'standard/contacts/1.3',
    contactsChannel.port2).then(function() {
  contacts.addEventListener('message', function() {
    // ZOMG, contacts!
  });
  contacts.postMessage('gimmeSomeContacts');
}, function() {
  console.log('we failed to connect, maybe next time?');
});
```

The failure of promise indicates that the connection is not possible. The actual reason (denied, service unavailable) is not communicated back.
