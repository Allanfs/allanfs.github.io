---
layout: post
title:  "Primeiro contato: Terraform"
date:   2023-11-22 22:00:00 -0300
categories: terraform
---

# Primeiras impressões

Vi que é possível utilizar Terraform localmente com *resource* local e até mesmo
[gerenciar cluster kubernetes através dele](https://registry.terraform.io/providers/hashicorp/kubernetes/latest)! Excelente pra se habituar com a
ferramenta aplicada ao k8s.

O Terraform é uma ferramenta de automação para provisionar e gerenciar recursos
nos mais diversos provedores de nuvem ou em data centers. Ele faz isso através
de providers, os quais definem as "classes e atributos" (falando em termos de
orientação a objeto) necessários para manipular seus recursos.

Com eles fazendo a configuração específica para cada *provedor* o Terraform se 
encarrega de executar as configurações definidas pelo dev. Providers e Terraform
falam a mesma lingua.

# A documetação e tutorial

A documentação do Terraform possui [vários tutoriais de uso com diferentes
providers](https://developer.hashicorp.com/terraform/tutorials). Eu [comecei pelo 
Docker](https://developer.hashicorp.com/terraform/tutorials/docker-get-started) 
e nele a doc guiou para a configuração de baixar uma imagem do nginx e criar um 
container dele.

Através disso ele ensinou sobre a sintaxe do arquivo `.tf`, como organizar a definição
dos recursos e comandos do Terraform como `apply`, `destroy`, `fmt` etc... Além 
de como utilizar variáveis e outputs nos manifestos.

Nesse começo deu pra ver que o básico da ferramenta é bem simples, a maior parte da
complexidade acaba vindo do provider pois você precisa entender seu conceito
para então organizar suas configurações no arquivo.

---

- https://www.terraform.io/
- https://developer.hashicorp.com/terraform?product_intent=terraform
- https://registry.terraform.io/browse/providers