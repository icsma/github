**Tutorial Gerenciado o KVM**

**Lista todas as vms**

<img src="https://user-images.githubusercontent.com/51387190/114323313-f3ef9b80-9afa-11eb-9c6c-6be743809624.png" alt="topologia" title="listando vms"/>

```
virsh list --all
```
**Verificando informações**

<img src="https://user-images.githubusercontent.com/51387190/114323374-42049f00-9afb-11eb-8c1d-ad74084114fa.png" alt="topologia" title="verificando informacoes"/>

```
virsh dominfo vm1
```
**Desligando vm**

<img src="https://user-images.githubusercontent.com/51387190/114323463-b3445200-9afb-11eb-9d1f-12f4dd828848.png" alt="topologia" title="desligando vms"/>

```
virsh shutdown vm1
```
**Iniciando as VM**
<img src="https://user-images.githubusercontent.com/51387190/114323494-dec73c80-9afb-11eb-94e4-2813b7b8f7db.png" alt="topologia" title="iniciando vms"/>

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
