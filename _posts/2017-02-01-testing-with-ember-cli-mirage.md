---
layout: post
title: Notes on using Ember CLI Mirage
date: 2017-03-02
---

Almost every Ember app needs to communicate with an API and every Ember app needs tests (eh, right?). So what do you do when you have to make API calls in your Ember tests?

Ember CLI Mirage can solve this issue in an elegant way. I like to look at it as a mock node server that is primarily used for testing but can also be used by the actual ember app for demonstrating your app!

## Negatives
Of course there are always try offs. Here are some issues I commonly see:
- It's hard to setup
- It's too much overhead
- Have to maintain parity with my real API

These things can be true. My advice is that you should start small and incrementally add more as you need. After you set it up in a projects it's easy to add to your tests.

## Benefits
There are a lot of good reasons why you should use Migrate. Here are a couple:
- Can be run in CI
- Dynamic endpoints allows for more in-depth acceptance tests
- Could be used as a minimal server for validating ideas

## Getting started
It's relatively quick to get started also. Here are the basic steps:
1. Install addon
2. Do some configuration
3. Setup endpoints<br>
3a. First with real stubs<br>
3b. Replace with fixtures<br>
3c. Create dynamic models using factories<br>
4. Run your tests!


## Install addon
`ember install ember-cli-mirage`

## Do some configuration
In `environment.js`  set `ENV['ember-cli-mirage']` to `true` for testing and `false` for everything else (unless you want to use it for different environments like development and production). More configurations at [Mirage's docs](http://www.ember-cli-mirage.com/docs/v0.2.x/configuration/).

## Begin with stubs
You'll thank yourself. In my opinion, it's okay to start off with basic tests and drill as you progress.

Lets say I have a todo list app and I want to test if I click on the checkbox to finish a task, that it goes away. If you already have a real API endpoint for getting my latest tasks, I can just copy the payload from that and stub out that request in Mirage.

You can do it by doing something like this:
```javascript
this.get('/tasks', () => {
    return {
      "data": [
        {
          "id": "b3c21d7a-2843-47f1-89af-1ef1eb195966",
          "type": "task",
          "attributes": {
            "name": "Do Homework",
            "modified-time": "2016-09-21T17:09:39.566+00:00"
          }
        },
        {
          "id": "24cc0aca-2034-4d07-bf4b-ddda5d161416",
          "type": "task",
          "attributes": {
            "name": "Read book",
            "modified-time": "2016-09-20T17:32:09.697+00:00"
          }
        }
      ]
    }
});
```

So now when testing Ember will fetch from here. Simple enough right?

Well what if you need to create a task?
```javascript
this.post('/tasks', (schema, request) => {
    return {
      "data": [
        {
          "id": "b41fe5e4-24ce-4e2c-8af4-8606dcecb025",
          "type": "task",
          "attributes": {
            "name": "New task!",
            "modified-time": "2016-09-22T10:02:09.016+00:00"
          }
        }
      ]
    }
});
```
This will only return one static task, which obviously isn't ideal but it's a place to start!

Stubbing out can be good to quickly get tests up and running but that most likely won't help you cover most your app functionality.

## Create dynamic models using factories
Time to replace `/tasks` with Factory Girl style factories. Here is where the real magic happens! Lets make it so that it dynamically creates new tasks so we can test better.
```javascript
this.post('/tasks', (schema, request) => {
  const params = JSON.parse(request.requestBody);

  if (!params.name) {
    return new Mirage.Response(422, {some: 'header'}, {errors: {title: ['cannot be blank']}});
  } else {
    return schema.tasks.create(params);
  }
});
```


## (Somewhat) Working example
Here is a [Ember Twiddle](https://ember-twiddle.com/03aa10f0323d50ccd737154476f3edb9?openFiles=mirage.scenarios.default.js%2C) ([based off this Twiddle](https://ember-twiddle.com/03aa10f0323d50ccd737154476f3edb9?openFiles=mirage.config.js%2C)) that really demos the abilities of Ember Mirage. It has Mirage models (factories), serializer, endpoints, and also some tests. I highly recommend checking it out.

<div style="position: relative; height: 0px; overflow: hidden; max-width: 100%; padding-bottom: 56.25%;"><iframe src="https://ember-twiddle.com/03aa10f0323d50ccd737154476f3edb9?fullScreen=true" style="position: absolute; top: 0px; left: 0px; width: 100%; height: 100%;"></iframe></div>


## Skipping Mirage's endpoint
Though code coverage isn't perfect I recommend using it. [ember-cli-code-coverage](https://github.com/kategengler/ember-cli-code-coverage) is a good option. You'll have to allow a passthrough for it so that it doesn't get caught by Mirage. You can do that by adding this in your `config.js`:
```javascript
this.passthrough('/write-coverage');
```

Namespace requests by adding `this.namespace = '/api/v1';` in `config.js`

## Other tips
- This is a very broad overview, there is so much more. Thank you for making a great library [@samselikoff](https://twitter.com/samselikoff)!

[Ember CLI Mirage homepage](http://www.ember-cli-mirage.com/)

Tested against: Ember 2.10.0, Ember Mirage CLI 0.2.2
