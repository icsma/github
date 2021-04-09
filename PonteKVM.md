**TUTORIAL REDE KVM PONTE BRIGDE**

**Instalando Dependências**

```
sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients virtinst bridge-utils
```

**Crie o arquivo com editor de texto de sua preferência**   
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

**Crie o arquivo com editor de texto de sua preferencia**
```
nano /etc/udev/rules.d/99-bridge.rules
```
**ou**
```
vi /etc/udev/rules.d/99-bridge.rules
```
**Cole o script abaixo. OBS: todo o script necessita esta na mesma linha para evitar erros**
```
ACTION=="add", SUBSYSTEM=="module", KERNEL=="br_netfilter", \           RUN+="/sbin/sysctl -p /etc/sysctl.d/bridge.conf"
```
**Iremos desabilitar as interfaces padrão criadas pelo KVM**
```
virsh net-destroy default
virsh net-undefine default
```
**Edite o arquivo abaixo com editor de sua preferencia**
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
    enp0s7:
      dhcp4: false
      dhcp6: false
  bridges:
    br0:
      interfaces: [ enp0s7 ]
      addresses: [192.168.0.104/24]
      gateway4: 192.168.0.1
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
```
sudo netplan apply
```  
**Crie o arquivo com editor de texto de sua preferencia**
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
```
virsh net-define host-bridge.xml
virsh net-start host-bridge
virsh net-autostart host-bridge
```
**Verifique se a rede foi ativada**
```
virsh net-list --all
```
