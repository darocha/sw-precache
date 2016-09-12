# sw-precache? sw-toolbox? What's the difference?

## sw-precache ⇒ app shell, sw-toolbox ⇒ dynamic content

It's very common to use `sw-precache` and `sw-toolbox` in conjunction with each
other, especially when following the
[App Shell](https://github.com/GoogleChrome/sw-precache/blob/master/GettingStarted.md#app-shell) +
[dynamic content](https://github.com/GoogleChrome/sw-precache/blob/master/GettingStarted.md#dynamic-content)
model.

The service worker that `sw-precache` generates handles the local asset
versioning and uses cache-first strategy for your App Shell.

`sw-toolbox` handles runtime caching strategies for dynamic content, such as API
calls, third-party resources, and large or infrequently used local resources
that you don't want precached.

## Using them together via sw-precache's runtimeCaching

We wanted to make it easier for developers to use the two libraries together.
Because `sw-precache` has to be directly integrated with your build environment
and must be responsible for outputting your top-level service worker file, it
made the most sense as an integration point to give `sw-precache` the ability to
include the `sw-toolbox` code and configuration alongside the its own
configuration. Using the
[`runtimeCaching`](https://github.com/GoogleChrome/sw-precache#runtimecaching-arrayobject)
configuration option in `sw-precache` is a shortcut that accomplishes what you
could do manually by importing `sw-toolbox` in your service worker and writing
your own routing rules. You can confirm this by looking at a
[sample `service-worker.js`](https://github.com/GoogleChrome/sw-precache/blob/5fede7148a516c0bf555e9580c74b9accffe721c/service-worker.js#L206)
file generated by `sw-precache` when the
[`runtimeCaching` option is used](https://github.com/GoogleChrome/sw-precache/blob/9118fe1e3905f959198f3bdd21004238b1c884f5/demo/gulpfile.js#L55).

## The future

This relationship might change at some point in the future once changes to the
service worker specification make it possible to
[relax the requirement](https://github.com/GoogleChrome/sw-precache/issues/147)
that `sw-precache` generate the top-level service worker.