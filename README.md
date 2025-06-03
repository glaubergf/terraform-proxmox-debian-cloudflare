---
Projeto: terraform-proxmox-debian-cloudflare
Descrição: Este projeto automatiza o provisionamento de um servidor Debian 12 (Bookworm) no Proxmox, utilizando o Terraform e Cloud-Init, realizando a instalação do Cloudflared em container Docker.
Autor: Glauber GF (mcnd2)
Data: 2025-05-07
---

![Image](https://github.com/glaubergf/terraform-proxmox-debian-cloudflare/blob/main/images/tf-pm-cloudflare.png)

![Image](https://github.com/glaubergf/terraform-proxmox-debian-cloudflare/blob/main/images/cloudflare-access.png)

![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)

# Servidor Debian Cloudflare (Docker)

## 📜 Sobre o Projeto

Este projeto automatiza a criação de uma VM Debian no Proxmox com Cloudflare Tunnel configurado automaticamente, permitindo expor serviços locais de forma segura e rápida pela infraestrutura da Cloudflare. Ideal para quem busca praticidade, segurança e agilidade na exposição de serviços.

## 🪄 O Projeto Realiza

- Download da imagem Debian noCloud.
- Criação da VM no Proxmox via QEMU.
- Configuração do sistema operacional via Cloud-Init.
- Instalação e configuração do Docker.
- Deploy do container do Cloudflared.

## 🧩 Tecnologias Utilizadas

![Terraform](https://img.shields.io/badge/Terraform-623CE4?logo=terraform&logoColor=white&style=for-the-badge)
- [Terraform](https://developer.hashicorp.com/terraform) — Provisionamento de infraestrutura como código (IaC).
 ---
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?logo=proxmox&logoColor=white&style=for-the-badge)
- [Proxmox VE](https://www.proxmox.com/en/proxmox-ve) — Hypervisor para virtualização.
---
![Cloud-Init](https://img.shields.io/badge/Cloud--Init-00ADEF?logo=cloud&logoColor=white&style=for-the-badge)
- [Cloud-Init](https://cloudinit.readthedocs.io/en/latest/) — Ferramenta de inicialização e configuração automatizada da VM.
---
![Debian](https://img.shields.io/badge/Debian-A81D33?logo=debian&logoColor=white&style=for-the-badge)
- [Debian 12 (Bookworm)](https://www.debian.org/) — Sistema operacional da VM.
---
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white&style=for-the-badge)
- [Docker](https://www.docker.com/) — Containerização da aplicação sysPass.
---
![Cloudflare](https://img.shields.io/badge/-Cloudflare-F38020?logo=cloudflare&logoColor=white&style=for-the-badge)
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/) — Túnel seguro.

## 🛠️ Pré-requisitos

- ✅ Proxmox VE com acesso à API.
- ✅ Usuário no Proxmox com permissão para criação de VMs.
- ✅ Conta na Cloudflare.
- ✅ Token API da Cloudflare com permissões para Tunnels.
- ✅ Terraform instalado localmente.

## 📂 Estrutura do Projeto

```
terraform-proxmox-debian-cloudflare
├── configs
│   ├── cloud_config.yml
│   ├── config_motd.sh
│   ├── docker-compose.yml
│   ├── motd_cloudflare
│   ├── network_config.yml
│   └── vm_template.sh
├── images
│   ├── cloudflare-access.png
│   └── tf-pm-cloudflare.png
├── LICENSE
├── notes
│   ├── art_ascii_to_modt.txt
│   └── docker-compose.yml.txt
├── output.tf
├── provider.tf
├── README.md
├── security
│   ├── .env
│   ├── proxmox_id_rsa
│   └── proxmox_id_rsa.pub
├── terraform.tfvars
├── variables.tf
└── vm_proxmox.tf
```

### 📄 Arquivos

- `provider.tf` → Provedor do Proxmox
- `vm_proxmox.tf` → Criação da VM, configuração da rede, execução dos scripts
- `variables.tf` → Definição de variáveis
- `terraform.tfvars` → Valores das variáveis (customização)
- `cloud_config.yml` → Configurações do Cloud-Init (usuário, pacotes, timezone, scripts)
- `network_config.yml` → Configuração de rede estática
- `.env` → Token de acesso ao Cloudflare Tunnel

## 🚀 Fluxo de Funcionamento

1. **Terraform Init:** Inicializa o Terraform e carrega os providers e módulos necessários.

2. **Download da imagem Debian noCloud:** Baixa a imagem Debian pré-configurada (noCloud) se ainda não estiver no Proxmox.

3. **Criação da VM no Proxmox:** Terraform cria uma VM no Proxmox com base nas variáveis definidas.

4. **Aplicação do Cloud-Init:** Injeta configuração automática na VM (rede, usuário, SSH, hostname, etc.).

5. **Configuração inicial da VM:** A VM é inicializada e aplica configurações básicas: acesso remoto, hostname, rede, etc.

6. **Instalação do Docker:** Scripts do Cloud-Init instalam Docker e Docker Compose na VM.

7. **Deploy do container Cloudflared:** O Docker Compose sobe o container do Cloudflared.

## 🐧 Debian

Distribuição Linux livre, estável e robusta. A imagem utilizada é baseada em **Debian noCloud**, que permite integração com Cloud-Init no Proxmox.

Saiba mais: [https://www.debian.org/](https://www.debian.org/)

### ☁️ Sobre a imagem Debian nocloud

Este projeto utiliza a imagem Debian nocloud por maior estabilidade no provisionamento via Terraform no Proxmox, evitando problemas recorrentes como **kernel panic** em outras versões (*generic*, *genericcloud*).

## ☁️ Cloud-Init

Ferramenta de provisionamento padrão de instâncias de nuvem. Permite configurar usuários, pacotes, rede, timezone, scripts e mais, tudo automaticamente na criação da VM.

Saiba mais: [https://cloudinit.readthedocs.io/](https://cloudinit.readthedocs.io/)

## 🔐 Cloudflare

O Cloudflare usa o *cloudflared* que é um cliente de linha de comando desenvolvido pela própria Cloudflare que permite estabelecer conexões seguras entre seus serviços locais e a rede global da Cloudflare, sem a necessidade de expor diretamente esses serviços à internet pública. Ele atua como um proxy reverso baseado em uma arquitetura de *"zero trust"* (confiança zero).

Saiba mais: [https://www.cloudflare.com/pt-br/](https://www.cloudflare.com/pt-br/)

## 🛠️ Terraform

Ferramenta de IaC (Infrastructure as Code) que permite definir e gerenciar infraestrutura através de arquivos de configuração declarativos.

Saiba mais: [https://developer.hashicorp.com/terraform](https://developer.hashicorp.com/terraform)

## 🐳 Docker

Plataforma que permite empacotar, distribuir e executar aplicações em containers de forma leve, portátil e isolada, facilitando a implantação e escalabilidade de serviços.

Saiba mais: [https://www.docker.com](https://www.docker.com)

## ⚙️ Execução do Projeto

1. Clone o repositório:
```bash
git clone https://github.com/glaubergf/terraform-proxmox-debian-cloudflare.git
cd terraform-proxmox-debian-cloudflare
```

2. Configure as variáveis em `terraform.tfvars`.

3. Inicialize o Terraform:
```bash
terraform init
```

4. Execute o plano:
```bash
terraform plan
```

5. Aplique a infraestrutura:
```bash
terraform apply
```

6. Para destruir toda a infraestrutura criada:
```bash
terraform destroy
```

✅ Use `--auto-approve` para evitar confirmações manuais.

## 💡 Contribuição

Contribuições são bem-vindas!

## 📜 Licença

Este projeto está licenciado sob os termos da **[GNU General Public License v3](https://www.gnu.org/licenses/gpl-3.0.html)**.

### 🏛️ Aviso Legal

```
Copyright (c) 2025

Este programa é software livre: você pode redistribuí-lo e/ou modificá-lo
sob os termos da Licença Pública Geral GNU conforme publicada pela
Free Software Foundation, na versão 3 da Licença.

Este programa é distribuído na esperança de que seja útil,
mas SEM NENHUMA GARANTIA, nem mesmo a garantia implícita de
COMERCIALIZAÇÃO ou ADEQUAÇÃO A UM DETERMINADO FIM.

Veja a Licença Pública Geral GNU para mais detalhes.
```
