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

Import image into content lib

![GitHub](import1.png)

![GitHub](import2.png)

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

Extract the pub key

```
cat /root/.ssh/id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCjuBKpQqHYYtpRCyPx9eJ4VgvFKwq4ysaUW0hEfkH9qhEltCcR7db8n5ikHQkQesTYftV1FkbhnoZXA2SDTLzUF/M24ZyzBVOPP+goJa5UBiw4/6MPwVcK+epmCyUDRDqW0dw4EM13xmb+I1Rq0RMh86MaD3I99iY848F60QjLEVokonvbI3dR9ecpuydXzP9Pt4Scn8YTHJ5ywnZ0jCbMh9HJxJXyLDUDWG5dPYV9I+7v+GB90VaOcLMqmILWrUk+v2pmpFDfOCLfnKR9HHHUu2S+gw2wFiePo4WgoHMDKm7sRllheLhWRVAdPhZE8hsRm7sZK072kV0sEXsKdKB20wHDgCmYuOxiY2dP4GYrP/Um0jN3IbIJDRP1VN2ruZc7vY6TRPTfEBdineVRQ8WQIOLb3RwK85xFM2Au5k33inOA6wOWWm9RNGJwo5QfJLgHTXhRO+uPegfGFyAYWskrH9I93DgPSypP9/RBV3CesZuO1rsLRgzwBoOypG+ZSsRVLDxJV9bg1VYl61nfzxUTuQQVkerhAmepwz7sovHqpVQ5thIYrbKkMV/gJBDouVa6qG/jtItBCxAa6Tlok2lE5MViPxdIICdLTuzmwhvjlZtnP7Q+u/KtuC6w45aWBT8iuW+wakzz6wNfwSNUG3a9vrVWTBwQRR8ljQuKVVQnxw== root@localhost.localdomain
```

