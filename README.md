# React Mouse Events

## Objectives

1. Describe how we might use mouse events to enhance interactivity
2. Describe how we can attach and use click events

## Event pooling
Since React has its own implementation for events (called `SyntheticEvent`), it allows us to take advantage of several
techniques to increase the performance of our applications. One such technique is called 'event pooling'. It might sound
complicated, but it's actually pretty simple.
 
Event pooling means that whenever an event fires, its event data (an object) is sent to the callback. The object is then
immediately cleaned up for later use. This is what we mean by 'pooling': the event object is in effect being sent back
to the pool for use in a later event.

This is something that trips up a lot of people, and you might have run into it yourself when inspecting data in the
browser. For example, let's say we have this React component:

```js
export default class Clicker extends React.Component {
  constructor() {
    super();
    
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick(event) {
    console.log(event);
  }

  render() {
    return (
      <div></div>
    );
  }
}
```

If we click the button and then inspect the logged out object in our console, we'd notice that all properties are `null`
again. By the time we inspect the object in our browser, the event object will have already been returned to the pool.
This means that we can't access event data in an asynchronous manner by saving it in the state, or running a timeout and
_then_ accessing the event again.

You usually don't need to access your event data in an asynchronous manner like described above, but if you do, there
are two options: you either store the data you need in a variable (e.g. `const target = event.target`), _or_ we can
make the event persistent by calling that method: `event.persist()`.

## Accessing event data
Now that we know about this little caveat, it's time to take a deeper look at the actual event being passed through. A
`SyntheticEvent` event has all of its usual properties and methods. This includes its type, target, mouse coordinates,
and so on.

For example, if we wanted to get the target of an event, we'd use `event.target`. If we want to prevent a default
action whenever an event happens, we call `event.preventDefault()`. This is all super similar to regular browser events
and should feel very familiar!

## Resources
- [React Events](https://facebook.github.io/react/docs/events.html)
