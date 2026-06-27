
# Provisionamento de Infraestrutura (Terraform)

Este diretório contém os códigos declarativos escritos em HCL (HashiCorp Configuration Language) utilizados para o provisionamento automatizado da infraestrutura no ambiente OpenStack.

O objetivo destes arquivos é definir o estado desejado do hardware virtual (máquinas, discos, chaves SSH e interfaces de rede) e gerar dinamicamente o inventário de instâncias que será consumido posteriormente pelo motor do Ansible.

---

## 1. Estrutura de Arquivos

A configuração foi modularizada nos seguintes arquivos para isolamento de contexto:

* `main.tf`: Declaração do provider e autenticação com a API da nuvem.
* `variables.tf`: Estrutura de dados (dicionário) contendo as especificações de cada máquina virtual.
* `instances.tf`: Lógica de iteração para criação dos recursos computacionais e do inventário local.

---

## 2. Configuração do Provider (`main.tf`)

O arquivo `main.tf` é responsável por inicializar o plugin de comunicação com o OpenStack e definir as rotas de acesso (endpoints) da API REST.

```hcl
terraform {
  required_providers {
    openstack = {
      source  = "terraform-provider-openstack/openstack"
      version = "~> 1.50.0" 
    }
  }
}

provider "openstack" {
  cloud = "labredes"

  endpoint_overrides = {
    "compute"  = "[http://10.10.2.9:8774/v2.1/90f1c80288444ed4bb4e41b5aa2d003f/](http://10.10.2.9:8774/v2.1/90f1c80288444ed4bb4e41b5aa2d003f/)"
    "network"  = "[http://10.10.2.9:9696/v2.0/](http://10.10.2.9:9696/v2.0/)"
    "image"    = "[http://10.10.2.9:9292/v2/](http://10.10.2.9:9292/v2/)"
    "volumev3" = "[http://10.10.2.9:8776/v3/90f1c80288444ed4bb4e41b5aa2d003f/](http://10.10.2.9:8776/v3/90f1c80288444ed4bb4e41b5aa2d003f/)"
    "identity" = "[http://10.10.2.9:5000/v3/](http://10.10.2.9:5000/v3/)"
  }
}
