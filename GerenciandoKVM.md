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

<img src="https://user-images.githubusercontent.com/51387190/114323524-fc94a180-9afb-11eb-9992-a657d963ead0.png" alt="topologia" title="incio automatico vms"/>

```
virsh autostart vm1
```
**Excluir VM**

<img src="https://user-images.githubusercontent.com/51387190/114324715-fb667300-9b01-11eb-8116-fb4eac725f70.png" alt="topologia" title="excluindo vms"/>

```
virsh shutdown vm1
virsh undefine vm1
```
**para excluir o disco por completo da vm1**

<img src="https://user-images.githubusercontent.com/51387190/114324750-2f419880-9b02-11eb-8f95-4da5daa17d54.png" alt="topologia" title="excluindo vms"/>

```
rm -ri /var/lib/libvirt/images/vm1-os.qcow2
```
**Para ver uma lista completa do tipo de comando virsh**

<img src="https://user-images.githubusercontent.com/51387190/114323559-24840500-9afc-11eb-9a26-2c79ea3c0dcb.png" alt="topologia" title="lista do comando virsh"/>

```
virsh help | less
```

**[último recurso], forçando reincio das vms**

<img src="https://user-images.githubusercontent.com/51387190/114323632-775dbc80-9afc-11eb-9428-d844a26f071f.png" alt="topologia" title="forca reboot"/>

```
virsh reset vm1
```

**Criando snapshot e restaurando.** 

**listando os snapshot criados**

<img src="https://user-images.githubusercontent.com/51387190/114324199-459a2500-9aff-11eb-80b0-fd928161010b.png" alt="topologia" title="lista snapshot"/>

```
virsh snapshot-list vm1
```

**criando snapshot**

<img src="https://user-images.githubusercontent.com/51387190/114324243-7712f080-9aff-11eb-927e-08c33965db4a.png" alt="topologia" title="forca reboot"/>

```
virsh snapshot-create-as --domain vm1 --name "snapshot_A" --description "MeuPrimeiroSnapshot" 
```
**Liste agora o snapshot criado**

<img src="https://user-images.githubusercontent.com/51387190/114324331-ceb15c00-9aff-11eb-9b3c-8501106d4787.png" alt="topologia" title="lista snapshot"/>

```
virsh snapshot-list vm1
```
**verificando os detalhes dos snapshot**

<img src="https://user-images.githubusercontent.com/51387190/114324348-df61d200-9aff-11eb-90cb-cc958d6c4a63.png" alt="topologia" title="detalhes snapshot"/>

```
virsh snapshot-info --domain vm1 --current
```

**Para reverter para um snapshot (restauração de instantâneo)**

<img src="https://user-images.githubusercontent.com/51387190/114324413-3071c600-9b00-11eb-999a-2c0bdb009447.png" alt="topologia" title="reverte snapshot"/>

``` 
virsh snapshot-revert --domain vm1 --snapshotname "snapshot_A" 
``` 
**Para excluir um snapshot**

<img src="https://user-images.githubusercontent.com/51387190/114324434-50a18500-9b00-11eb-9d9b-a1f3d35f8f5a.png" alt="topologia" title="reverte snapshot"/>

```
virsh snapshot-delete --domain vm1 --snapshotname "snapshot_A"
```
