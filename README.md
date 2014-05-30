tubes
=====

# Web Platform Plumbing

This is (an in-progress tinkering) about rationalizing and layering of the web platform APIs that reach outside of the box that is commonly called *the rendering engine*. Let's call these APIs **system APIs** for the purpose of this project.

For concepts and simplistic introduction, see the [What's Beneath Bedrock?](https://docs.google.com/a/glazkov.com/presentation/d/1jqAjoU22R4A4OF6k0Eg0yru2sHz6ehXUffBhOegGEvA/pub?start=false&loop=false&delayms=3000) presentation, originally given at the [Extensible Web Summit](http://lanyrd.com/2014/extensible-web-summit/).

In primitive terms, the **tubes** view all system APIs as having two distinct layers:

1. The **pipes layer**, which usually consists of at least one `MessagePort` and a protocol to communicate with this port. The port is the actual manifestation of the system API in the web platform.
2. The **sugar layer**, which is the author-facing JS API.

The **sugar layer** uses the **pipes layer** and should be implementable in plain JS.

## Motivations

* Come up with a generic method of building the **pipes layer**. While different browser vendors could have different strategies for implementing the same system API, they would use the same vehicle (`MessagePort` + message-passing protocol) to express these strategies.
* Build scientific evolution into the API development process. The proper design of a **sugar layer** needs experience and consensus of experts, whereas the **pipes layer** would have a much lower barrier to entry. Thus, we could start building **sugar layer** APIs by letting the vendors build their variety of **pipes layer** implementations, observing them in the wild, and coming up with the proper platform **sugar layer** based on these observations.


## Consistency and Process

There needs to be a simple (as simple as possible) standards process around implementing a **pipes layer** API. A quick sketch:

1. There is a standard document template to describe a **pipes layer** API.
2. There's a github repository that contains all documented **pipes layers** APIs.
3. When someone wants to implement a new API, they submit a pull request with a new filled-out template.
4. There's a brief disussion period, mostly to help to the submitter make sure their API isn't already redundant or implementable using existing API.
5. The pull request is added to the repository.
6. When someone wants to modify the API, they follow the same process as adding a new API, plus mutually link this new API and the now-deprecated API (the template should probably provide places for these links). 
7. When someonw wants to just deprecate the API, they submit a pull request to mark the API as deprecated.

There likely be other places of developing standard practices and patterns (error handling, etc.).

## Safety

The **tubes** makes this distinction between the **platform** and the **user agent**.

* The **platform** is the union of all features (APIs, protocols, behaviors, etc.) that is implemented by all vendors
* The **user agent** is the actor that makes decisions on which subset of the **platform** is acessible to the author.

Some of these decisions are made due to implementation deficiencies (when the browser doesn't support a feature, for example), and some are made as a result of negotiating the balance of power between the user and the author (for instance, when the user does not want the author to have information or control, exposed by a feature).

In he same vein, it is up to the **user agent** to  answer the questions on whether a given **pipes layer** API is accessible to the author. One simplistic pattern would be for the **user agent** to install a _valve_ in front of the primordial `MessagePort`: something that looks like the primordial `MessagePort`, but actually has a whitelist of **pipes layer** APIs that are deemed safe, and fails when an "unsafe" API is requested.

Some vendors (such as Cordova) would never want to block any of **pipes layer**, and some vendors would want to block most if not all of them. This is okay.

