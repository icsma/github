**TUTORIAL KVM SEM CABEÇA**   


<div class=text-justify>

<img src="https://user-images.githubusercontent.com/51387190/112634160-0094a400-8e19-11eb-9ead-7bfe36c89dca.png" alt="topologia" title="Topologia KVM!" />

**Topológia: Criação própria**

- KVM (para máquina virtual baseada em kernel) se refere a uma tecnologia que comemora 10 anos de projeto, essa solução trás virtualização completa para linux em hardware x86 (Intel VT ou AMD-V).  O KVM é um software de código aberto capaz de virtualizar imagens não modificadas do linux ou windows, essa virtualização possui para cada máquina virtual um hardware virtualizado privado, além de uma placa de rede lógica, disco virtual etc. Para mais informações consulte: KVM.
Na imagem abaixo é demonstrado o ambiente no qual foi realizado os testes, o servidor roda o sistema operacional ubuntu server na versão 20.04, possui um processador AMD, com uma placa de rede onboard, no qual  a rede é o endereço ip do roteador comodato do provedor de serviço de internet, após a instalação das dependências para rodar o kvm, recebe a placa de rede lógica juntamente da sub rede de faixa 192.168.120.0 e a rede responde pelo nome "virbr0".
Nesse tutorial será demonstrado a instalação do KVM sem cabeça no ubuntu 20.04, nesse ambiente não esta sendo utilizado interface gráfica conforme mostrado na figura abaixo, as configurações utilizada nesse servidor, necessitam que o processador tenha suporte a virtualização, para isso execute o comando abaixo.
                             
<img src="https://user-images.githubusercontent.com/51387190/112635152-39814880-8e1a-11eb-8628-11386a1b9cd2.png" alt="Terminal Linux" title="Terminal Linux" />

**1. Verificando a compatibilidade do processador com a virtualização**

- Verificando a quantidade de núcleos.

<img src="https://user-images.githubusercontent.com/51387190/112647492-5a03cf80-8e27-11eb-8656-5ca308440d54.png" alt="checando os núcleos" title="checando os núcleos" />

```
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```

- Para verificar se o processador suporta a virtualização que o kvm necessita, é necessario instalar o "cpu-checker" e utilizar o comando "kvm-ok".

<img src="https://user-images.githubusercontent.com/51387190/112647523-625c0a80-8e27-11eb-9c4d-f0871078d2f0.png" alt="checando os núcleos" title="checando os núcleos" />

```
sudo apt install cpu-checker
```
<img src="https://user-images.githubusercontent.com/51387190/112647555-6be57280-8e27-11eb-91cf-8eee7fe3cf44.png" alt="checando os núcleos" title="checando os núcleos" />

```
kvm-ok
```
**2. Instalação do KVM e suas respectivas dependências.**

- Agora é hora de instalar o KVM e suas respectivas dependências. Vale salientar que a parti desse momento é importante ter privilégios especiais, ou seja utilizar o usuário root para instalação e configuração do ambiente. 
- A tabela 1 abaixo representa as dependências e descrição dos pacotes a serem instalados.

Nome do Pacote | Descrição | Instalação
:---------: | :------: | :-------:
qemu-kvm | Virtualização completa QEMU em hardware x86 | KVM sem cabeça
libvirt-daemon-system | Arquivos de configuração do daemon Libvirt | KVM sem cabeça |
libvirt-clients | Programas para a biblioteca libvirt | KVM sem cabeça
bridge-utils |Interface lógica para criação da ponte | KVM sem cabeça
virtinst | Pacote para editar as máquinas virtuais | KVM sem cabeça
virt-manager | aplicativo de desktop para gerenciamento de máquinas virtuais | KVM sem cabeça
cpu-checker | ferramentas para ajudar a avaliar certos recursos da CPU (ou BIOS) | KVM sem cabeça
libguestfs-tools | Sistema de gerenciamento de imagem de disco convidado e ferramentas para imagens em nuvem | KVM sem cabeça
libosinfo-bin | Ferramentas para consultar o banco de dados osinfo | KVM sem cabeça

**Tabela 1 - Adaptado de  Autor: Vivek Gite.**  

- Utilize o comando sudo mesmo se estiver como usuário root, é necessário utilizar os dois comandos conforme mostrado abaixo.

<img src="https://user-images.githubusercontent.com/51387190/112647592-756eda80-8e27-11eb-8f3c-a621c0cca13f.png" alt="checando os núcleos" title="checando os núcleos" />

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager os
```

<img src="https://user-images.githubusercontent.com/51387190/112647618-7acc2500-8e27-11eb-9f43-5f6f0d4b584b.png" alt="checando os núcleos" title="checando os núcleos" />

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virtinst cpu-checker libguestfs-tools libosinfo-bin
```

