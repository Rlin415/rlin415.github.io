---
layout: post
title: React, Babel, and Browserify
---

If you're familiar with React you should know that it's all about building reusable user interface components.
To make this process easier, the React team created something called JSX which is a JavaScript syntax extension
that looks similar to XML. This allows for easier readability and understanding especially because of the balanced
opening and closing tags. To use this feature however, you need a tool called Babel, which transforms the JSX
syntax back to plain old JavaScript your browser can run. Without Babel, the browser will not understand what
the JSX syntax is and end up crashing your program.

Since we're building multiple components we want to keep our code modular. But, how do we do that on the frontend?
Especially when our components are highly interrelated. With the backend it's fairly easy. Take this example, using
Node:

`lib/component1.js`

```
module.exports = {
  text: 'This is so easy to export!'
};
```

`lib/component2.js`

```
var node = require('./component1.js');
console.log(node.text); // logs 'This is so easy to export!'
```

Node and many other server side languages come with a built in module loading system that is relatively simple to use. How would we able to achieve something similar to this in the frontend?

Let's say we have a react component that holds data for a single track:

`client/views/track.js`

```
React.createClass({
  handleClick: function() {
    SC.stream('/tracks/' + this.props.song.id).then(function(player) {
      player.play(); // uses the SoundCloud API to play selected song
    });
  },
  render: function() {
    return (
      <div className='track' onClick={this.handleClick}>
        <h4 className='trackArtist'>
          {this.props.song.title}
        </h4>
      </div>
    );
  }
});
```

And we wanted to use this component in here:

`client/views/tracklist.js`

```
React.createClass({
  handleSongClick: function(song) {
    this.props.onSongClick(song);
  },
  render: function() {
    var trackList = this;
    var trackNodes = this.props.data.map(function(track) {
      return (
        <Track song={track} onSongClick={trackList.handleSongClick} key={track.id}></Track>
      );
    });
    return (
      <div className='trackList'>
        {trackNodes}
      </div>
    );
  }
});

```

How can we keep the modularity of this and export the track.js file so it can used in the tracklist.js file
at the same time? Well my friend, let me introduce you to Browserify. Browserify allows you require modules
in the browser by bundling up all your dependencies into one single file. If you've worked with Node before,
this syntax will be very familiar to you.

This is how we export a file with Browserify:

`client/views/track.js`

```
module.exports = React.createClass({
  handleClick: function() {
    SC.stream('/tracks/' + this.props.song.id).then(function(player) {
      player.play(); // uses the SoundCloud API to play selected song
    });
  },
  render: function() {
    return (
      <div className='track' onClick={this.handleClick}>
        <h4 className='trackArtist'>
          {this.props.song.title}
        </h4>
      </div>
    );
  }
});

```

This is how we would require a file:

`client/views/tracklist.js`

```
var Track = require(./track.js);
React.createClass({
  render: function() {
    var trackNodes = this.props.data.map(function(track) {
      return (
        <Track song={track}></Track>
      );
    });
    return (
      <div className='trackList'>
        {trackNodes}
      </div>
    );
  }
});

```

Fairly simple and self-explanatory. There is one more step to this process however, which is
bundling up these 2 files together. Without doing so, the requiring module functionality added by
Browserify would not work. You must do two things:

1) Install babel-preset-react. This is a preset for all React plugins, which will help us transform our JSX.
To do this we will use Node's package manager. Run this command in your CLI:

```
npm install babel-preset-react --save

```

2) Use the plugin with Browserify to bundle up our files into one. Let's say we are in our currently
in our client directory. Run this command in your CLI:

```
browserify -t [ babelify --presets [react] ] views/tracklist.js -o bundle.js

```

This will create a new file for us called bundle.js and it will look like this:

`client/bundle.js`

```
(function e(t,n,r){function s(o,u){if(!n[o]){if(!t[o]){var a=typeof require=="function"&&require;if(!u&&a)return a(o,!0);if(i)return i(o,!0);var f=new Error("Cannot find module '"+o+"'");throw f.code="MODULE_NOT_FOUND",f}var l=n[o]={exports:{}};t[o][0].call(l.exports,function(e){var n=t[o][1][e];return s(n?n:e)},l,l.exports,e,t,n,r)}return n[o].exports}var i=typeof require=="function"&&require;for(var o=0;o<r.length;o++)s(r[o]);return s})({1:[function(require,module,exports){
module.exports = React.createClass({
  displayName: 'exports',

  handleClick: function () {
    SC.stream('/tracks/' + this.props.song.id).then(function (player) {
      player.play(); // uses the SoundCloud API to play selected song
    });
  },
  render: function () {
    return React.createElement(
      'div',
      { className: 'track', onClick: this.handleClick },
      React.createElement(
        'h4',
        { className: 'trackArtist' },
        this.props.song.title
      )
    );
  }
});

},{}],2:[function(require,module,exports){
var Track = require('./track.js');
React.createClass({
  render: function () {
    var trackNodes = this.props.data.map(function (track) {
      return React.createElement(Track, { song: track });
    });
    return React.createElement(
      'div',
      { className: 'trackList' },
      trackNodes
    );
  }
});

},{"./track.js":1}]},{},[2]);

```

We now have a file that is the combination of tracklist.js and track.js that we can use as our
distribution source. Wow, how awesome is that? We kept our code modular and created a distribution
file with just one tool. Thanks Browserify.

So, next time you're using React or doing any type of work at all in the front-end, and wish to keep your
code modular, you now know how!
