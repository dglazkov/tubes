tubes
=====

# Web Platform Plumbing

This is about rationalizing and layering of the web platform APIs that need to reach outside of the box that is commonly called *the rendering engine*. Let's call these APIs **system APIs** for the purpose of this project.

For concepts and simplistic introduction, see the [What's Beneath Bedrock?](https://docs.google.com/a/glazkov.com/presentation/d/1jqAjoU22R4A4OF6k0Eg0yru2sHz6ehXUffBhOegGEvA/pub?start=false&loop=false&delayms=3000) presentation, originally given at the [Extensible Web Summit](http://lanyrd.com/2014/extensible-web-summit/).

In primitive terms, **tubes** views all system APIs as having two distinct layers:

1. The **pipes layer**, which usually consists of at least one `MessagePort` and a protocol to communicate with this port. The port is the actual manifestation of the system API in the web platform.
2. The **sugar layer**, which is the author-facing JS API.

The **sugar layer** uses the **pipes layer**.

## Motivations

* Come up with a generic method of building the **pipes layer**. While different browser vendors could have different strategies for implementing the same system API, they would use the same vehicle (`MessagePort` + message-passing protocol) to express these strategies.
* Build scientific evolution into the API development process. The proper design of a **sugar layer** needs experience and consensus of experts, whereas the **pipes layer** would have a much lower barrier to entry. Thus, we could start building **sugar layer** APIs by letting the vendors build their variety of **pipes layer** implementations, observing them in the wild, and coming up with the proper platform **sugar layer** based on these observations.


## Consistency and Process

There needs to be a simple (as simple as possible) standards process around implementing a **pipes layer** API. A quick sketch:

1. There is a standard document template to describe a **pipes layer** API
2. There's a github repository that contains all documented **pipes layers** APIs 
3. When someone wants to implement a new API, they submit a pull request with a new filled-out template
4. There's a brief disussion period, mostly to help to the submitter make sure their API isn't already redundant or implementable using existing API
5. The pull request is added to the repository.
6. When someone wants to modify the API, they follow the same process as adding a new API, plus mutually link this new API and the now-deprecated API (the template should probably provide places for these links). 
7. When someonw wants to just deprecate the API, they submit a pull request to mark the API as deprecated.

There likely be other places of developing standard practices and patterns (error handling, etc.).



