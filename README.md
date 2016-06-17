## SSH : 

#### Debugging SSH:  
  
`ssh -v <username>@<host>`: the more '-v's the more debugging information is displayed;
this debugs when connecting  
`/usr/bin/sshd -d -p 2222` : sends debug output to stderr; this is for debugging on the server `-p` is the port 
number on which sshd was started on (where it listens)  
`sshd` : listens for connections from clients; it forks new daemon on each incoming connection.   
`-p` specifies the port on which sshd listens (default is port 22).  
`echo $?` : shows exit status of last command issued   
`tail -f /var/log/auth.log`:  tail outputs the last 10 lines of the file; `-f` follows, it appends
output as the file grows. run this on the remotehost  
`ps aux | grep 'ssh-agent'`: check that key agent is running; `aux` shows all processes in a 
user readable mode  
`ssh-add -l`: check that ssh-agent has your key or do `ls ~/.ssh/`  
  
**permissions**: "bad ownership or modes for ..."  
if the server is running in _`StrictModes on`_ then it will refuse to use
your public keys in the `~/.ssh/authorized_keys` file. home directory only writable by you and   authorized_keys
should be 600:  `chmod 700 ~/.ssh/authorized_keys; chmod 600 ~/.ssh/authorized_keys`   


#### `ps` vs `top` :   
`top` : used interactively; displays running processes and system statistics continuously  
(`top` == table of processes that updates periodically)  
`ps`  : used from scripts, displays a snapshot of running processes (`ps` == process status)  

#### chmod review :  
3 sets of permissions: _owner_, _group_, _other_   
700 means that the owner has rwx permissions (4+2+1)   
600 means that the owner has rw permissions (4+2)   
