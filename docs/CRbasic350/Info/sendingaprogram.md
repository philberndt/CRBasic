# Sending a Program to the Datalogger

A good practice is to always retrieve data from the datalogger before sending a program; otherwise, data may be lost. Some methods of sending a program give the option to retain data when possible. Regardless of the send program option selected, data is erased when a new program is sent if any change occurs to one or more data table structure in the following list:

- Data table name(s)

- Data output interval or offset

- Number of fields per record Number of bytes per field

- Field type, size, name, or position

- Number of records in table

## Program Run Options

When sending a program via File Control, Short Cut, or CRBasic, you can choose to have programs **Run on Power-up** and **Run Now**.** Run Now **will run the program when it is sent to the datalogger.** Run on Power-up **runs the program when the datalogger is powered up. Selecting both** Run Now **and** Run on Power-up **will invoke both options.

## Creating a Compressed File

If the** Compress File**box is checked, a second file is created that is saved with an`_str`suffix after the original program name. This`_str`file is sent to the datalogger instead of the original. An`_str`file contains no program comments or spaces (tabs or blank lines). This "stripped" file without remarks and spaces can be significantly smaller in size than one that includes comments and spaces. This is useful for sending very large programs over a slow communication link or when the original program is too large to fit into the datalogger s program memory.

## Select Server (LoggerNet Admin and LoggerNet Remote, only)

The **Select Server** button is used to display the server login window, from which you can select a different LoggerNet server than the one you are logged in. Any LoggerNet server that can be logged in to via TCP/IP can be selected.
