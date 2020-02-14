---
title: {{ title }}
tags:
---

# Vagrant

1. [Primeiros passos](#primeiros-passos)
2. [Configuração da rede](#configura%c3%a7%c3%a3o-da-rede)
3. [Lidando com o SSH](#lidando-com-o-ssh)

## Primeiros passos

### Instalar

- Instalar **Vagrant**
- Instalar Hypervisor (monitor de máquina virtual): no caso utilizei [VirtualBox](https://www.vagrantup.com/docs/virtualbox/) compatível com Vagrant

### Criar e subir máquina virtual

- Criar um diretório para os arquivos
- Escolher [box](https://www.vagrantup.com/intro/getting-started/boxes.html)

```bash
# criar diretorio
mkdir <nome-do-diretorio>
cd <nome-do-diretorio>

# definir configurações da máquina virtual (nesse caso Ubuntu 12.04 => hashicorp/precise64) - cria o arquivo Vagrantfile
vagrant init <box-name>
vagrant init hashicorp/precise64

# criar (baixar e importar a box) e aplicar as configurações da máquina virtual
vagrant up
```

### Checar status

```bash
vagrant status
```

Na VirtualBox também é possível ver o status e se está funcionando.

### Conectar à máquina virtual com `ssh`

```bash
vagrant ssh

# mostra as configurações ssh
vagrant ssh-config

# desconectar da máquina
logout
exit
```

### Comandos básicos

```bash
# subir a máquina
vagrant up

# para a máquina
vagrant halt

# destruir a máquina
vagrant destroy

# reload da máquina (vagrant halt && vagrant up)
vagrant reload
```

[Vagrant > Comandos](https://www.vagrantup.com/docs/cli/)

## Configuração da rede

Conectados à máquina virtual (com `vagrant ssh`), para instalar um serviço como o servidor Nginx devemos atualizar a lista de pacotes e depois instalar o programa.

```bash
sudo apt-get update

sudo apt-get install -y nginx
```

O Nginx já estará rodando e para confirmar usar `netstat -lntp`. Há outro comando de envio de requisição - `curl http://localhost` - que  devolve em html uma confirmação de que o web server está rodando.

### Forward Ports

<small>Vagrant > Networking > [Forward Ports](https://www.vagrantup.com/docs/networking/forwarded_ports.html)</small>

Podemos usar uma porta específica em Windows, o qual recebe a requisição e acessa a máquina virtual automaticamente através do processo de *Port Forwarding* na rede. Para definir esta chamada, é necessário adicionar a linha de configuração network no arquivo "Vagrantfile", alterando para o host do Windows e salvando:

```ruby
Vagrant.configure("2") do |config|
    config.vm.network "forwarded_port", guest: 80, host:8080
end
```
 
O `host` é o hospedeiro (o sistema que permite o guest (convidado) rodar) e o `guest` é a máquina virtual.

Depois é necessário parar a máquina virtual (`vagrant halt`) e subir de novo (`vagrant up`) para que essa configuração seja aplicada.
E depois, acessando o navegador com `localhost:8080` irá aparecer a página html que foi retornada com o comando `curl http://localhost` na máquina virtual!

### Rede privada (*static* ou *dhcp*)

As configurações de rede privada permitem acessar a máquina virtual somente através do host - seu computador.

<small>Vagrant > Networking > [Private Network](https://www.vagrantup.com/docs/networking/private_network.html)</small>

> Vagrant private networks allow you to access your guest machine by some address that is not publicly accessible from the global internet. In general, this means your machine gets an address in the private address space.

#### DHCP

> The easiest way to use a private network is to allow the IP to be assigned via DHCP.

```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", type: "dhcp"
end
```

Para aplicar essa configuração, talvez seja necessário destruir a máquina e recriá-la (não esquecer de instalar novamente nginx).

São apresentados dois adaptadores: `nat` e `hostonly`. Ao executar `ipconfig` podemos ver as entradas no Windows. Posso usar o IP no navegador. 

#### IP estático

> You can also specify a static IP address for the machine. This lets you access the Vagrant managed machine using a static, known IP. The Vagrantfile for a static IP looks like this:

```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4"
end
```
### Rede pública (*static* ou *dhcp*)

As configuração de rede pública permitem acessar a máquina virtual através dos computadores em uma única rede pública.

<small>Vagrant > Networking > [Private Network](https://www.vagrantup.com/docs/networking/public_network.html)</small>

#### DHCP

```ruby
Vagrant.configure("2") do |config|
  config.vm.network "public_network"
end
```

São apresentados dois adaptadores: `nat` e `bridged`.

#### IP estático

```ruby
Vagrant.configure("2") do |config|
    config.vm.network "public_network", ip: "192.168.1.24"
end
```

***

## Lidando com o SSH

### Autenticação com chave privada gerada automaticamente pelo vagrant

O Vagrant gera automaticamente um par de chaves SSH. A chave pública fica na máquina virtual (guest), a chave privada fica no host. 
O comando `vagrant ssh-config` lista as configurações SSH que o comando `vagrant ssh` usará.

```bash
# tentando se conectar com ssh (essa tentativa gera um erro)
ssh vagrant@192.168.50.4
```

- No arquivo .ssh/known_host fica guardado o *fingerprint* de cada máquina com a qual o SSH se conectou. 
- Como criamos máquinas através do Vagrant com frequência, a ID muda e é preciso limpar esse arquivo ou apagá-lo quando der esse erro.

```bash
# apagar
rm .ssh/known_hosts

# tentando se conectar novamente utilizando a chave privada gerada automaticamente
ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@192.168.50.4
```

### Autenticação com chave privada gerada manualmente

```bash
# gerar uma chave
## o sistema pede para escolher local para salvar
## caso queira, pode criar senha para proteger a chave
ssh-keygen -t rsa

# copiar a chave publica para a máquina virtual
## a - acessar a máquina
vagrant ssh
## b - acessar a pasta automaticamente montada pelo Windows
ls /vagrant/
## c - copiar a chave da pasta vagrant para a máquina
cp /vagrant/id_bionic.pub
## d - adicionar ao arquivo authorized_keys
cat id_bionic.pub >> .ssh/authorized_keys
```

Agora, saindo da máquina virtual e retornando ao host usamos a chave privada para nos conectar remotamente à máquina virtual baseado em uma chave pública/privada.

```bash
ssh -i id_bionic vagrant@192.168.50.4
```
