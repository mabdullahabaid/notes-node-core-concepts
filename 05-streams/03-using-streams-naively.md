# Using Streams Naively.

We came out of the last lecture with the following code and metrics.

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

We cannot make the code any faster until we resort to using streams. The following code contains the same solution, but with the introduction of streams.

```javascript
const fs = require("node:fs/promises");

(async () => {
  console.time("WriteMany");

  const fileHandle = await fs.open("test.txt", "w");

  const stream = fileHandle.createWriteStream();

  for (let i = 0; i < 1000000; i++) {
    const buff = Buffer.from(` ${i} `, "utf-8");
    stream.write(buff);
  }

  console.timeEnd("WriteMany");
})();
```

If we execute the code above, the metrics tell a very different story.

```javascript
const fs = require("node:fs/promises");

// Execution Time: 170ms.
// CPU Usage: 100% (One Core).
// Memory Usage: 200MB.
(async () => {
  console.time("WriteMany");

  const fileHandle = await fs.open("test.txt", "w");

  const stream = fileHandle.createWriteStream();

  for (let i = 0; i < 1000000; i++) {
    const buff = Buffer.from(` ${i} `, "utf-8");
    stream.write(buff);
  }

  console.timeEnd("WriteMany");
})();
```

It is important to note that the code we wrote above is not a good practice. One reason behind this is statement is the memory usage - it jumps past 2GB if we increase the number to ten million.
