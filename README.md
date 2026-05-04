# NetHunter DPKG Fixer

Se você está usando o NetHunter 2026 (especialmente a versão Full/Desktop) e o seu apt upgrade ou apt install travou com erros de **"post-installation script"**, este script é a solução.
​Ele corrige travamentos causados por:
​Novas funções de `Systemd`: O Kali moderno tenta ativar serviços que o Kernel do Android não suporta.
​Conflitos de Desktop: Erros em pacotes como polkitd, network-manager, orca e pkexec.
**​Protocol driver not attached:** Mata o loop infinito do dpkg tentando configurar o que o Android bloqueia.
​**Machine-ID & D-Bus:** Gera identificadores válidos para evitar falhas em ferramentas gráficas e de rede.

## ​Por que usar?

​As atualizações recentes do Kali inseriram verificações de hardware mais rigorosas. Como o chroot compartilha o Kernel do Android, o dpkg quebra ao tentar acessar drivers de protocolo inexistentes. Este script realiza uma "cirurgia" no banco de dados (/var/lib/dpkg/status) usando AWK para forçar o estado de instalação e neutralizar os gatilhos que travam o seu terminal.

## O que este script faz?

**Init-less Compatibility:** Cria um `systemctl` falso (`/bin/true`) e gera um `machine-id` válido.

**Post-inst Neutralization:** Esvazia scripts de pós-instalação que tentam interagir com o hardware/kernel do Android.

**AWK Status Patch:** Manipula o banco de dados do `dpkg` (`/var/lib/dpkg/status`) usando **AWK** para forçar o estado de instalação e remover campos de configuração conflitantes (`Config-Version`).

**Environment Cleanup:** Limpa o cache do APT e atualiza os repositórios.

**Reverter Alterações**
Caso precise restaurar o banco de dados original do dpkg:

```bash
cp /var/lib/dpkg/status.bak /var/lib/dpkg/status
```

> [!CAUTION]
> ### **AVISO LEGAL E DE SEGURANÇA**
> **ESTE SCRIPT FOI CRIADO EXCLUSIVAMENTE PARA AMBIENTES MOBILE (NETHUNTER/TERMUX).**
> Não execute este script em uma instalação nativa de desktop (PC/Laptop). Ele desativa componentes críticos do `systemd` que são necessários para o boot e funcionamento de hardware real.
> Este script realiza modificações diretas no banco de dados do DPKG. Embora um backup seja criado, use com cautela em ambientes de produção."

Desenvolvido por:[![Telegram](https://img.shields.io/badge/Telegram-Contact-blue?style=for-the-badge&logo=telegram)](https://t.me/cybe4)


## Como usar

Clone o repositório e execute o script com privilégios de root dentro do seu terminal NetHunter:

```bash
curl -skL https://raw.githubusercontent.com/qrt2/fix_nethunter/main/fix_nethunter | bash
```
## Via BusyBox (Ideal para NetHunter Minimal)
Se o seu sistema está "limpo" e não possui curl ou wget nativos, o busybox (que sempre vem no NetHunter) resolve:

```bash
busybox wget --no-check-certificate -O- https://raw.githubusercontent.com/qrt2/fix_nethunter/main/fix_nethunter | bash
```
## Via Método Manual (Cat) - Em caso de falha de rede
Se você prefere ver o código antes ou se os comandos acima falharem, siga estes passos:

Acesse o código fonte bruto (RAW) aqui: * **[Clique aqui para Visualizar o Script RAW](https://raw.githubusercontent.com/qrt2/fix_nethunter/main/fix_nethunter)**


Copie todo o código da página

No seu terminal NetHunter, digite:

`cat > fix.sh`

Cole o código, pressione Enter para garantir uma linha em branco e depois Ctrl + D para salvar

```bash
chmod +x fix.sh && ./fix.sh
```
