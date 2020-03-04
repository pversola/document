# INSTALLING PACKAGES
Search for a package by keyword.  
`yum search <KEYWORD>`

Install package.  
`yum install <PACKAGE>`

Display description and summary information about package.  
`yum info <PACKAGE>`

Install package from local file named package.rpm  
`rpm -i <PACKAGE>.rpm`

Remove/uninstall package.  
`yum remove <PACKAGE>`

Install software from source code.  
```
tar zxvf sourcecode.tar.gz
cd sourcecode
./configure
make
make install
```
---

[Back](../basic-command.md)