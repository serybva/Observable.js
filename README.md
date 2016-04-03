# Observable.js
Simple but effective Javascript class allowing to listen for variables changes.
Watched vars can be objects or primitives.

## Usage

    var fooBar = {foo: 'bar'};
    //Object will be replaced by the instance of Observable
    //wrapping object properties, thus allowing us to listen for changes
    //on those properties

    fooBar = new Observable(fooBar, new Events());

    //The second argument must be an instance of any event dispatcher
    //implementation providing at list a method named emit, with this signature:
    //emit(eventName, args....);
    //Both EventEmitter module in NodeJS or Emitter (https://github.com/component/emitter.git)
    //for browser environment are perfect

    fooBar.on('value-changed', function() {//Listen for changes in fooBar object
        alert(fooBar.originalProps());
    });

    fooBar.foo = 'baz';//Alert 'baz'

## Methods

    Observable(object, eventDispacther)
*As mentioned above, eventDispacther must be an instance of a event dispatcher
implementation providinga least a emit method to emit events when a watched
variable is subject to change*

    set(prop, value)
*Affects the given value to the given property contained in the Observable instance*

    on(event, callback)
*Binds a listener to the given event*

    get(prop)
*Returns the value of the given property in the watched object, if the initial
    variable is not an object then the only possible value will be returned, the
value of the watched variable*

    originalProps()
*Observable affects the properties of the watched object to it's own instance,
thus to avoid overwritting it's own properties by accident, it prefixes
every property name of the watched object by the following: "_Observed_".

Hence you can't just pass you monitored object as an argument to a function
which will try to access it's properties by their original keys and except
it to work.

You first need to retrieve the original object by calling the originalProps()
method, which will return an object with the same structure as the original,
but not the same values is they changed in the meantime.*

## Events

    value-changed
*Emitted when a property of a monitored object, or the value of the watched variable
changed*
