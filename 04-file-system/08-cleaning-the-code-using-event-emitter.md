# Cleaning the Code using Event Emitter.

The Node JS documentation suggests that the FileHandle class inherits from the EventEmitter class. Therefore, we can use the EventEmitter object to clean up our code the following way.

```javascript
const fs = require("fs/promises");

(async () => {
  const commandFileHandler = await fs.open("./command.txt", "r");

  commandFileHandler.on("change", async () => {
    const size = (await commandFileHandler.stat()).size;
    const buffer = Buffer.alloc(size);

    const offset = 0;
    const length = buffer.byteLength;
    const position = 0;

    const content = await commandFileHandler.read(buffer, offset, length, position);
    console.log(content);
  });

  const watcher = fs.watch("./command.txt");
  for await (const event of watcher) {
    if (event.eventType === "change") {
      commandFileHandler.emit("change");
    }
  }

  commandFileHandler.close();
})();
```

Also, since we create the buffer ourself, storing the response of the `fs.read` method is redundant. Instead, we can log the buffer upon successfully reading the file into the buffer.

```javascript
const fs = require("fs/promises");

(async () => {
  const commandFileHandler = await fs.open("./command.txt", "r");

  commandFileHandler.on("change", async () => {
    const size = (await commandFileHandler.stat()).size;
    const buffer = Buffer.alloc(size);

    const offset = 0;
    const length = buffer.byteLength;
    const position = 0;

    // updated the following two lines
    await commandFileHandler.read(buffer, offset, length, position);
    console.log(buffer);
  });

  const watcher = fs.watch("./command.txt");

  for await (const event of watcher) {
    if (event.eventType === "change") {
      commandFileHandler.emit("change");
    }
  }

  commandFileHandler.close();
})();
```
