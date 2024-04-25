# Benchmarking.

Before we can understand the value proposition of streams, we need to build up the background to understand where we must use streams. For this, we depend on what we learned in the last two sections of this course.

To start things off, we create a program to write something to a file one million times and track its performance.

```javascript
const fs = require("node:fs/promises");

(async () => {
  console.time("WriteMany");
  const fileHandle = await fs.open("test.txt", "w");

  for (let i = 0; i < 1000000; i++) {
    await fileHandle.write(` ${i} `);
  }

  console.timeEnd("WriteMany");
})();
```

If we execute the code above, we get something similar to the following logged to the console.

<p align="center">
    <img src="../images/S05-SS01.png" width="800" />
</p>

Note that the time it takes for the program to execute is marginally different across executions and machines. This comes down to CPU, processes and a few other factors. That said, regardless of the marginal difference, this gives us a good benchmark to track the performance. Also, if we run the code more than once, the file gets completely overwritten because we are writing to the file, not appending things to the end.

Next, we can also track the metrics of the CPU and memory consumption inside the activity monitor. First, we check out the CPU consumption of the "node" process.

<p align="center">
    <img src="../images/S05-SS02.png" width="800" />
</p>

Per the diagram, the "node" process is using 104.7% CPU. This means that the "node" process is consuming full power of one core of our CPU; if the number was beyond 200%, it would suggest that two cores were being used to their fullest. Also, note that 81.44% of the total CPU is idle despite the consumption by the "node" process.

When deploying an application on a virtual machine, we must leave a good amount of idle CPU - we discuss this in detail later. Also, we use one core by default because Node JS is a single-threaded runtime, but if we want to use more cores, we can rely on clustering or the worker thread module - again, we discuss this in detail later.

Moving on, we can also check the memory consumption of the "node" process, which hovers around the 50MB mark over the course of program execution.

<p align="center">
    <img src="../images/S05-SS03.png" width="800" />
</p>

We can include some comments in the code to track these metrics.

```javascript
const fs = require("node:fs/promises");

// Execution Time: 8s.
// CPU Usage: 100% (One Core).
// Memory Usage: 50MB.
(async () => {
  console.time("WriteMany");

  const fileHandle = await fs.open("test.txt", "w");

  for (let i = 0; i < 1000000; i++) {
    await fileHandle.write(` ${i} `);
  }

  console.timeEnd("WriteMany");
})();
```

Now, if we update the code to use the Callback API instead of the Promises API, our code becomes a little more performant.

```javascript
const fs = require("node:fs");

// Execution Time: 2s.
// CPU Usage: 100% (One Core).
// Memory Usage: 50MB.
console.time("WriteMany");

fs.open("test.txt", "w", async (err, fileDescriptor) => {
  for (let i = 0; i < 1000000; i++) {
    fs.writeSync(fileDescriptor, ` ${i} `);
  }

  console.timeEnd("WriteMany");
});
```

If we push too many events to the event loop for asynchronous execution of the `fs.write` method, we get inconsistent results inside the file because the callback function added later can finish executing earlier. This also increases the memory usage significantly because a million callback functions are being executed in the background. Therefore, it is good to try and avoid this approach and resort to `fs.writeSync` when using the Callback API.

```javascript
const fs = require("node:fs");

// Execution Time: 2s.
console.time("WriteMany");

fs.open("test.txt", "w", async (err, fileDescriptor) => {
  for (let i = 0; i < 1000000; i++) {
    fs.write(fileDescriptor, ` ${i} `, () => {});
  }

  console.timeEnd("WriteMany");
});
```

If we increase the number of callback functions to ten million instead, the memory usage climbs to 8GB with time, forcing the application to run out of heap and crash.

Note that the string we pass into the write method is converted to a buffer behind the scenes by Node. We can also explicitly pass in the buffer, but it slows down the speed slightly because we need to initialize a new variable before writing data to the file.

```javascript
const fs = require("node:fs");

console.time("WriteMany");

fs.open("test.txt", "w", async (err, fileDescriptor) => {
  for (let i = 0; i < 1000000; i++) {
    const buff = Buffer.from(` ${i} `, "utf-8");
    fs.writeSync(fileDescriptor, buff);
  }

  console.timeEnd("WriteMany");
});
```
