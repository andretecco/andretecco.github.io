---
author:
  name: "André Tecco"
date: 2023-11-23
title: Ansible Semaphore UI
#categories:
#- Ansible
#- Automation
#- Monitoramento
tags:
- ansible
- user interface
- interface
- dashboard
- semaphore
linktitle: Ansible Semaphore UI
type:
- post
- posts
weight: 10
series:
- Ansible
- User Interface
- Interface
- Semaphore
---

## Semaphore UI
------------------------
> Semaphore é uma interface moderna para execução de instruções do Ansible (role, playbook, etc), pode ser considerado uma alternativa ao AWX para o homelab.
> 
> [Documentação - Semaphore UI](https://www.ansible-semaphore.com/)
> 
> [Documentação - AWX](https://github.com/ansible/awx)

## Estrutura
------------
O diretório principal utilizado para os testes é `/tmp/semaphore` se não quiser perder as informações a cada reboot, utilizar outro diretório.

## Informações Adicionais
-------------------------
Informações referente ao ambiente de testes utilizado.

Testes com a versão:
- SO: Debian GNU/Linux 12 (bookworm) e Armbian 23.8.1 (bullseye)
- Ansible: 2.9
- Semaphore UI: 2.9.37

## Configuração 
-----------------

1. Criando o ambiente.
   1. Debian: `mkdir -p /tmp/semaphore`
   2. Acessar o diretório: `cd /tmp/semaphore`
2. Download para instalação: `wget -c https://github.com/ansible-semaphore/semaphore/releases/download/v2.9.37/semaphore_2.9.37_linux_amd64.deb`  
3. Instalar o Semaphore: `sudo dpkg -i semaphore_2.9.37_linux_amd64.deb`
4. Configuração do Semaphore: `semaphore setup`
   - Obs.: As informações abaixo foram preenchidas para os testes, alterar caso tenha necessidade.
   - É necessário responder algumas perguntas, abaixo um exemplo.
   > #### What database to use:
   >   - Selecionar 2 - BoltDB é um banco de dados local
   >
   > #### db filename (default /tmp/semaphore/database.boltdb):
   >   - Enter para selecionar o default
   >
   > #### Playbook path (default /tmp/semaphore): 
   >   - Enter para selecionar o default
   >
   > #### Public URL (optional, example: https://example.com/semaphore):
   >   - Enter para selecionar o default, não será utilizado nesse post
   >
   > #### Enable email alerts? (yes/no) (default no):
   >   - Enter para selecionar o default, não será utilizado nesse post
   >
   > #### Enable telegram alerts? (yes/no) (default no):
   >   - Enter para selecionar o default, não será utilizado nesse post
   >
   > #### Enable slack alerts? (yes/no) (default no):
   >   - Enter para selecionar o default, não será utilizado nesse post
   >
   > #### Enable LDAP authentication? (yes/no) (default no):
   >   - Enter para selecionar o default, não será utilizado nesse post
   >
   > #### Config output directory (default /tmp/semaphore):
   >   - Enter para selecionar o default
   >
   > #### Preencher as informações abaixo conforme sua necessidade, todas as informações devem ser preenchidas.
   >   - Username: semaphore
   >   - Email: semaphore@localhost
   >   - Your name: semaphore
   >   - Password: semaphore
   >
   > #### Se tudo correr bem, deverá aparecer as informações abaixo, conforme as configurações feitas anteriormente.
   > You are all setup semaphore!
   > Re-launch this program pointing to the configuration file
   > 
   >./semaphore server --config /tmp/semaphore/config.json
   >
   > To run as daemon:
   >
   > nohup ./semaphore server --config /tmp/semaphore/config.json &

5. Executar o semaphore: `nohup semaphore server --config=./config.json &`
   - Executar o comando dentro da pasta /tmp/semaphore.

6. Acessar Semaphore UI
   - localhost:3000 (Utilizando o navegador da sua preferência)

7. Criar chaves ssh
   - Executar o comando dentro da pasta /tmp/semaphore
   - `ssh-keygen`
   >
   >  Generating public/private rsa key pair.
   >
   >  Enter file in which to save the key (/home/seu_usuario/.ssh/id_rsa): semaphore_rsa
   >  
   >  Enter passphrase (empty for no passphrase): Enter para deixar sem senha 
   >
   >  Enter same passphrase again: Enter para deixar sem senha
   >
   >  Your identification has been saved in semaphore_rsa
   >
   >  Your public key has been saved in semaphore_rsa.pub

   - Ao colocar semaphore_rsa, será gerado duas chaves, semaphore_rsa e semaphore_rsa.pub no diretório que foi executado o comando, no caso em /tmp/semaphore

8. Copiando chave publica (semaphore_rsa.pub) ssh para o host que será configurado.
   - `ssh-copy-id -i semaphore_rsa.pub seu_usuario@ip_host`

9. Configurar o Semaphore.
    
   - Playbook `nginx.yml` utilizado nos testes em /tmp/semaphore, que será citado na instrução Tasks Template

   ```yaml
     - name: Semaphore Homelab
       hosts: semaphore_nodes
       become: true
       tasks:
         - name: Instalando Nginx
           ansible.builtin.apt:
             name: nginx
             state: latest
             update_cache: true
   ```
   
   - Configurar um nome para o Projeto.
     ![Novo Projeto](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_new_project.png)

   - Acessar o menu Key Store e criar duas chaves, uma para o ssh e outra para o password par utilizar com sudo na execução do playbook pelo semaphore.

   - Key Store SSH
     - No campo Private Key, colocar o conteúdo do arquivo semaphore_rsa
     ![Key Store SSH](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_ssh_key_store.png)

   - Key Store Login with password
     - Preencher o campo password com a senha de acesso ao host
     ![Key Store Login with password](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_key_store.png)

   - Environment
     - Preencher o Extra Variables com alguma extra_vars
     ![Environment](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_environment.png)

   - Repositories
     - Preencher o campo URL or Path e Acces Key com a Key gerada somente com a senha.
     - O campo URL or Path colocar /tmp/semaphore onde estará o playbook utilizado para o teste.
     ![Repositories](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_repository.png)

   - Inventory
     - Configurar o inventory com as informações sobre o host que será configurado.
     ![Inventory](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_inventory.png)

   - Tasks Template
     - Configurar o template com informações do que deverá ser executado.
     - Em Playbook Filename é o playbook a ser executado que está na pasta /tmp/semaphore configurada em repository.
     ![Tasks Template](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_template.png)

   - Tasks Template Execução
     - Acessar Tasks Template e clicar em Run ao lado direito da tela.
     ![Tasks Template Execução](https://github.com/andretecco/andretecco.github.io/blob/main/images/images/semaphore-post/inicial_execucao_do_template.png)
