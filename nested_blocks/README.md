If you see more than a level or two of indent, this is a sign that it may be time to refactor your code. Let's
look at an example, blow.

Tip: Paste these examples into your javascript console in your browser.

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

The above code is hard to read.
- It becomes more unreadable as the inner loop becomes longer.
- It becomes more unreadable as the number of variables increase.
- It becomes more unreadable as the number of indent levels increase.

# Do this instead

Let's refactor the above code, step by step, to be cleaner.

## Step 1 - invert if and return
The first line is an `if` statement that wraps the entire method.
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

That's a little better already.

## Step 2: break inner loop out into another method

Next, start with the inner most loop and break it out into another method:

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

Good. This code is starting to flatten out. Flat code is readable code.

Notice how it is now very clear that the `inner_loop()` method takes exactly one argument. This is extremely important. One of the biggest problems with heavily nested code is that it is very difficult to figure out how many dependencies the inner loop has.

**Do** use an `options` object for your inner loop method if you find your inner loop has more than three arguments.

## Step 3: break outer loop out into another method

```js
{
  let inner_loop = (third) => {
    if (third) {
      console.log('third');
    } else {
      console.log('not third');
    }
  }

  let outer_loop = (second, third) => {
    if (second) {
      for (let y = 0; y < 10; y++) {
        inner_loop(third);
      }
    } else {
      console.log('not second');
    }
  }
  
  let make_spaghetti = (first, second, third) => {
    if (!first) return;
    for (let x = 0; x < 10; x++) {
      outer_loop(second, third);
    }
  }

  make_spaghetti('foo','bar','baz');
}
```

Great! This is much more readable. It is now clear that the outer loop takes two arguments. The code is much flatter and more readable.

## Step 4: Invert if and return in outer loop

Now that our outer loop is a method, we can make use of `return` to make it even more succinct and readable.

```js
{
  let inner_loop = (third) => {
    if (third) {
      console.log('third');
    } else {
      console.log('not third');
    }
  }

  let outer_loop = (second, third) => {
    if (!second) return console.log('not second');
    for (let y = 0; y < 10; y++) {
      inner_loop(third);
    }
  }
  
  let make_spaghetti = (first, second, third) => {
    if (!first) return;
    for (let x = 0; x < 10; x++) {
      outer_loop(second, third);
    }
  }

  make_spaghetti('foo','bar','baz');
}
```

This is great! The code is now very readable, flat, and clean.
