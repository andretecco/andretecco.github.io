---
author:
  name: "André Tecco"
date: 2023-06-27T21:00:00-03:00
title: Ansible
categories:
- Ansible
- Automation
tags:
- ansible
- automation
linktitle: Ansible Localhost
type:
- post
- posts
weight: 10
series:
- Ansible
---


## Playbook Ansible para configuração Local
Playbook para configuração e instalação local no Ubuntu

Este Playbook foi testado com a versão:
- SO: Ubuntu 20.04
- Ansible: 2.9

> Para outras versões pode ser necessário alterar o nome dos pacotes e/ou acrescentar pacotes conforme a sua necessidade.

> Link para a Documentação do Ansible
https://docs.ansible.com/ansible/latest/index.html

> Link para a Instalação do Ansible
https://docs.ansible.com/ansible/latest/installation_guide/index.html

> Link para o repositório com a role utilizada para execução
https://github.com/andretecco/ansible-localhost

## Playbooks Ansible

O diretório principal ``ansible-localhost`` contém dois arquivos ``hosts`` e ``local.yml``.

- ``hosts`` - onde o Ansible consulta para saber em qual dispositivo deverá executar as ações.
- ``local.yml`` - onde é especificado quais conjuntos de regras serão executadas, são chamadas de ``roles``


O diretório ``localhost`` contém mais diretórios.

- ``tasks`` - onde estão as instruções que o Ansible deve seguir.
- ``vars`` - onde estão as variáveis que o Ansible irá buscar na execução dos Playbooks.
- ``handlers`` - onde estão as intruções para manipular os serviços, exemplo restart quando houver alterações nos arquivos de configuração.
- ``files`` - onde estãos os arquivos de configuração, por exemplo arquivos .conf.

Comando para execução de todo o conjunto de playbook.

- `ansible-playbook -i hosts local.yml -vvv`
> Pode ser necessário executar o comando com previlégios de super-usuário utilizando o comando sudo.