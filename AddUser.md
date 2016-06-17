### Adding and Removing Users:  
`adduser <new_user>`: adds user  
`groups <new_user>`: shows group memberships  
 adding a user to the sudo group will extend full privileges to that user  
 a newuser is only a member of their own group initially  

`usermod -aG root <newuser>`: to add a user to a group; first **see available groups**  
 `usermod` modify a user account  
 `-a` append the user to the group  
 `-G` remove from groups not listed  

ALTERNATIVELY: use the `visudo` command to specify the new user's privileges.  this will open  
a configuration file `/etc/sudoers`   

`visudo` is the only recommended way to make changes to the `/etc/sudoers` file because it  
prevents multiple user from writing to it simultaneously, and before writing it checks the   
validity of the changes  

search for `newuser` in the file,  and modify to match :  
  
      newuser ALL=(ALL:ALL) ALL

(Add a line after the root user line). First give the user. The first 'ALL' indicates  
for **all hosts**. The second can issue root commands as **all users**. The third ... 
as **all groups**. The fourth means ... that these privileges apply to **all commands**.  

Exit like you would exit `vi`.  

Finally, to delete a user: 
`userdel newuser`: - will simply remove `newuser` from `/etc/passwd`  
`-r` : use this to delete the user's home directory 

**remove from visudo**

#### Utilities:  
to see all users :  
`cut -d: f1 /etc/passwd`
`cut`: cuts out selected portions of each line of a file  
`-d` - specifies the delimiter  
`f1` - specifies fields to print out  

to see all groups: 
`cut -d: -f1 /etc/group`  
