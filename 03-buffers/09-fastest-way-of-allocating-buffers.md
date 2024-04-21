# Fastest Way of Allocating Buffers.

Till now, we have used the static method `alloc` to create a buffer within our memory. This method creates a buffer of the specified size and then fills out each element with a zero.

```javascript
const { Buffer } = require("buffer");

const buff = Buffer.alloc(4);

console.log(buff);
```

We can pass in a second argument to the `alloc` method if we want to fill out the buffer with some other pre-defined value instead.

```javascript
const { Buffer } = require("buffer");

const buff = Buffer.alloc(4, 0x22);

console.log(buff);
```

This process of filling out the buffer with a specific value has a time cost associated to it. Therefore, there is a quicker way to create a buffer where we do not fill out its elements with a specific value. This static method is called `allocUnsafe`.

```javascript
const { Buffer } = require("buffer");

const buff = Buffer.allocUnsafe(4);

console.log(buff);
```

When we create a buffer using the `allocUnsafe` method, it does not overwrite the existing element stored at a memory location. Therefore, we can never certainly comment on the value of each element within the buffer; it can be anything between 0 to 255 for each element upon buffer creation.

We can create an situation where we come across an existing element within a new buffer by increasing the size of the buffer. This would increase the probability of the memory location having already been used earlier.

```javascript
const { Buffer } = require("buffer");

const unsafeBuffer = Buffer.allocUnsafe(8000);

for (let i = 0; i < unsafeBuffer.length; i++) {
  if (unsafeBuffer[i] !== 0) {
    console.log(`Element at ${i} has value: ${unsafeBuffer[i].toString(2)}`);
  }
}
```

The only situation in which we can use an unsafe buffer is when we know we would write to each element within the buffer quickly after its creation, replacing any pre-existing data. We talk more about security later in the course, but know that unsafe buffers can introduce a good amount of security vulnerabilities.

Now, the buffer created with the static methods `from` and `concat` use `allocUnsafe` behind the scenes. However, each element within the buffer is filled as soon as possible. Therefore, using these two methods to create a buffer is still safe.

We discussed one reason due to which the `allocUnsafe` method is faster, which is no pre-filling/overwriting the existing values. There is also another reason for this behavior, but only for specific sizes.

When we run a "node" process, it reserves a small piece for memory for itself. The purpose of this space is to use it for future buffers. Therefore, when we create a new unsafe buffer, Node JS tries to use the reserved space, and only reserves a new piece of memory if the buffer size is greater than the reserved space. On the contrary, safe buffers always reserve a new piece of memory, which is obviously slower than utilizing an already reserved piece.

<p align="center">
    <img src="../images/S03-SS12.png" width="800" />
</p>

We can confirm the size of this reserved piece of memory by executing the following code, which prints out 8 KiB.

```javascript
const { Buffer } = require("buffer");

console.log(Buffer.poolSize);
```

Per the official Node JS documentation, when using `allocUnsafe` to allocate new buffer instances, allocations under 4 KiB are sliced from a single pre-allocated buffer. This allows applications to avoid the garbage collection overhead of creating many individually allocated buffer instances.

If we were to create a formula out of the details above, we take the pool size, divide it by two, and then floor the result. This operation is represented by the notation we add to the console statement below.

```javascript
const { Buffer } = require("buffer");

console.log(Buffer.poolSize >>> 1);
```

The `>>>` operator is the unsigned right shift operator. It takes a binary number and shifts it to the right by the specified number - one in our case. Excess bits shifted off to the right are discarded, and zero bits are shifted in from the left. The following code provides a simple example.

```javascript
const a = 5; //  00000000000000000000000000000101
const b = 2; //  00000000000000000000000000000010

// Discarded two right-most bits of "a" and added zeros to the left to maintain size.
console.log(a >>> b); //  00000000000000000000000000000001
```

These "shift" operators are also how computers perform division operation internally.

```
60 / 2
= 0011 1100 >>> 1
= 0001 1110
= 30

15 / 2
= 0000 1111 >>> 1
= 0000 0111
= 7
```

There is also another way of creating an unsafe buffer, which is the `allocUnsafeSlow` method. The difference between the two unsafe methods is that the `allocUnsafe` method can use the pre-allocated piece of memory, but the `allocUnsafeSlow` method will always reserve a new piece of memory.
