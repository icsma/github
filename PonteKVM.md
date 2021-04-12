**TUTORIAL REDE KVM PONTE BRIGDE**

**Instalando Dependências**

<img src="https://user-images.githubusercontent.com/51387190/114325001-07ebcb00-9b04-11eb-8c47-14e3f260dd3d.png" alt="topologia" title="instalando dependencias" />

```
sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients virtinst bridge-utils
```

**Crie o arquivo para criar a rede bridge com editor de texto de sua preferência**   

<img src="https://user-images.githubusercontent.com/51387190/114325058-6ca72580-9b04-11eb-82bb-5dc183bacd46.png" alt="topologia" title="arquivo bridge" />

```
nano /etc/sysctl.d/bridge.conf
```
**ou**
```
vi /etc/sysctl.d/bridge.conf
```
**Cole o script abaixo**

```
net.bridge.bridge-nf-call-ip6tables=0
net.bridge.bridge-nf-call-iptables=0
net.bridge.bridge-nf-call-arptables=0
```

**Crie uma regra no arquivo com editor de texto de sua preferencia**

<img src="https://user-images.githubusercontent.com/51387190/114325073-834d7c80-9b04-11eb-8b65-98c5528fa316.png" alt="topologia" title="criando regra" />

```
nano /etc/udev/rules.d/99-bridge.rules
```
**ou**
```
vi /etc/udev/rules.d/99-bridge.rules
```
Cole o script abaixo. 
**OBS: todo o script necessita esta na mesma linha para evitar erros**

```
ACTION=="add", SUBSYSTEM=="module", KERNEL=="br_netfilter", \           RUN+="/sbin/sysctl -p /etc/sysctl.d/bridge.conf"
```
**Iremos desabilitar as interfaces padrão criadas pelo KVM**
<img src="https://user-images.githubusercontent.com/51387190/114325084-96f8e300-9b04-11eb-897a-f26e9203c460.png" alt="topologia" title="desabilitando interface" />
<img src="https://user-images.githubusercontent.com/51387190/114325089-a37d3b80-9b04-11eb-9228-e75823485b5d.png" alt="topologia" title="desabilitando interface" />

```
virsh net-destroy default
virsh net-undefine default
```
**Edite o arquivo abaixo com editor de sua preferencia**
<img src="https://user-images.githubusercontent.com/51387190/114325173-15ee1b80-9b05-11eb-9b13-2d685ecc95b6.png" alt="topologia" title="alterando arquivo" />

```
nano /etc/netplan/00-installer-config.yaml 
```
**ou**
```
vi /etc/netplan/00-installer-config.yaml 
```
**Cole o script abaixo e altere o nome da interface pela sua interface física e as configurações de rede**
```
network:
  ethernets:
    ens33:
      dhcp4: false
      dhcp6: false
  bridges:
    br0:
      interfaces: [ ens33 ]
      addresses: [192.168.101.16/24]
      gateway4: 192.168.101.1
      mtu: 1500
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
      parameters:
        stp: true
        forward-delay: 4
      dhcp4: no
      dhcp6: no
  version: 2
```

**Aplique as configurações realizadas com o comando abaixo** 
<img src="https://user-images.githubusercontent.com/51387190/114326009-21434600-9b09-11eb-908c-bcde444b7fb4.png" alt="topologia" title="aplicando configurações" />
```
sudo netplan apply
```  
**Crie o arquivo com editor de texto de sua preferencia**
<img src="https://user-images.githubusercontent.com/51387190/114325315-cd832d80-9b05-11eb-90de-f74a1f14daf2.png" alt="topologia" title="criando arquivo de rede" />

```
nano host-bridge.xml
```
**ou**
```
vi host-bridge.xml
```
**cole o script abaixo**

```  
<network>
  <name>host-bridge</name>
  <forward mode="bridge"/>
  <bridge name="br0"/>
</network>
```

**execute os comandos abaixo**

<img src="https://user-images.githubusercontent.com/51387190/114325330-ea1f6580-9b05-11eb-8bdf-f7940355321e.png" alt="topologia" title="executando script" />


```
virsh net-define host-bridge.xml
virsh net-start host-bridge
virsh net-autostart host-bridge
```
**Verifique se a rede foi ativada**

<img src="https://user-images.githubusercontent.com/51387190/114325535-fa841000-9b06-11eb-98bd-7d267fcf7c1d.png" alt="topologia" title="verificar o status" />
```
virsh net-list --all
```
