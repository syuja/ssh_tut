## SSH Keys and Debugging :  
  1. [Intro](#intro)  
  2. [Creating the Keys] (#create)  
  3. [Exchanging the Keys](#exch)  
  4. [Debugging](#debug)  
  

**Note**:  
Debugging `SSH` is easier from the server side.  
Start an `sshd` in debug mode on port 2222, `/usr/bin/sshd -p 2222`, then connect with `ssh -p 2222 user@remotehost`.   

Running `ssh -i -vvv user@remotehost`, prints out information that is not very helpful.   

<a id = "intro"></a>
### Intro:  
---  

Setting up SSH keys provides a faster and secure way of logging into server. Once keys are exchanged  
the user will no longer need to enter a password every time.  

To set up the SSH key you will need to be able to sign in to the server using a password.  Otherwise,
you will be unable to exchange the keys; this is a necessary part.  

Setting up a key for one user doesn't set up a key for another user. For example, setting a key  
as root will not also set up a key for the entire machine.   

The general steps are: 
  A. create a pair of keys  (private and public)
  B. copy the public key onto the server  
  C. connect to the server using the private key  


<a id = "create"></a>
### Create the Keys:  
---  

Call `ssh-keygen` this will create a new key. If one already exists, you may choose to use that
one. Simply hit `enter` until it is done.  
The new key pair will have been created in the `~/.ssh/` directory.   
  ![ls-key-pair](https://github.com/syuja/ssh_tut/blob/master/img/ls-key-pair.png)  
  
By default, the created keys are: `id_rsa`(private) and `id_rsa.pub`(public).  
Keep the private key secret; don't share it with anyone. This will allow you to login to the server.  

<a id = "exch"></a>
### Exchanging the Keys:   
--- 

The public key can be shared freely, and it must be uploaded to the server that you wish to connect to.  
Append the public key to the server under the `~/.ssh/authorized_keys` file.   
This can be done in several ways. One ways is to use `ssh-copy-id -i <key_file> <username>@<ipaddress>`, which will copy it onto the  
`~/.ssh/authorized_keys`.  

Another way, is to log in to the server using password authentication. Navigating onto that 
directory and pasting the public key at the end of `~/.ssh/authorized_keys`.   
  
<a id = "debug"></a>
### Debugging SSH:  
---  
**Run `sshd` from server side in debug mode.**   

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
---  

`top` : used interactively; displays running processes and system statistics continuously  
(`top` == table of processes that updates periodically)  
`ps`  : used from scripts, displays a snapshot of running processes (`ps` == process status)  

### chmod review :   
---  

3 sets of permissions: _owner_, _group_, _other_   
700 means that the owner has rwx permissions (4+2+1)   
600 means that the owner has rw permissions (4+2)   


  
  
Resources:   
<sub><sup> https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2   </sub></sup>   
<sub><sup> http://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/   </sub></sup>  
<sub><sup> https://help.ubuntu.com/community/SSH/OpenSSH/Keys   </sub></sup>  
<sub><sup> https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server    </sub></sup>  
