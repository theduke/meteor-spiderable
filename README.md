# spiderable

## Why this fork was needed

This is a fork of the regular spiderable package of https://github.com/meteor/meteor.
This fork does not use the /dev/stdin workaround for handling the PhantomJS result,
and therefore works well on Docker containers, where /dev/stdin can be an issue.

The changes to the original package are minimal (only 3 lines are different).

Inspired by https://github.com/ongoworks/spiderable.


`spiderable` is part of [Webapp](https://www.meteor.com/webapp). It's one possible way to allow web search engines to index a Meteor application. It uses the [AJAX Crawling specification](https://developers.google.com/webmasters/ajax-crawling/) published by Google to serve HTML to compatible spiders (Google, Bing, Yandex, and more).

When a spider requests an HTML snapshot of a page the Meteor server runs the client half of the application inside [phantomjs](http://phantomjs.org/), a headless browser, and returns the full HTML generated by the client code.

In order to have links between multiple pages on a site visible to spiders, apps must use real links (eg `<a href="/about">`) rather than simply re-rendering portions of the page when an element is clicked. Apps should render their content based on the URL of the page and can use [HTML5 pushState](https://developer.mozilla.org/en-US/docs/DOM/Manipulating_the_browser_history) to alter the URL on the client without triggering a page reload. See the [Todos example](http://meteor.com/examples/todos) for a demonstration.

When running your page, `spiderable` will wait for all publications to be ready. Make sure that all of your [`publish functions`](#meteor_publish) either return a cursor (or an array of cursors), or eventually call [`this.ready()`](#publish_ready). Otherwise, the `phantomjs` executions will fail.

## Notes

If you deploy your application with `meteor bundle`, you must install `phantomjs` ([http://phantomjs.org](http://phantomjs.org/)) somewhere in your `$PATH`. If you use `meteor deploy` this is already taken care of.