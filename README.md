# Apply decorator

This decorator will return property descriptor which value will be the result of
invoking function from first decorator's argument with:
 - decorated function as first argument
 - rest decorator's arguments

Example
```
Before:
class Component {
    constructor() {
        this.debouncedMethod = debounce(this.debouncedMethod, 250);
    }

    debouncedMethod() {
        console.log('i am debounced');
    }
}

After:
class Component {
    @apply(debounce, 250)
    debouncedMethod() {
        console.log('i am debounced');
    }
}
```

Pay attention: for now decorators can be used with transpilers such as [Babel](http://babeljs.io)

**To Babel 6 users:**
For now decorators syntax is not final. So you may use [babel-plugin-transform-decorators-legacy](https://github.com/loganfsmyth/babel-plugin-transform-decorators-legacy).

**To autobind-decorator users:**
Make sure you apply callback before autobind-decorator as it returns accessor descriptor.

Example:
```
Bad:
    @apply(someFunc)
    @autobind
    handleSomething() { ... }

Good:
    @autobind
    @apply(someFunc)
    handleSomething() { ... }
```

Installation:

    % npm install apply-decorator

Example:

    import apply from 'apply-decorator'

    let count = 0;
    const logToConsole = function(original) {
        return function() {
            console.log(`Original called ${++count} times`);
            original.apply(this, arguments);
        };
    };

    class Component {
      render() {
        return <div onClick={ this.handleClick } />;
      }

      @apply(logToConsole)
      handleClick() {
        // do useful work here
      }
    }
