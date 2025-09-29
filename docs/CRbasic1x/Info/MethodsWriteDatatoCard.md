# Methods for Writing Data to a Card

## Is one method of writing data to a card better than the other?

An advantage of the [CardOut()](../Instructions/cardout.md) instruction is that it is very simple to add to your program.

In many applications, however, the [TableFile()](../Instructions/tablefile.md) instruction with **option 64** has advantages over the`CardOut()`instruction. These advantages include:

- Allowing multiple small files to be written from the same data table so that storage for a single table can exceed 2 GB. The`TableFile()`instruction controls the size of its output files through the`NumRecs`,`TimeIntoInterval`, and`Interval`parameters.

- Faster compile times when small file sizes are specified.

- Easy retrieval of closed files viaFile Control

  **Note:** File Control is a feature of LoggerNet, PC400 and RTDAQ datalogger support software. It provides a view of the datalogger file system and a menu of file management commands.

  utility,FTP

  **Note:** File Transfer Protocol. A TCP/IP application protocol.

  , or email.

- Closed files are safe files. Anytime a file is open for writing, it can become corrupted if a power loss occurs or if the writing is interrupted for any reason.` TableFile()``Option 64 `will close files at the programmed time or record interval. Once those files are closed, they are safe from this type of corruption.

To learn more about the` TableFile()``Option 64 `, consider the following resources:

- Technical paper: [A Better Way to Write High-Frequency Data to 16 GB and Smaller CF Cards](https://s.campbellsci.com/documents/us/technical-papers/write-high-frequency-data-to-cf-cards.pdf).

- Blog article: [How to Store Datalogger Data to a Memory Card](https://www.campbellsci.com/blog/store-datalogger-data-to-memory-card).
