# SQLAnywhere File Format Reverse Engineering

This will be a timeline of my process of:<br/>
  - Reverse engineering the file format of SQL Anywhere (I will start with version 17, but might eventually add support for other versions. <br/>
  - Parsing the file format. 

### Current Summary
- Page Size is at byte 0x1A.
- First byte of each page is the page #.
- Where text is not-compressed, byte before text is the text length.

### *2024-01-04*
- Not sure how much today's findings are useful.
- So far, 8 bytes at 0x10 have always been 0x030000005EBA7ADA --> which is kind of like "DATABASE 3"
- DBInfo checks if 0x4A is 0
- Following SQL String is executed "SELECT 1 FROM SYS.SYSOPTIONS WHERE lower(\"option\") = 'sql_flagger_error_level' and user_name = 'PUBLIC'"


### *2023-12-29*
Data seems to be compressed.<br/>
Where text is not-compressed, byte before text is the text length.

### *2023-12-28*
Currently figuring out the basics.<br/>
Methodology will be combining *(i)* creating different dbs with different settings and seeing how the binary is affected and *(ii)* using the database dlls that load/parse the db to help. 
- The page size is at byte 0x1A - uint8 representing the log2 of the page size in bytes (i.e. 0x0c = 12 --> 4096 bytes or 2^12 bytes).
- There seems to be (what seems to me) header magic: "DA7ABA5E00454D3D" at hex 0x22C (hexified DATABASE 00434D3D) and then in reverse "3D4D45005EBA7ADA" at hex 0x2B4.

