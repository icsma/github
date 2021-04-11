**Tutorial Gerenciado o KVM**


**Lista todas as vms**
```
virsh list --all
```
**Verificando informações**
```
virsh dominfo vm1
```
**Desligando vm**
```
virsh shutdown vm1
```

**Iniciando as VM**
```
virsh start vm1
```

**Para que a VM incie quando o servidor for reiniciado**
```
virsh autostart vm1
```
**Excluir VM**
```
virsh shutdown vm1
virsh undefine vm1
```
**para excluir o disco por completo da vm1**
```
rm -ri /var/lib/libvirt/images/vm1-os.qcow2
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
