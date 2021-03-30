### generate ssh key
root@joepoc-k8s-master02:~# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:V9plgDg

### rename pub key
root@joepoc-k8s-master02:~# cd ~
root@joepoc-k8s-master02:~# mv .ssh/id_rsa.pub .ssh/authorized_keys3

### edit ssh config
root@joepoc-k8s-master02:~/.ssh# vim /etc/ssh/sshd_config

PermitRootLogin yes 
AuthorizedKeysFile .ssh/authorized_keys3 
PubkeyAuthentication yes
PasswordAuthentication no

### restart sshd
root@joepoc-k8s-master02:~# systemctl restart sshd

### copy pri key /root/.ssh/id_rsa to client
root@joepoc-k8s-master02:~# cat /root/.ssh/id_rsa

### delete /root/.ssh/id_rsa
root@joepoc-k8s-master02:~# rm /root/.ssh/id_rsa

### jump to client, create .pem
root@joepoc-k8s-master01:~# vim key02.pem

### restrict access of .pem
root@joepoc-k8s-master01:~# chmod 400 key02.pem

### ssh to root
root@joepoc-k8s-master01:~# ssh -i key02.pem root@joepoc-k8s-master02

---
## do the same to master 01 & 03 and worker 01 02
### for 01, it plays both client and server

### copy private key to other server
root@joepoc-k8s-master02:~# cat /root/.ssh/authorized_keys3

### copy to other server

root@eai-k8s-worker02:~# vim /root/.ssh/authorized_keys3

root@eai-k8s-worker02:~# vim /etc/ssh/sshd_config
PermitRootLogin yes 
AuthorizedKeysFile .ssh/authorized_keys3 
PubkeyAuthentication yes

root@eai-k8s-worker02:~# systemctl restart sshd



