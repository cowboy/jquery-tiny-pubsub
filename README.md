# jQuery Tiny Pub/Sub

A really, really, REALLY tiny pub/sub implementation for jQuery.

_(see the [original gist](https://gist.github.com/661855))_

## Getting Started
Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/cowboy/jquery-tiny-pubsub/master/dist/tiny-pubsub.min.js
[max]: https://raw.github.com/cowboy/jquery-tiny-pubsub/master/dist/tiny-pubsub.js

Example usage:

```html
<script src="jquery.js"></script>
<script src="dist/tiny-pubsub.min.js"></script>
<script>
// Creates a "named" logging function.
function createLogger(name) {
  return function(_, a, b) {
    // Skip the first argument (event object) but log the name and other args.
    console.log(name, a, b);
  };
}

// Subscribe to the "foo" topic (bind to the "foo" event, no namespace).
$.subscribe('foo', createLogger('foo'));
// Subscribe to the "foo.bar" topic (bind to the "foo" event, "bar" namespace).
$.subscribe('foo.bar', createLogger('foo.bar'));
// Subscribe to the "foo.baz" topic (bind to the "foo" event, "baz" namespace).
$.subscribe('foo.baz', createLogger('foo.baz'));

// Publish arbitrary values.
$.publish('foo', [1, 2]);
// logs:
// foo 1 2
// foo.bar 1 2
// foo.baz 1 2

$.publish('foo.bar', [3, 4]);
// logs:
// foo.bar 3 4

$.publish('foo.baz', [5, 6]);
// logs:
// foo.baz 5 6

$.unsubscribe('foo.bar');
$.publish('foo', [7, 8]);
// logs:
// foo 7 8
// foo.baz 7 8
</script>
```

## Documentation
_Note: Ignore the first argument passed to your subscribed callbacks (the jQuery event object)._

_Another Note: [Previous versions](https://gist.github.com/661855/2c518edd29b744d04bff55ec9a2a5d12afe41595) (v0.4+) were written in an attempt to remove the first argument and create a more future-proof API, but unfortunately this resulted in much less elegant, larger and slower code. The point of this plugin is to be TINY, to be used in situations where only size (not performance or usability) is the primary concern (tweets, code golf, etc).**_

I frequently see comments about how jQuery's events system has unnecessary overhead that precludes it from being used as the core of a Pub/Sub implementation. The jQuery events system is tried-and-true, having been architected to be both fast and robust, and the vast majority of users of this plugin should never encounter any kind of performance issues.

Because this plugin's `$.subscribe`, `$.unsubscribe` and `$.publish` methods all use the jQuery [.on()](http://api.jquery.com/on/), [.off()](http://api.jquery.com/off/) and [.trigger()](http://api.jquery.com/trigger/) methods internally, those methods' complete signatures are available to you.

You can use [namespaces](http://docs.jquery.com/Namespaced_Events) for more control over unsubscribing and publishing.

Just use this handy terminology guide (jQuery events term → Pub/Sub term), and everything should make sense:

 * on → subscribe
 * off → unsubscribe
 * trigger → publish
 * type → topic

In addition, should you need it, these methods are fully compatible with the [jQuery.proxy()](http://api.jquery.com/jQuery.proxy/) method, in case you not only want more control over to which context the subscribed callback is bound, but want to be able to very easily unsubscribe via callback reference.

Regarding performance: If at some point, your application starts processing so many messages that performance issues start to develop, you could always write a replacement "jQuery Not-So-Tiny Pub/Sub" plugin with the same API and just drop it in as a replacement to this plugin. But keep in mind that you'll also need to add in the aforementioned features, too.

## Release History
2013-01-29 - v0.7.0 - First official release.
