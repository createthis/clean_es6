Any time you see more than a level or two of indent, this is a sign that it is time to refactor your code. Let's
look at an example. Open up your javascript console and copy and paste these examples to see them run.

# Don't do this
```js
{
  let make_spaghetti = (first, second, third) => {
    if (first) {
      for (let x = 0; x < 10; x++) {
        if (second) {
          for (let y = 0; y < 10; y++) {
            if (third) {
              console.log('third');
            } else {
              console.log('not third');
            }
          }
        } else {
          console.log('not second');
        }
      }
    }
  }

  make_spaghetti('foo','bar','baz');
}
```

The above code is very hard to read.

# Do this instead

Let's refactor the above code step by step to be cleaner. The first line is an `if` statement that wraps the entire method.
We can eliminate this block by flipping the sign and returning:

```js
{
  let make_spaghetti = (first, second, third) => {
    if (!first) return;
    for (let x = 0; x < 10; x++) {
      if (second) {
        for (let y = 0; y < 10; y++) {
          if (third) {
            console.log('third');
          } else {
            console.log('not third');
          }
        }
      } else {
        console.log('not second');
      }
    }
  }

  make_spaghetti('foo','bar','baz');
}
```

That's a little better already. Next, start with the inner most loop and break it out into it's own method:

```js
{
  let inner_loop = (third) => {
    if (third) {
      console.log('third');
    } else {
      console.log('not third');
    }
  }

  let make_spaghetti = (first, second, third) => {
    if (!first) return;
    for (let x = 0; x < 10; x++) {
      if (second) {
        for (let y = 0; y < 10; y++) {
          inner_loop(third);
        }
      } else {
        console.log('not second');
      }
    }
  }

  make_spaghetti('foo','bar','baz');
}
```
