## Task

 Instalação e configuração do Prometheus:

`yum install git ansible vim -y`{{execute}}

Após finalizar a instalação dos pacotes vamos acessar o diretório do Ansible:

`cd /etc/ansible`{{execute}}

Vamos configurar o inventário do Ansible, primeiro vamos criar um grupo chamado `local` no final do arquivo, esse grupo vai referenciar onde o Prometheus será instalado:

 `vim hosts`{{execute}}

```
[local]
127.0.0.1
```

As configurações feitas no arquivo `/etc/ansible/hosts` definem em quais dispositivos o Ansible irá se conectar para aplicar as configurações, é comum criarmos um grupo para os endereços entre colchetes, assim como definimos em `[local]`.

Agora vamos fazer um clone de um projeto que possui as instruções de instalação do Prometheus, esse projeto realizará a instalação de forma automática, para isso vamos executar o comando:


`git clone https://github.com/cloudalchemy/ansible-prometheus.git roles/ansible-prometheus `{{execute}}

Após finalizar o clone do projeto vamos criar a playbook que executará a role (tarefa) de instruções de instalação do Prometheus:


`vim prometheus-playbook.yml`{{execute}}


```yml
---
- name: "Instalação do Prometheus"
  hosts: local
  roles:
   - ansible-prometheus

```

O arquivo `/etc/ansible/prometheus-playbook.yml` definirá a role que será executada e onde ela será executada. Neste laboratório definimos que será executada a role `ansible-prometheus`, e que as instruções nessa role serão aplicadas a todos os hosts no grupo `local` (que só referencia o servidor `localhost`, ou seja, o próprio Prometheus).

Para executar as instruções como foram definidas no arquivo `prometheus-playbook.yml`, utilizamos o comando `ansible-playbook`:

`ansible-playbook prometheus-playbook.yml --connection=local`{{execute}}

Após o Ansible finalizar a execução das instruções para instalação do Prometheus, é possível acessá-lo a partir da porta **9090**, as áreas da interface são basicamente para consulta de métricas e configurações. Toda configuração do Prometheus é aplicada em seu arquivo de configuração em `/etc/prometheus/prometheus.yml`.

Clique no link abaixo  ou apenas clique na aba `"Port 9090"` para acessar o PromDash:


[Prometheus Website](https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/)
