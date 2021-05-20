# borg-backup-tutorial

## Remote server
1. install borg


**synology**   
https://roll.urown.net/NAS/borg-backup-server.html

### SSH config, `~/.ssh/authorized_keys`
use public key from client `~/.ssh/id_ed25519_borg.pub`
````bash
command="/usr/local/bin/borg serve --restrict-to-path /volumeXXX/borg-backup/$HOSTNAME/" ssh-ed25519 $SSH-PUB-key $SSH-PUB-commnet
````

## Client

### generate passphrase 

````bash
head -c 32 /dev/urandom | base64 -w 0
````

### ssh config
````bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519_borg -C "some useful comment"
````
````bash
Host eideen_borg
   IdentityFile ~/.ssh/id_ed25519_borg
   Port $REMOTE_PORT
   User borg-backup
   hostname $REMOTE_Host
````

### setup

````bash
export BORG_REPO='ssh://user@host:port/path/to/repo'
export BORG_PASSPHRASE='Some key'
borg init -e repokey-blake2
````
### backup
Backup key, passphase, and othere info to a different location like a key mangere.
````bash
borg key export $BORG_REPO key.txt
````
