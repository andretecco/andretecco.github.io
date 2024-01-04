---
author:
  name: "André Tecco"
date: 2023-12-08
title: Drops - Ansible Lint
#categories:
#- Ansible
#- Automation
#- Monitoramento
tags:
- ansible
- lint
- drops
- github
- actions
linktitle: Drops - Ansible Lint
type:
- post
- posts
- drop
- drops
weight: 10
series:
- Ansible
- Lint
- Drops
- Github
- Actions
---

## Ansible Lint com Github Actions
------------------------------------
> Abaixo uma rápida configuração do Ansible Lint junto ao Github Actions.
> 
> Repositório com o código utilizado. 
>   - [ansible-lint-msg](https://github.com/andretecco/ansible-lint-msg)
>
> Documentação do Ansible Lint.
>   - [Documentação - Ansible Lint](https://ansible.readthedocs.io/projects/lint/)

## Estrutura
------------
  - `.config` diretório para configuração personalizada do Ansible Lint.
  - `.github` diretório para configuração do Github Actions.

## Conteúdo dos diretórios
--------------------------
  - `.config/ansible-lint.yml`
    ```yaml
      ---
      # .ansible-lint
      
      profile: min # min, basic, moderate,safety, shared, production
      
      # Enforce variable names to follow pattern below, in addition to Ansible own
      # requirements, like avoiding python identifiers. To disable add `var-naming`
      # to skip_list.
      var_naming_pattern: "^[a-z_][a-z0-9_]*$"
      
      use_default_rules: true
      # Load custom rules from this specific folder
      # rulesdir:
      #   - ./rule/directory/
      
      # Some rules can transform files to fix (or make it easier to fix) identified
      # errors. `ansible-lint --fix` will reformat YAML files and run these transforms.
      # By default it will run all transforms (effectively `write_list: ["all"]`).
      # You can disable running transforms by setting `write_list: ["none"]`.
      # Or only enable a subset of rule transforms by listing rules/tags here.
      write_list:
        - all
    ```

  - `.github/workflows/ansible-lint.yml`
    ```yaml
      name: ansible-lint
      on:
        push:
          branches: ["*"]
        pull_request:
          branches: ["*"]
      jobs:
        build:
          name: Ansible Lint
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v4
            - name: Run ansible-lint
              uses: ansible/ansible-lint@v6
    ```