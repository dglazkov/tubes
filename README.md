tubes
=====

# Web Platform Plumbing

This is about rationalizing and layering of the web platform APIs that need to reach outside of the box that is commonly called *the rendering engine*. Let's call these APIs **system APIs** for the purpose of this project.

For motivations, concepts and simplistic introduction, see the [What's Beneath Bedrock?](https://docs.google.com/a/glazkov.com/presentation/d/1jqAjoU22R4A4OF6k0Eg0yru2sHz6ehXUffBhOegGEvA/pub?start=false&loop=false&delayms=3000) presentation, originally given at the [Extensible Web Summit](http://lanyrd.com/2014/extensible-web-summit/).

In primitive terms, **tubes** views all system APIs as having two distinct layers:

1. The **pipes layer**, which usually consists of at least one `MessagePort` and a protocol to communicate with this port.
2. The **sugar layer**, which is the author-facing JS API.

The **sugar layer** relies on the **pipes layer**.

## Motivations

* Come up with generic method of building the **pipes layer**. While different browser vendors could have different strategies for implementing the same system API, they would use the same vehicle (`MessagePort` + message-passing protocol) to express these strategies.
* Build scientific evolution into the API development process. The proper design of a **sugar layer** needs experience and consensus of experts, whereas the **pipes layer** would have a much lower barrier to entry. Thus, we could start building **sugar layer** APIs by letting vendors build their variety of **pipes layer** implementations, observing them in the wild, and coming up with proper platform **sugar layer** based on these observations.
