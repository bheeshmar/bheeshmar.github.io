---
layout: post
title: "The path toward HATEOAS"
date: 2015-01-11 19:15:47 -0600
comments: true
categories: coding
---
We were discussing API design at work. Specifically, we were debating the
merits of adding a boolean attribute to a JSON object. Now, if you know me, you
know how much I hate booleans (more on that another time), so I argued against
it. The design consideration in question was to add a way to conditionally hide
the "like" button in the UI from the author of a post. The request was to add
an attribute to let the client know whether the current user was the author of
the post:

```javascript
{ id: 12, title: "Awesome post", ..., author: true }
```

Then the client would check that attribute in Javascript and take the
appropriate action:

```javascript
addLikeButton('/post/' + model.get('id') + '/like');

// Authors can't like their own posts
if (model.get('author')) {
  $('like-button').hide();
} else {
  $('like-button').show();
}
```

One problem I have with this is that it delegates business rules to the client.
The server already knows whether or not to allow a "like" to be applied or not,
the client is trying to convey that nicely to the user. Another problem I have
is that it is using an indirect test: we want the user to not be able to like
something, and the fact that that applies to an author is just a business rule
for today. So, why not make the attribute more explicit:

```javascript
{ id: 12, title: "Awesome post", ..., likeable: false }
```

Now the client would use more or less the same code as before, but with better clarity:

```javascript
addLikeButton('/post/' + model.get('id') + '/like');

if (model.get('likeable')) {
  $('like-button').show();
} else {
  $('like-button').hide();
}
```

Better, but again, I don't like that boolean! I would much rather have some
sort of enumerated type, as this general problem is one of exposing
capabilities:

```javascript
{ id: 12, title: "Awesome post", ..., capabilities: ["likeable", "commentable", ...] }

addLikeButton('/post/' + model.get('id') + '/like');

if (_.contains(model.get('capabilities'), 'likeable')) {
  $('like-button').show();
} else {
  $('like-button').hide();
}
```

Of course, this has its own drawbacks: the fact that things are
["stringly-typed"](http://c2.com/cgi/wiki?StringlyTyped) is not great, and you
still need to have a boolean test.

After some discussion, I thought about how it would be nice if the server sent
down the mechanism for the action as the flag. Well, isn't that
[HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)? Why not return this from the
API:


```javascript
{ id: 12, title: "Awesome post", ..., like: { rel: "like", href: "/post/12/like" } }

if (model.get('like')) {
  addLikeButton(model.get('like')['href']);
}
```

I like that the server returns the URL now and that we don't have hard-coded
URLs in the client. The presence or absence of the ability to like is used to
key the one conditional which adds the button. I'm sure there are some
drawbacks here, but so far this looks very promising!


