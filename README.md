# NetHunter DPKG Fixer

Este script foi desenvolvido para resolver erros críticos de dependências e configuração de pacotes (como `systemd`, `udev` e `libselinux1`) em ambientes **Kali NetHunter (chroot/proot)** rodando sobre o Android.

## O Problema

O Kali NetHunter compartilha o Kernel do Android, que não possui suporte a drivers de protocolo exigidos pelo `systemd` moderno (erro: *Protocol driver not attached*). Isso causa um travamento no `dpkg`, impedindo instalações e atualizações via `apt`.

## O que este script faz?

**Init-less Compatibility:** Cria um `systemctl` falso (`/bin/true`) e gera um `machine-id` válido.

**Post-inst Neutralization:** Esvazia scripts de pós-instalação que tentam interagir com o hardware/kernel do Android.

**AWK Status Patch:** Manipula o banco de dados do `dpkg` (`/var/lib/dpkg/status`) usando **AWK** para forçar o estado de instalação e remover campos de configuração conflitantes (`Config-Version`).

**Environment Cleanup:** Limpa o cache do APT e atualiza os repositórios.

> [!CAUTION]
> ### **AVISO LEGAL E DE SEGURANÇA**
> **ESTE SCRIPT FOI CRIADO EXCLUSIVAMENTE PARA AMBIENTES MOBILE (NETHUNTER/TERMUX).**
> Não execute este script em uma instalação nativa de desktop (PC/Laptop). Ele desativa componentes críticos do `systemd` que são necessários para o boot e funcionamento de hardware real. Use por sua conta e risco

Desenvolvido por:[![Telegram](https://img.shields.io/badge/Telegram-Contact-blue?style=for-the-badge&logo=telegram)](https://t.me/cybe4)


## Como usar

Clone o repositório e execute o script com privilégios de root dentro do seu terminal NetHunter:

```bash
curl -skL https://raw.githubusercontent.com/qrt2/fix_nethunter/main/fix_nethunter.sh | bash
```