- Verifique se o KVM que utiliza a biblioteca libvirtd para executar o serviço no linux está rodando. 

<img src="https://user-images.githubusercontent.com/51387190/112647659-83bcf680-8e27-11eb-8039-08fd1d6ed210.png" alt="checando os núcleos" title="checando os núcleos" />

```
sudo systemctl is-active libvirtd    
```

- Para que possa seja possível o gerenciamento das máquinas virtuais o recomendado é que seja adicionado o seu usuário do linux aos grupos "libvirt" e "kvm", para isso utilize a variável "$USER". 

<img src="https://user-images.githubusercontent.com/51387190/112647678-87e91400-8e27-11eb-875a-ad0dca45b4d6.png" alt="checando os núcleos" title="checando os núcleos" />

```
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

- É importante saber quais são os sistemas operacionais que rodam no "KVM", é possível verificar uma quantidade enorme de sistemas operacionais que são suportados pelo "KVM".

<img src="https://user-images.githubusercontent.com/51387190/112647793-a2bb8880-8e27-11eb-8aea-75920e6dc331.png" alt="checando os núcleos" title="checando os SO" />

```
Comando = osinfo-query os
```

**3. Instalação VM manual por meio da ISO:**

- Criando as máquinas virtual por meio da ISO, baixando o arquivo ISO para o servidor de acordo com o diretório corrente.

- Criando diretório, baixando a ISO e dando permissão. Para criação do diretório, utilize o comando "mkdir", seguido do nome para esse diretório. O comando "wget", fará com quer o mesmo baixe o arquivo onde a "ISO" dessa distribuição linux foi disponibilizada. A permissão utilizada "755", atribui a permissão de leitura, escrita e execução para o dono do arquivo (7), leitura e execução para usuários do mesmo grupo e para os demais usuários. 

<img src="https://user-images.githubusercontent.com/51387190/112647822-ab13c380-8e27-11eb-8ecb-83b3540f576e.png" alt="checando os núcleos" title="checando os núcleos" /> 

```
MKDIR osmedia
wget https://buildlogs.centos.org/rolling/7/isos/x86_64/CentOS-7-x86_64-Minimal-1910-01.iso
mv CentOS-7-x86_64-Minimal-1910-01.iso /home/kvmismael/osmedia
chmod 755 CentOS-7-x86_64-Minimal-1910-01.iso
```
**Obs: Como a "ISO" foi baixado para o diretório /hom/kvmismael, foi necessário move para o diretório criado "osmedia".**
                            
- Para essa configuração foi utilizado a placa de rede em modo bridge: virbr0 no qual o kvm cria por padrão na instalação, também foi necessário definir o nome da maquina virtual "vm1", assim como o tipo do "SO", para "centos7.0", a quantidade de memória ram "1024" e a quantidade de núcleos da cpu "1". Foi definido também o disco onde montara a "VM"  e o local que se encontra a ISO. 

<img src="https://user-images.githubusercontent.com/51387190/112647850-b0710e00-8e27-11eb-8651-aef42f25dca8.png" alt="checando os núcleos" title="checando os núcleos" /> 

```
virt-install  --network bridge:virbr0 --name vm1 \      
 --os-variant=centos7.0 --ram=1024 --vcpus=1  \         
 --disk path=/var/lib/libvirt/images/vm1-os.qcow2,format=qcow2,bus=virtio,size=5 \  
 --graphics none  --location=/home/kvmismael/osmedia/CentOS-7-x86_64-Minimal-1910-01.iso \
 --extra-args="console=tty0 console=ttyS0,115200"  --check all=off \
