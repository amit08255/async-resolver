# async-resolver [![Build Status](https://travis-ci.org/ApoorvSaxena/async-resolver.svg?branch=master)](https://travis-ci.org/ApoorvSaxena/async-resolver) [![Coverage Status](https://coveralls.io/repos/github/ApoorvSaxena/async-resolver/badge.svg?branch=master)](https://coveralls.io/github/ApoorvSaxena/async-resolver?branch=master) [![dependencies Status](https://david-dm.org/ApoorvSaxena/async-resolver/status.svg)](https://david-dm.org/ApoorvSaxena/async-resolver)

<h1 align="center">
	<img src="async-resolver-logo.png" align="center">
</h1>

AsyncResolver.js implements a PubSub architecture where subscribers of events are decision makers (return promise when they receive an event) and after publishing an event, publisher gets the decision of the subscribers. Supports both Node and browser.

### Where to use?

There are situations where we want to maintain distinct subscribers of an event, though want to act on the basis of how subscribers react. AsyncResolver.js is the solution for this need, it's an amalgamation of pub sub architecture and promises to provide decision making capability in asynchronous environment.
> For example: Let's consider a case where there are several components on a webpage, whose state can be changed by the user and we make every component to subscribe as listener to listen to a page transition so that we can check if a user is trying to move without saving data.

> Now, when an individual clicks on a link, we publish an event mentioning of the transition of the user from the page, though we want to ask every listener (or component) if user has made any changes to their state and is moving without saving them. In case there are any unsaved changes in any of the component, then we cancel the transition and instead dispay an information dialog to the user asking him to save information before proceeding further.

## Install

```sh
### NPM
npm install async-resolver

### Yarn
yarn add async-resolver
```


## Usage

```js
const AsyncResolver = require('async-resolver');
let resolver = new AsyncResolver();

resolver.subscribe('locationChange', () => {
	// custom logic
    return Promise.resolve();
});

resolver.subscribe('locationChange', () => {
	// custom logic
    return Promise.reject();
});

resolver
	.publish('locationChange', {
		promiseMethod: 'any'
	})
	.then(() => console.log('location change allowed'))
	.catch(() => console.log('location change denied'))
```

## API

### AsyncResolver()

Returns `AsyncResolver` function that can be instantiated.

### subscribe(eventName, callback)

Allows subscribing to the event and executing the callback when an event is published.

#### eventName

Type: `string`

Event published.

#### callback

Type: `function`

Callback to be executed when the publish event is received.

### publish(eventName, options)

Returns a promise, that defines the decision of subscribers.

#### eventName

Type: `string`

Event published.

#### options

Type: `object`

Options to be passed while publishing to an event.

##### options.promiseMethod

Type: `string`

Method to be applied at the collection of promises such as `all`, `any` etc. Here's [list of methods](http://bluebirdjs.com/docs/api/collections.html) supported by Promise collection.

## License

MIT © [Apoorv Saxena](https://apoorv.pro/)