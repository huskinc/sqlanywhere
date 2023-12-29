# SQLAnywhere File Format Reverse Engineering

This will be a timeline of my process of:<br/>
  - Reverse engineering the file format of SQL Anywhere (I will start with version 17, but might eventually add support for other versions. <br/>
  - Parsing the file format. 


### *2023-12-28*
Currently figuring out the basics.<br/>
Methodology will be combining *(i)* creating different dbs with different settings and seeing how the binary is affected and *(ii)* using the database dlls that load/parse the db to help. 
- The page size is at byte 0x19 - uint16 representing the log2 of the page size in bytes (i.e. 0x000c = 12 --> 4096 bytes or 2^12 bytes).
- There seems to be (what seems to me) header magic: "DA7ABA5E00454D3D" at hex 0x22C (hexified DATABASE 00434D3D) and then in reverse "3D4D45005EBA7ADA" at hex 0x2B4.
