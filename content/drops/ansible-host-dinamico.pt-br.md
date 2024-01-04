---
author:
  name: "André Tecco"
date: 2024-01-03
title: Drops - Ansible Host Dinâmico
#categories:
#- Ansible
#- Automation
#- Monitoramento
tags:
- ansible
- dinamico
- drops
- host
linktitle: Drops - Ansible Host Dinâmico
type:
- post
- posts
- drop
- drops
weight: 10
series:
- Ansible
- Dinamico
- Drops
- Host
---

## Ansible Host Dinâmico
------------------------------------
> Abaixo uma rápida configuração do Ansible adicionando um host dinamicamente sem utilizar inventário (inventory).
> 
> Será utilizado um único arquivo com extenção yml. 
>

## Estrutura
------------
  - `deploy.yml` playbook com as instruções para adicionar o host e executar as tasks/role.

## Preenchimento de váriaveis (extra-vars).
--------------------------
  - server - Colocar o IP do host que será configurado.
  - -e - parâmetro que especifica uma ou mais váriaveis (extra_vars).

## Procedimento
--------------------------
1. Executando o playbook de configuração.
   - Executar o playbook: `ansible-playbook deploy.yml -e "server=10.0.0.1"`

2. Adiciona o host na primeira etapa, que foi especificado em '-e "server=10.0.0.1"'
   - A condicional 'when: server is defined and server != ""' verifica se a váriavel server existe e não está vazia.

3. Executa a tarefa no host que foi adicionado na etapa anterior e mostra o hostname (ansible_hostname)
## Conteúdo do Playbook
--------------------------

  - `deploy.yml`
    ```yaml
      - name: Adicionando Host Automaticamente
        hosts: localhost
        connection: local
        gather_facts: true
        pre_tasks:
          - name: Adicionando Host
            add_host:
              name: '{{ server }}'
              groups: dynamic
            when: server is defined and server != ""
      
      - name: Executando Task com Host adicionado
        hosts: dynamic
        gather_facts: true
        become: true
        tasks:
          - name: Mostrando infos sobre hostname
            debug:
              var: ansible_hostname

    ```