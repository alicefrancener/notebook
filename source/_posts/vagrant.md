---
title: {{ title }}
tags:
---

# Vagrant

1. [Primeiros passos](#primeiros-passos)
2. [Configuração da rede](#configura%c3%a7%c3%a3o-da-rede)
   - [Forward Ports](#forward-ports)
   - 

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


### Rede privada (static ou dhcp)

As configurações de rede privada permitem acessar a máquina virtual somente através do host - seu computador.

#### IP estático

#### DHCP

### Rede pública (static ou dhcp)

As configuração de rede pública permitem acessar a máquina virtual através dos computadores em uma única rede pública.
