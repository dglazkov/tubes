tubes
=====

# Web Platform Plumbing

This is about rationalizing and layering of the platform APIs that need to reach outside of the box that is commonly called *the rendering engine*.

For motivations, concepts and simplistic introduction, see the [What's Beneath Bedrock?](https://docs.google.com/a/glazkov.com/presentation/d/1jqAjoU22R4A4OF6k0Eg0yru2sHz6ehXUffBhOegGEvA/pub?start=false&loop=false&delayms=3000) presentation, originally given at the [Extensible Web Summit](http://lanyrd.com/2014/extensible-web-summit/).

In primitive terms, **tubes** views all APIs that reach outside as having two distinct layers:

1. The **pipes layer**, which usually consists of at least one `MessagePort` and a protocol to communicate with this port.
2. The **sugar layer**, which is the author-facing JS API.

The **sugar layer** relies on the **pipes layer**.

