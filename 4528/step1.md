## Task

 Instalação e configuração do Prometheus:

`yum install git ansible vim -y`{{execute}}

Após finalizar a instalação dos pacotes vamos acessar o diretório do Ansible:

`cd /etc/ansible`{{execute}}

Em seguida vamos editar o arquivo de configuração do Ansible descomentando os parâmetros:

```bash
 vim ansible.cfg
```

```
host_key_checking = False
remote_user = root
```

As configurações aplicadas ao arquivo `/etc/ansible/ansible.cfg` são:

`host_key_checking = False`
Usado para se conectar via SSH, aceitando automaticamente o hash de identificação do dispositivo e armazenando em `~/.ssh/known_hosts`.

`remote_user = root`
Para definir que o usuário remoto para conexão será sempre `root`.

Ajustes feitos, vamos configurar o inventário do Ansible, primeiro vamos criar um grupo chamado `local`, esse grupo vai referenciar onde o Prometheus será instalado:


```bash
 vim hosts
```

```
[local]
127.0.0.1
```

As configurações feitas no arquivo `/etc/ansible/hosts` definem em quais dispositivos o Ansible irá se conectar para aplicar as configurações, é comum criarmos um grupo para os endereços entre colchetes, assim como definimos em `[local]`.

Agora vamos fazer um clone de um projeto que possui as instruções de instalação do Prometheus, esse projeto realizará a instalação de forma automática, para isso vamos executar o comando:


`git clone https://github.com/cloudalchemy/ansible-prometheus.git roles/ansible-prometheus `{{execute}}

Após finalizar o clone do projeto vamos criar a playbook que executará a role (tarefa) de instruções de instalação do Prometheus:


```bash
 vim prometheus-playbook.yml
```

```yml
---
- name: "Instalação do Prometheus"
  hosts: local
  roles:
   - ansible-prometheus

```

O arquivo `/etc/ansible/prometheus-playbook.yml` definirá a role que será executada e onde ela será executada. Neste laboratório definimos que será executada a role `ansible-prometheus`, e que as instruções nessa role serão aplicadas a todos os hosts no grupo `local` (que só referencia o servidor `localhost`, ou seja, o próprio Prometheus).

Para executar as instruções como foram definidas no arquivo `prometheus-playbook.yml`, utilizamos o comando `ansible-playbook`:

`ansible-playbook prometheus-playbook.yml`{{execute}}
