#Double Buffering

## Module purpose

This module is a simple (and still in progress) implementation of double buffering in Rust.
This module is intented to be used in situation where an application has to do read IOs on files stored on different disk,and perform some process on the payload.

A naive implementation would be 

```
foreach_files {
  read files => buffer
}
process buffer

```

Here we sequentially read all files, bufferized it, then process it. With this implementation, the total operation cost would be the sum of all files reading + processing time of buffers.

In addition, the other drawback is if we have a huge number of big files to read, we need a very big bufferization

In short, this is a simple issue where we have to deal with
* memory (with data bufferization)
* IO (with files reading)
* CPU (with buffer processing)

The module try to solve all issues to leverage the fact that files are on different disks

## How it works

The module spawns a thread for each file. Each thread shares a double entry buffer with the master. The master send commands to the threads (we can call it readers).  A command will provide an offset and a len to read in the file. While readers are filling one entry of thei shared buffer, the master is processing all others entries of each shared buffers of all threads.

In short, while thread are busy reading file and filling entry 0, main thread is processing what was previsouly stored on entry 1 of each shared buffers.
When processing is done, new commands can be sent to readers. They will stored now their datas on entry 1, while main thread will be processing entry 0.
