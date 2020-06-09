# proposal-array-equality
> Determining whether two arrays are equal.

__Author:__ [Hemanth HM](https://github.com/hemanth)

__Champions:__ [Jordan Harband](https://github.com/ljharb) & [Hemanth HM](https://github.com/hemanth)

# Motivation:

Today we don't have straightforward way to check if two arrays are equal, it get more messier to check when they are deeply nested arrays.

# How is this currently handled?

There is no standard way to do it, there are few modules like [array-equal](https://www.npmjs.com/package/array-equal), [deep-equal](https://www.npmjs.com/package/deep-equal) with like 5M and 8M downloads per week respectively, which is subset of deep comparison.

Consider few examples:

```js
// Schema validation
const equal = require('deep-equal');

const expectedSchema = {
  name: {
    type: String,
    required: true
  },
  score: {
    type: Number,
    default: 0
  }
};

equal(schemaCall.args[0], expectedSchema);

```

__node builtin:__

```js
// assert.deepStrictEqual(actual, expected[, message])

deepStrictEqual([new Uint32Array([1, 2, 3, 4]).subarray(1, 3), new Uint32Array([2, 3])]);
```

```js
// assert.deepEqual(actual, expected[, message])

assert.deepEqual( tokenizer( "AD", "G", cldr ), [{
		type: "G",
		lexeme: "AD",
		value: "1"
	}] );
```

# Proposal:

Have a `Array.prototype.equals` method, they might look like:

```js
[1,2,3].equals([1,2,3]) // evaluates to true

[1,2,undefined].equals([1,2,3]) // evaluates to false.
```

```js
[1, [2, [3,4]]].equals([1, [2, [3,4]]]) // true
```

```js
[{
  foo: 'bar'
}, {
  foo: 'baz'
}].equals[{
    foo: 'bar'
}] // false

```

__How are other langugaes handling this?__

__Ruby:__ `array1.to_set == array2.to_set`

__Python:__ `[1,[2,3]] == [1,[2,3]]`

__Java:__ `java.util.Arrays.equal`

__C#:__ `Enumerable.Except` or `SequenceEqual`
