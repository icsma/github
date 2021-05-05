**KVM criando host Windows**

**1° baixe a ISO no site oficial da microsoft**

**Copie a iso para o servidor kvm, ou baixe diretamente a iso com o comando "wget" mais o link do download. Optei por baixar diretamente na minha máquina local e copiei com o comando "scp", conforme mostrado abaixo.**


<img src="https://user-images.githubusercontent.com/51387190/117079250-c8cf2500-ad11-11eb-97df-50b47c8c0f06.png" alt="copiando" title="vms"/>

```
scp Win10_20H2_v2_BrazilianPortuguese_x64.iso kvmismael@192.168.101.7:/home/kvmismael
```

**Após concluir a ISO está presente na maquina,conforme mostrado abaixo**

<img src="https://user-images.githubusercontent.com/51387190/117080264-db4a5e00-ad13-11eb-8779-83673141b652.png" alt="ISO" title="vms"/>

**2° Iremos criar um disco virtual, com 30G de armazenamento, crie de acordo com a sua necessidade, a minha foi para teste**

```
qemu-img create -f qcow2 /home/kvmismael/system.img 30G
```
<img src="https://user-images.githubusercontent.com/51387190/117080264-db4a5e00-ad13-11eb-8779-83673141b652.png" alt="DISCO" title="vms"/>

**3° iniciando a instalação com 4gb de memoria ram e 4 nucleos vcpus. OBS: É importante verificar o diretório do disco virtual criado e da ISO onde foi baixado ou copiada para o servidor kvm.**

<img src="https://user-images.githubusercontent.com/51387190/117080738-d1752a80-ad14-11eb-8909-5dc5d757cedc.png" alt="DISCO" title="vms"/>

```
virt-install --name windows_10 --ram 4096 --cpu host --hvm --vcpus=4 --file /home/kvmismael/system.img --cdrom /home/kvmismael/Win10_20H2_v2BrazilianPortuguese_x64.iso
```

**Para concluir a instalação, precisa ir para interface gráfica. Utilize um cliente de sua preferencia para se conectar pelo ip ou pelo ip local, foi utilizado o ip local dentro do servidor KVM.** 


<img src="https://user-images.githubusercontent.com/51387190/117081040-74c63f80-ad15-11eb-972b-d93e58d6edb3.png" alt="DISCO" title="vms"/>



<img src="https://user-images.githubusercontent.com/51387190/117081068-860f4c00-ad15-11eb-84fb-aac23abbec95.png" alt="windows10" title="vms"/>

**instalação inicial**


<img src="https://user-images.githubusercontent.com/51387190/117081580-89570780-ad16-11eb-99d5-a0a481e15e8c.png" alt="windows10" title="vms"/>

**Escolhendo a versão a ser utilizada.**

<img src="https://user-images.githubusercontent.com/51387190/117081696-c7ecc200-ad16-11eb-85b9-3b261b054115.png" alt="windows10" title="vms"/>

**Disco virtual criado na etapa 2. Finalize a instalação.**

**Referências:** 
https://www.geekpills.com/virtulization/nstalling-windows-xp-kvm-platform-ubuntu-16-04-lts
