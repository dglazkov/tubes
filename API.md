```js
partial interface Navigator {
  Promise<any> connect(DOMString url, DOMString type, MessagePort port);
};
```

The ```navigator.connect``` attempts to establish a connection with a service.

The service **instance** is identified by the ```url``` parameter, and the type of the service is identified by the ```type``` parameter. The ```port``` parameter is a MessagePort that is transferred to the service.

For example, here's how I would try to talk to the **socialnetwork.com**'s Service Worker:

```js
var contactsChannel = new MessageChannel();
var contacts = contactsChannel.port1;
navigator.connect('https://socialnetwork.com', 'standard/contacts/1.3',
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

## Receving/Handling Connect Requests

If a Service Worker is installed and is able to handle the **url** within its scope, it receives a ```connect``` event that uses the ```ConnectEvent``` interface:

```js
[Constructor]
interface ConnectEvent : Event {
  readonly attribute DOMString origin;
  readonly attribute DOMString type;
  void accept();
  void reject();
};
```

For example, here's how the attempt above to connect to the **socialnetwork.com** would be handled by its Service Worker:

```js
/// in https://socialnetwork.com Service Worker
this.addEventListener('connect', function(e) {
  if (e.origin == 'https://happycustomer.com' && e.type == 'standard/contacts/1.3') {
    // yes, I will be happy to handle your request.
    e.accept();
  } else {
    // no, you evil basterd.
    e.reject();
  }
});
```

## Web Intents Redux

Because we have an explicit notion of advertising type along with the url, we open the path for enabling user agent-based mediation of connections.

For instance, if we make the ```url``` argument optional (or have a ```*``` as one of the choices), the caller of ```navigator.connect``` could simply ask for **standard/contacts/1.3** -- without identifying a specific service to handle the connection. The user agent could then choose (or ask user to choose) the most appropriate handler. And thus, the Web Intents are back with the vengeance.

Obviously, we would need more API to enable service providers to advertise types, but this can be done as a natural, incremental step.
