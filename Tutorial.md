**TUTORIAL KVM SEM CABEÇA**   


<div class=text-justify>

<img src="https://user-images.githubusercontent.com/51387190/112634160-0094a400-8e19-11eb-9ead-7bfe36c89dca.png" alt="topologia" title="Topologia KVM!" />

**Figura 1: Criação própria**

KVM (para máquina virtual baseada em kernel) se refere a uma tecnologia que comemora 10 anos de projeto, essa solução trás virtualização completa para linux em hardware x86 (Intel VT ou AMD-V).  O KVM é um software de código aberto capaz de virtualizar imagens não modificadas do linux ou windows, essa virtualização possui para cada máquina virtual um hardware virtualizado privado, além de uma placa de rede lógica, disco virtual etc. Para mais informações consulte: KVM.
Conforme mostrado na figura 1, é demonstrado o ambiente no qual foi realizado os testes, o servidor roda o sistema operacional ubuntu server na versão 20.04, possui um processador AMD, com uma placa de rede onboard, no qual  a rede é o endereço ip do roteador comodato do provedor de serviço de internet, após a instalação das dependências para rodar o kvm, recebe a placa de rede lógica juntamente da sub rede de faixa 192.168.120.0 e a rede responde pelo nome "virbr0".
Nesse tutorial será demonstrado a instalação do KVM sem cabeça no ubuntu 20.04, nesse ambiente não esta sendo utilizado interface gráfica conforme mostrado na figura , as configurações utilizada nesse servidor, necessitam que o processador tenha suporte a virtualização, para isso execute o comando abaixo.


                             
<img src="https://user-images.githubusercontent.com/51387190/112635152-39814880-8e1a-11eb-8628-11386a1b9cd2.png" alt="Terminal Linux" title="Terminal Linux" />

**Figura 2**

Instalação do KVM e suas respectivas dependências.

O primeiro passo é verificar a quantidade de núcleos que o servido físico possui. Isso é importante, para a criação das máquinas virtuais.





```
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```
O comando citado trás conforme mostrado na figura 3  os núcleos disponível, nesse caso são 4 núcleos e sera utilizado 3 dos 4 para cada máquina criada conforme visto na seção da topologia.

<img src="https://user-images.githubusercontent.com/51387190/112647492-5a03cf80-8e27-11eb-8656-5ca308440d54.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 3**

Para verificar se o processador suporta a virtualização que o kvm necessita, é necessario instalar o "cpu-checker" e utilizar o comando "kvm-ok".
```
sudo apt install cpu-checker
```

Na figura 4, é realizado a instalação, para instalar confirme com a tecla "Y".

<img src="https://user-images.githubusercontent.com/51387190/112647523-625c0a80-8e27-11eb-9c4d-f0871078d2f0.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 4**

Após a instalação, utilize o comando a seguir conforme visto na figura 5 "kvm-ok", o comando retornou que  "KVM acceleration can be used" ou seja, a aceleração ou virtualização pode ser usada.
```
kvm-ok
```
                                                                       

<img src="https://user-images.githubusercontent.com/51387190/112647555-6be57280-8e27-11eb-91cf-8eee7fe3cf44.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 5**

Agora é hora de instalar o KVM e suas respectivas dependências. Vale salientar que a parti desse momento é importante ter privilégios especiais, ou seja utilizar o usuário root para instalação e configuração do ambiente conforme mostrado na figura 6 abaixo. A tabela 1 abaixo representa as dependências e descrição dos pacotes a serem instalados.

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

Utilize o comando sudo mesmo se estiver como usuário root, é necessário utilizar os dois comandos conforme mostrado na figura 6 e 7.

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
os
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virtinst cpu-checker libguestfs-tools libosinfo-bin
```

<img src="https://user-images.githubusercontent.com/51387190/112647592-756eda80-8e27-11eb-8f3c-a621c0cca13f.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 6**
   
<img src="https://user-images.githubusercontent.com/51387190/112647618-7acc2500-8e27-11eb-9f43-5f6f0d4b584b.png" alt="checando os núcleos" title="checando os núcleos" />
**Figura 7**

Após a instalação ser concluída, verifique se o KVM que utiliza a biblioteca libvirtd para executar o serviço no linux está rodando. Observe na figura 8 que o serviço foi ativado.

```
sudo systemctl is-active libvirtd    
```
<img src="https://user-images.githubusercontent.com/51387190/112647659-83bcf680-8e27-11eb-8039-08fd1d6ed210.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 8**
Para que possa seja possível o gerenciamento das máquinas virtuais o recomendado é que seja adicionado o seu usuário do linux aos grupos "libvirt" e "kvm", para isso utilize a variável "$USER" de acordo com a figura 9 logo abaixo, assim a variável verificara o usuário no qual esta atualmente conectado. 
```
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

<img src="https://user-images.githubusercontent.com/51387190/112647678-87e91400-8e27-11eb-875a-ad0dca45b4d6.png" alt="checando os núcleos" title="checando os núcleos" />

**Figura 9**

É importante saber quais sãos os sistemas operacionais que rodam no "KVM", conforme mostrado na figura 10, é possível verificar uma quantidade enorme de sistemas operacionais que são suportados pelo "KVM". O comando para verificar essa lista de SO, está logo abaixo.
 
```
Comando = osinfo-query os
```


<img src=" https://user-images.githubusercontent.com/51387190/112647793-a2bb8880-8e27-11eb-8aea-75920e6dc331.png" alt="checando os núcleos" title="checando os núcleos" />                                                                                                                   **Figura 10**


Existem diversas fontes e automatização de scripts de como é criado as imagem para virtualização e gerenciamento, para esse tutorial será utilizado dois modos. O primeiro será manual onde você poderá baixar a ISO para o diretório corrente e executar a instalação, o segundo meio é por meio do "virt-builder" e imagens em nuvem. 

Instalação VM manual:

Criando as máquinas virtual por meio da ISO, baixando o arquivo bruto para o servidor de acordo com o diretório corrente.

Criando diretório, baixando a ISO e dando permissão. Para criação do diretório, utilize o comando "mkdir", seguido do nome para esse diretório. O comando "wget", fará com quer o mesmo baixe o arquivo onde a "ISO" dessa distribuição linux foi disponibilizada. A permissão utilizada "755", atribui a permissão de leitura, escrita e execução para o dono do arquivo (7), leitura e execução para usuários do mesmo grupo e para os demais usuários. Na figura 11, é possível acompanhar os comandos utilizados.
```
MKDIR osmedia
wget https://buildlogs.centos.org/rolling/7/isos/x86_64/CentOS-7-x86_64-Minimal-1910-01.iso
mv CentOS-7-x86_64-Minimal-1910-01.iso /home/kvmismael/osmedia
chmod 755 CentOS-7-x86_64-Minimal-1910-01.iso
```
Obs: Como a "ISO" foi baixado para o diretório /hom/kvmismael, foi necessário move para o diretório criado "osmedia".
                            

<img src="https://user-images.githubusercontent.com/51387190/112647822-ab13c380-8e27-11eb-8ecb-83b3540f576e.png" alt="checando os núcleos" title="checando os núcleos" /> 

**Figura 11**

