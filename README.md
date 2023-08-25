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

Add the minio content lib to the vsphere namespace (my canse namespace1000)

![GitHub](add1.png)

![GitHub](add2.png)

![GitHub](add3.png)


Check on the image

```
k config use-context namespace1000

Switched to context "namespace1000".
[root@localhost minio]# k get nodes
NAME                               STATUS   ROLES                  AGE   VERSION
4223110e3b11a02b6b177db0551f41ec   Ready    control-plane,master   77d   v1.24.9+vmware.wcp.1
42234f3fd4ee6ad71dde6b4da15622e5   Ready    control-plane,master   77d   v1.24.9+vmware.wcp.1
422398077f99efb5bf4475df395f65f3   Ready    control-plane,master   77d   v1.24.9+vmware.wcp.1
[root@localhost minio]#

kubectl get virtualmachineclassbindings

NAME                  AGE
best-effort-2xlarge   76d
best-effort-4xlarge   76d
best-effort-8xlarge   76d
best-effort-large     76d
best-effort-medium    76d
best-effort-small     76d
best-effort-xlarge    76d
best-effort-xsmall    76d
guaranteed-2xlarge    76d
guaranteed-4xlarge    76d
guaranteed-8xlarge    76d
guaranteed-large      76d
guaranteed-medium     76d
guaranteed-small      76d
guaranteed-xlarge     76d
guaranteed-xsmall     76d

kubectl get virtualmachineimages

NAME                                                                        PROVIDER-NAME                          CONTENT-LIBRARY-NAME   IMAGE-NAME                                                                  VERSION                                 OS-TYPE               FORMAT   AGE
centos-stream-8-vmservice-v1alpha1-1638306496810                            49cd0e81-beee-487c-925d-ded803265fa3                          centos-stream-8-vmservice-v1alpha1-1638306496810                                                                    centos8_64Guest       ovf      41m
ob-15957779-photon-3-k8s-v1.16.8---vmware.1-tkg.3.60d2ffd                   4d3b8db5-bf50-453f-b7d9-adc11e03fc79                          ob-15957779-photon-3-k8s-v1.16.8---vmware.1-tkg.3.60d2ffd                   v1.16.8+vmware.1-tkg.3.60d2ffd          vmwarePhoton64Guest   ovf      77d
.
.
.
ob-22187091-ubuntu-2004-amd64-vmi-k8s-v1.26.5---vmware.2-fips.1-tkg.1       4d3b8db5-bf50-453f-b7d9-adc11e03fc79                          ob-22187091-ubuntu-2004-amd64-vmi-k8s-v1.26.5---vmware.2-fips.1-tkg.1       v1.26.5+vmware.2-fips.1                 ubuntu64Guest         ovf      37h


kubectl get resourcequotas

NAME                         AGE   REQUEST                                                                                               LIMIT
namespace1000-storagequota   76d   pacific-gold-storage-policy.storageclass.storage.k8s.io/requests.storage: 203Gi/9223372036854775807   

k get sc

NAME                          PROVISIONER              RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
pacific-gold-storage-policy   csi.vsphere.vmware.com   Delete          Immediate           true                   76d

kubectl get network

NAME    AGE
work1   76d

```

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

