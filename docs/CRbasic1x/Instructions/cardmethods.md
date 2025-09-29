# Methods for Writing Data to a Card

## Is one method of writing data to a card better than the other?

An advantage of the [CardOut](cardout.md)() instruction is that it is very simple to add to your program.

In many applications, however, the [TableFile](tablefile.md)() instruction with Option 64 has advantages over the CardOut() instruction. These advantages include:

- Allowing multiple small files to be written from the same data table so that storage for a single table can exceed 2 GB. The TableFile() instruction controls the size of its output files through the NumRecs, TimeIntoInterval, and Interval parameters.

- Faster compile times when small file sizes are specified

- Easy retrieval of closed files via File Control utility, FTP, or email

- Closed files are safe files. Anytime a file is open for writing, it can become corrupted if a power loss occurs or if the writing is interrupted for any reason. TableFile() Option 64 will close files at the programmed time or record interval. Once those files are closed, they are safe from this type of corruption.

To learn more about the TableFile() instruction with Option 64, read the "[A Better Way to Write High-Frequency Data to 16 GB and Smaller CF Cards](https://s.campbellsci.com/documents/us/technical-papers/write-high-frequency-data-to-cf-cards.pdf)" technical paper.

You may also find it helpful to review the "How to Store Datalogger Data to a Memory Card" blog article. Copy this address to your browser search window: https://www.campbellsci.com/blog/store-datalogger-data-to-memory-card
