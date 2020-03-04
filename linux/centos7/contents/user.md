# USER INFORMATION AND MANAGEMENT
Display the user and group ids of your current user.  
`id`

Display the last users who have logged onto the system.  
`last`

Show who is logged into the system.  
`who`

Show who is logged in and what they are doing.  
`w`

Create a group named "test".  
`groupadd <GROUPNAME>`

Create an account named john, with a comment of description and create the user's home directory.  
`useradd -c "<DESCRIPTION>" -m <USERNAME>`

Delete the john account.  
`userdel <USERNAME>`

Add the john account to the sales group  
`usermod -aG <GROUPNAME> <USERNAME>`

---

[Back](../contents.md)