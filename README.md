# Deprecated. This module is now [part of the official asm.js repository](https://github.com/dherman/asm.js/pull/103).


[ ![Codeship Status for alexanderGugel/validate-asm](https://codeship.com/projects/93639d10-9cf4-0132-95e9-028d633c913e/status?branch=master)](https://codeship.com/projects/64402)

# validate-asm

Like JSLint, but for asm.js

## example

Let's say you have an asm module called `GeometricMean.js`:

```javascript
function GeometricMean(stdlib, foreign, buffer) {
  "use asm";

  var exp = stdlib.Math.exp;
  var log = stdlib.Math.log;
  var values = new stdlib.Float64Array(buffer);

  function logSum(start, end) {
    start = start|0;
    end = end|0;

    var sum = 0.0, p = 0, q = 0;

    // asm.js forces byte addressing of the heap by requiring shifting by 3
    for (p = start << 3, q = end << 3; (p|0) < (q|0); p = (p + 8)|0) {
      sum = sum + +log(values[p>>3]);
    }

    return +sum;
  }

  function geometricMean(start, end) {
    start = start|0;
    end = end|0;

    return +exp(+logSum(start, end) / +((end - start)|0));
  }

  return { geometricMean: geometricMean };
}

module.exports = GeometricMean;
```

In order to validate it, simply run

`validate-asm GeometricMean.js`

That's it!

## install

With [npm](http://npmjs.org) do:

```
npm install -g validate-asm
```

## usage

```
Usage: validate-asm [entry files] {OPTIONS}

Standard Options:

    --help, -h  Show this message

Specify a parameter.
```

## credits

* based on [asm.js](https://github.com/dherman/asm.js/tree/master/lib)

## license

ISC