```
                
- **Obs: é importante fixar que na criação de uma outra "VM", é necessário trocar o "--name vm1" por outro nome, exemplo "--name vm2", e também alterar o nome onde sera construído o novo disco virtual como por exemplo "/vm1-os.qcw2" para "/vm2-os.qcw2"**

- Na instalação é necessário concluir cada campo conforme a imagem abaixo, apenas o campo 3, 9 e 4 ficam sem configurações. Os campos mostrado, são para definir senha de usuário root, se o disco definido anteriormente sera utilizado todo, se haverá configurações de rede fixa ou dhcp, a zona ou localização como "TimeZone" etc.  

<img src="https://user-images.githubusercontent.com/51387190/112647878-b666ef00-8e27-11eb-854a-5fe50a29811d.png " alt="checando os núcleos" title="checando os núcleos" /> 
                                                                       
<img src="https://user-images.githubusercontent.com/51387190/112647897-bbc43980-8e27-11eb-966f-6270c33b6c77.png" alt="checando os núcleos" title="checando os núcleos" />                                          
                                                                           
- Nessa etapa, a configuração foi concluída conforme mostrado abaixo, agora ao pressionar a tecla "Enter", a VM recém criada é iniciada.


<img src="https://user-images.githubusercontent.com/51387190/112647921-c088ed80-8e27-11eb-9487-73f418fa86d8.png" alt="checando os núcleos" title="checando os núcleos" />                                                                                                                                                 
                                                                                                                                                  
- Observe que o "usuário" digitado deve ser o "root" e a senha definida na instalação da "VM". após se logar, é possível verificar o ip recebido pelo "dhcp" da "KVM default". E o "ping", para testa a comunicação com o "Google".

<img src="https://user-images.githubusercontent.com/51387190/112647942-c54da180-8e27-11eb-8ceb-081ff145e04a.png" alt="checando os núcleos" title="checando os núcleos" />                                                                                 

**4. Instalação VM com "virt-builder":**


Comando | Descrição
:--------- | :------:
virt-install  --network bridge:virbr0 --name vm1 \ | Definindo interface lógica e o nome da vm
 --os-variant=centos7.0 --ram=1024 --vcpus=1  \ | O tipo do sistema operacional utilizado, memória ram e quantidade de nucleo.
 --disk path=/var/lib/libvirt/images/vm1 os.qcow2,format=qcow2,bus=virtio,size=5 \ | O caminho onde sera criado a vm virtual  
--graphics none  --location=/home/kvmismael/osmedia/CentOS-7-x86_64-Minimal-1910-01.iso \ | O caminho que se encontra a ISO.
--extra-args="console=tty0 console=ttyS0,115200"  --check all=off | O console para inicia o processo da instalação

**Tabela 2 - criação própria**

- **Obs: Verificando sistemas a serem instalado na nuvem, que der a suporte ao virt-builder, no qual exibi a lista de sistemas operacionais que podem ser baixadas e criadas as "VM".**

<img src="https://user-images.githubusercontent.com/51387190/112647955-c979bf00-8e27-11eb-82e1-07dd47420a9b.png" alt="checando os núcleos" title="checando os núcleos" />  

```
$ sudo virt-builder --list
```

- Assim como filtrando os sistemas operacionais "debian" e "ubuntu".

```
$ sudo virt-builder --list | egrep -i 'debian|ubuntu'
```


- Criando as variáveis de ambientes no linux, nessa parte do tutorial foi utilizado o "debian-9", pois o "debian-10" estava dando erro de repositório do próprio "virt-builder". 

<img src="https://user-images.githubusercontent.com/51387190/112647995-d26a9080-8e27-11eb-9bae-1c1ac6c18a9a.png" alt="checando os núcleos" title="checando os núcleos" />  

```
export vm="debian-9-vm1"                # Nome da VM
export os="debian-9"                    # o sistema operacional, tem que ser o nome.    
export tz="America/Fortaleza"           # Time zone.
export ram="1024"                       # Memória ram da VM.
export disk="10G"                       # O tamnaho do disco a ser utilizado.
export vcpu="1"                         # Números de núcleos a serem utilizados.
export key=/root/.ssh/id_rsa.pub        # Chave publica do SSH
export pwd="Encrypted_PASSWORD_HERE"    # Criptografando a menssagem para criação da senha.
export bridge="virbr0"                  # nome da interface lógica de rede 'br0' ou 'virbr0'
export ostype="debian9"                 # o tipo do SO definido.
```
                                                                         
- **Obs: Se por acaso, a variável declarada tiver um erro de digitação verifique se a "váriavel" existe e exclua para criar novamente.**

```
kvmismael:~$ env | grep key         #Verificando se a variavel key existe
kvmismael:~$ unset DUALCASE         #Caso a variável existe e cometeu um erro de sintaxe, apague e recrie com o comando "export".  
```

- Gerando o par de chave.


<img src="https://user-images.githubusercontent.com/51387190/112648017-d7c7db00-8e27-11eb-9b2b-f72a73211961.png" alt="checando os núcleos" title="checando os núcleos" />

```
ssh-keygen
```

- Criando a maquina virtual após declarar a variável de ambiente nos passos anteriores. 

- **Obs: É importante destacar, que após finalizar a pré instalação da VM, a senha criptografada é gerada aleatoriamente e pode ser vista logo abaixo na cor "pink".

<img src="https://user-images.githubusercontent.com/51387190/112648039-dd252580-8e27-11eb-86b4-8e52943e3dfd.png" alt="checando os núcleos" title="checando os núcleos" />  
       
<img src="https://user-images.githubusercontent.com/51387190/112648064-e1514300-8e27-11eb-8b29-152a536f7edb.png" alt="checando os núcleos" title="checando os núcleos" />  

```
$ sudo virt-builder "${os}" \
--hostname "${vm}" \
--network \
--timezone "${tz}" \
--size=${disk} \
--format qcow2 -o /var/lib/libvirt/images/${vm}-disk01.qcow2 \
--update \
--firstboot-command "dpkg-reconfigure openssh-server" \
--firstboot-command "useradd -p ${pwd} -s /bin/bash -m -d /root -G sudo root" \
--edit '/etc/default/grub:s/^GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="console=tty0 console=ttyS0,115200n8"/' \
--ssh-inject "root:file:${key}" \
--run-command update-grub
```

- Finalizando a instalação e importando a vm recém criada para o KVM, a imagem é criada e inicializada.

<img src="https://user-images.githubusercontent.com/51387190/112648078-e6ae8d80-8e27-11eb-9a2a-a1380b329c09.png" alt="checando os núcleos" title="checando os núcleos" />     

```
sudo virt-install --import --name "${vm}" \
--ram "${ram}" \
--vcpu "${vcpu}" \
--disk path=/var/lib/libvirt/images/${vm}-disk01.qcow2,format=qcow2 \
--os-variant "${ostype}" \
--network bridge:virbr0 --noautoconsole
```

- Verificando a imagem recém criada e se esta em execução.

<img src="https://user-images.githubusercontent.com/51387190/112648102-ec0bd800-8e27-11eb-9dbe-b2ff51c4247c.png" alt="checando os núcleos" title="checando os núcleos" />          

```
virsh list --all
```

- Entrando no modo console da "VM" e ativando interface de rede manualmente da vm2 criada, para isso usuário é root e a senha foi gerada aleatoriamente no processo anterior, caso deseje alterar a senha utilize o comando após logado "passwd" e altere a senha de root.


<img src="https://user-images.githubusercontent.com/51387190/112648142-f6c66d00-8e27-11eb-9a8a-395ea549f214.png" alt="checando os núcleos" title="checando os núcleos" /> 

```
virsh console vm2
```

<img src="https://user-images.githubusercontent.com/51387190/112648170-fded7b00-8e27-11eb-96f4-5d82b264617d.png" alt="checando os núcleos" title="checando os núcleos" />                    

- Verificando que a interface de rede existe, mas não está ativa.

```
ip a
```

- Utilize o editor de texto de sua preferencia, para alterar o nome da interface.

<img src="https://user-images.githubusercontent.com/51387190/112648184-03e35c00-8e28-11eb-9184-b5037101c623.png" alt="checando os núcleos" title="checando os núcleos" />  

```
vi /et/network/interfaces
```
                                                                      
- Altere o ens2, por enp1s0 de acordo com a imagem abaixo.

<img src="https://user-images.githubusercontent.com/51387190/112648202-0776e300-8e28-11eb-8e33-c7ebcd44095b.png" alt="checando os núcleos" title="checando os núcleos" />                                                                                                                      

- Alteração deve ficar de acordo com a interface existente conforme mostrado abaixo.

<img src="https://user-images.githubusercontent.com/51387190/112648215-0c3b9700-8e28-11eb-98ed-5f079529e0fb.png" alt="checando os núcleos" title="checando os núcleos" />  

- Restarte o serviço de rede e verifique se a rede recebeu "IP" vindo da sub-rede do "KVM", caso contrario, reinicie a máquina virtual com o comando "init 6" ou "reboot".

<img src="https://user-images.githubusercontent.com/51387190/112648231-1198e180-8e28-11eb-9b63-8ee713bd1eaf.png" alt="checando os núcleos" title="checando os núcleos" />

```
/etc/init.d/networking restart
```
- A máquina virtual recebe o ip por meio do DHCP do KVM. 

<img src="https://user-images.githubusercontent.com/51387190/112648276-1eb5d080-8e28-11eb-88e1-5824fa0e69f0.png" alt="checando os núcleos" title="checando os núcleos" />

**Referências:**

https://qastack.com.br/programming/6877727/how-do-i-delete-an-exported-environment-variable

https://buildlogs.centos.org/rolling/7/isos/x86_64/

https://help.ubuntu.com/community/KVM/Networking#Bridged_Networking

https://www.youtube.com/watch?v=zcBJRQkK7W4

https://wiki.debian.org/KVM

https://www.server-world.info/en/note?os=Ubuntu_20.04&p=kvm&f=2

https://libvirt.org/downloads.html

https://fabianlee.org/2019/04/01/kvm-creating-a-bridged-network-with-netplan-on-ubuntu-bionic/

https://www.cyberciti.biz/faq/how-to-install-kvm-on-ubuntu-20-04-lts-headless-server/

https://linuxize.com/post/how-to-install-kvm-on-ubuntu-20-04/

https://www.ucartz.com/clients/index.php?rp=/knowledgebase/605/How-to-install-KVM-server-on-Debian-Linux-9-Headless-Server.html

</div>
