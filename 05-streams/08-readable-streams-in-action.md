# Readable Streams in Action.

Readable streams are used in operations where data must be read in small chunks. Suppose we want to read from the `test.txt` file we created earlier in this section. We can use the following code snippet to achieve the desired outcome.

```javascript
const fs = require("node:fs/promises");

(async () => {
  const fileHandle = await fs.open("test.txt", "r");

  const stream = fileHandle.createReadStream();

  stream.on("data", (chunk) => {
    console.log(chunk);
  });
})();
```

Unlike the 16 KiB default `highWaterMark` value for a <stream.Readable> object, the stream returned by `createReadStream` method has a `highWaterMark` value of 64 KiB.

Suppose that we want to read data from a file and write it to a new file. The following code allows us to execute this behavior.

```javascript
const fs = require("node:fs/promises");

(async () => {
  const fileHandleRead = await fs.open("src.txt", "r");
  const fileHandleWrite = await fs.open("dest.txt", "w");

  const streamRead = fileHandleRead.createReadStream();
  const streamWrite = fileHandleWrite.createWriteStream();

  streamRead.on("data", (chunk) => {
    streamWrite.write(chunk);
  });
})();
```

Typically, hard-drives have a higher read speed in comparison to the write speed. This introduces some back pressuring and increases the memory usage - Node JS buffers data to the memory to write it later. The `src.txt` file contains one million numbers at the moment, so the back pressuring is not v significant. However, if we increase the numbers to one billion instead, the back pressuring fills up some 8GBs of memory.

High memory usage is not ideal, ofc. Therefore, we must update the code to deal with this issue.

```javascript
const fs = require("node:fs/promises");

(async () => {
  const fileHandleRead = await fs.open("src.txt", "r");
  const fileHandleWrite = await fs.open("dest.txt", "w");

  const streamRead = fileHandleRead.createReadStream();
  const streamWrite = fileHandleWrite.createWriteStream();

  streamRead.on("data", (chunk) => {
    // if the write buffer is full, give it time to drain
    if (!streamWrite.write(chunk)) {
      streamRead.pause();
    }
  });

  streamWrite.on("drain", () => {
    streamRead.resume();
  });
})();
```

With the updated code, reading and writing one billion numbers takes some 30MBs of memory throughout the operation, which is much less than 8GBs, even though the file itself is of 10.9 GB in size. This becomes significant when we want to read 100GBs of data from a database or send such huge amounts of data to another process on the OS.
