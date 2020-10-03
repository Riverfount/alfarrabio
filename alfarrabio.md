# Dicas e truques de uso da linha de comando do GNU/Linux 

Todos os comandos abaixo foram testados no Debian Sid

## Como renomear a máquina do linux

1. Editar os arquivos e mudar para o novo nome: /etc/hostname e /etc/hosts.
2. Executar: `hostname -F /etc/hostname`
3. Fechar e abrir o terminal que já vai ver o hostname correto.

## Comparando arquivos

`dif -Naur arquivo1 arquivo2`

## Listando pacotes instalados:

`dpkg -l`

## Listando pacotes nos repositórios que usa:

`apt search chave-de-pesquisa`

## Lista com pacotes de todas as versões e seções, site:
https://www.debian.org/distrib/packages


## Como saber de onde os pacotes vêm:
```
for i in `dpkg --get-selections | awk '{ print $1 }'`; do egrep -lRI "^Filename: .*/${i}_[^/]+.deb" /var/lib/apt/lists/ | grep -q 'sid' && echo $i; done
```
