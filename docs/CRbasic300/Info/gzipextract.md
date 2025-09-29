# GZIP Extract

GRANITE-series, CR6, CR3000, CR1000X, CR800-series, CR1000 dataloggers support the ability to extract the contents of program and operating system files that have been created using GZIP. The file name must be in the format: filename.fileextension.gz (for example, MyFile.CR1.gz or TestPgm.CR6.gz or CR1000.Std.29.obj.gz).

Zipping a file has the potential of significantly reducing its size. The amount of reduction realized depends mainly on the number and proximity of redundant blocks of information in the file. A reduction in file size means fewer bytes have to be transferred when sending a file to a datalogger. This reduction can have a significant impact on transfer times over slow or high latency links or cost when utilizing "pay by the byte" data plans. Those using low baud rate terrestrial radio, satellite, or restricted cellular data plans should consider gzip ing programs and operating systems before sending.

Compatible files can be created using any utility that supports the GZIP file format. Several free utilities are available that provide zipping to this format. Once the zipped file is created, simply send the file using the software's Send Program functionality. Note that the datalogger will not automatically unzip files that are sent using File Control. However, compressed files sent via File Control can be manually decompressed by marking the files as Run Now.

Compression has little effect on an encrypted program (FileEncrypt in the CRBasic Editor), since the encryption process does not produce a large number of repeatable byte patterns. Additionally, GZIP has little effect on files that already employ compression such as JPEG or MPEG-4.
