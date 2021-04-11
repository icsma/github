**Tutorial Gerenciado o KVM**


**Lista todas as vms**
```
virsh list --all
```
**Verificando informações**
```
virsh dominfo centos8-vm1
```
**Desligando vm**
```
virsh shutdown centos8-vm1
```

**Iniciando as VM**
```
virsh start centos8-vm1
```

**Para que a VM incie quando o servidor for reiniciado**
```
virsh autostart centos8-vm1
```
**Excluir VM**
```
virsh shutdown centos8-vm1
virsh undefine centos8-vm1
virsh pool-destroy centos8-vm1
D=/var/lib/libvirt/images
VM=centos8-vm1.img
rm -ri $D/$VM
```
**Para ver uma lista completa do tipo de comando virsh**
```
virsh help | less
```
**ou**
```
virsh help | grep reboot
```
**Reset (hard reset/not safe) VM [último recurso]**

```
virsh reset ubuntu-vm1
```

**Criando snapshot e restaurando.** 

**listando os snapshot criados**
```
virsh snapshot-list vm1
```
**criando snapshot**
```
virsh snapshot-create-as --domain vm1 --name "snapshot_A" --description "MeuPrimeiroSnapshot" 
```
**Liste agora os snapshot
```
virsh snapshot-list vm1
```
**verificando os detalhes dos snapshot**
```
virsh snapshot-info --domain vm1 --current
```
**Para reverter para um snapshot (restauração de instantâneo)**
``` 
virsh snapshot-revert --domain vm1 --snapshotname "snapshot_A" 
``` 
**Para excluir um snapshot**
```
virsh snapshot-delete --domain vm1 --snapshotname "snapshot_A"
```
