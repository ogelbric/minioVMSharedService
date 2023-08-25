# MinIO on VM Service



Link to the special VMService VM on Marketplace (download) 

```
https://marketplace.cloud.vmware.com/services/details/vm-service-image-for-centos1111?slug=true
```

Create a new content library for the image

![GitHub](lib1.png)

![GitHub](lib2.png)

![GitHub](lib3.png)

![GitHub](lib4.png)

Generate a key for the VM on your jump server

```
ssh-keygen -t rsa -b 4096

Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:PYyUkw5SWf/3Q0uR3vNV3o/Q1KGTUesH5nRHmZdnhy0 root@localhost.localdomain
The key's randomart image is:
+---[RSA 4096]----+
|      .o.    ..==|
|     ..  +    E*X|
|    . . = .  +=*O|
|     . + = . *++*|
|        S + o +**|
|           . oooB|
|              .o+|
|                .|
|                 |
+----[SHA256]-----+
```

