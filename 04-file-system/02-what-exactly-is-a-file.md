# What is a File?

On UNIX systems, everything is a file. A file of data is a datafile but a directory (folder) is also a file. A device is a file (found in the /dev directory). Programs are executable files and even running program and system states have associated files (found in /run). Network connections are files that can be read and written. However, what is a file at its most basic level?

At its most basic level, a file is just a sequence of zeros and ones stored in a container. It is data, and there is nothing more to it than that. An application takes in these zeros and ones, attributes meaning to them, and provides some information in a sensible form.

The same arbitrary sequence of zeros and ones can be interpreted/decoded in many different ways. However, in most cases, we need to know the exact encoding to decode the file in the required format. So how do we know which encoding should be used for which file? We use the the file extension. A file extension denotes how data in the file is structured and how it can be decoded.

Our operating system itself can only run executable files; it has no idea of how to run a PDF file or a PNG file. Therefore, operating system is shipped with some other pre-installed applications that help decode a PDF file or a PNG file.

Note that every bit stored on our hard drive belongs to a file; we cannot write anything on our hard drive unless we write it to a file. This suggests that the whole operating system itself is just different files on our hard drive. Similarly, all the commands that we execute on our terminal are different files. We can execute the following commands to confirm.

```sh
which mkdir
open /bin/
```
