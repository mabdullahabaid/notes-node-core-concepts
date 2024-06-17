# Building Streaming Solution Using Buffers.

Suppose we want to create a copy of a file without the use of streams. The following code allows us to execute the required operation.

```javascript
const fs = require("node:fs/promises");

// File Size Copied: 1GB.
// Memory Usage: 1GB.
// Execution Time: 900ms.
(async () => {
  console.time("copy");

  const destFile = await fs.open("dest.txt", "w");
  const srcFile = await fs.readFile("src.txt");

  await destFile.write(srcFile);

  console.timeEnd("copy");
})();
```

The `readFile` method buffers the complete file into the memory, therefore this logic is extremely problematic. Also, the method throws an error if we try to read a file greater than 2GBs.

Now, we can try to improve this logic without the use of built-in Node JS streams. For this, we resort to building our own streams.

```javascript
const fs = require("node:fs/promises");

// File Size Copied: 1GB.
// Memory Usage: 30MB.
// Execution Time: 2s.
(async () => {
  console.time("copy");
  const srcFile = await fs.open("src.txt", "r");
  const destFile = await fs.open("dest.txt", "w");

  let bytesRead = -1;

  while (bytesRead !== 0) {
    // reads one chunk of a file - 16,384 bytes.
    const readResult = await srcFile.read();

    // until we hit the end of the file, this should be 16,384 bytes.
    bytesRead = readResult.bytesRead;

    // buffer is 16384 bytes, but since file can end before 16384, we get zeros at the end which we need to filter.
    if (bytesRead !== 16384) {
      const indexOfNotFilled = readResult.buffer.indexOf(0);
      const newBuffer = Buffer.alloc(indexOfNotFilled);
      readResult.buffer.copy(newBuffer, 0, 0, indexOfNotFilled);
      destFile.write(newBuffer);
    } else {
      destFile.write(readResult.buffer);
    }
  }

  console.timeEnd("copy");
})();
```
