# FILE AND DIRECTORY COMMANDS
List all files in a long listing (detailed) format  
`ls -al`

Display the present working directory  
`pwd`

Create a directory  
`mkdir <DIRECTORY>`

Remove (delete) file  
`rm <FILE>`

Remove the directory and its contents recursively  
`rm -r <DIRECTORY>`

Force removal of file without prompting for confirmation  
`rm -f <FILE>`

Forcefully remove directory recursively  
`rm -rf <DIRECTORY>`

Copy file1 to file2  
`cp <FILE1> <FILE2>`

Copy source_directory recursively to destination. If destination exists, copy source_directory into destination, otherwise create destination with the contents of source_directory.  
`cp -r <SOURCE> <DESTINATION>`

Rename or move file1 to file2. If file2 is an existing directory, move file1 into directory file2  
`mv <FILE1> <FILE2>`

Create symbolic link to linkname  
`ln -s /path/to/file linkname`

Create an empty file or update the access and modification times of file.  
`touch <FILE>`

View the contents of file  
`cat <FILE>`

Browse through a text file  
`less <FILE>`

Display the first 10 lines of file  
`head <FILE>`

Display the last 10 lines of file  
`tail <FILE>`

Display the last 10 lines of file and "follow" the file as it grows.  
`tail -f <FILE>`

---

[Back](../basic-command.md)