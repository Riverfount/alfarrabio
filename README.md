# Dicas e truques no VIM
## Selecionando uma coluna específica e incluíndo, por exemplo, o caracter de comentário (no python)
1. Digite `ctrl+v`
2. Com as teclas de movimentação e/ou as setas selecione as linhas que desaja
3. Digite `shift+i`
4. Digite o caracter `#`, ele aparecerá somente na primeira linha da seleção
5. Digite `Esc` e todas as demais linhas serão preenchidas com o mesmo caracter

## Substituindo um padrão de texo por outro
```
:%s/padrao_subistituivel/padrao_subistituto  # Subistitui somente a primeira ocorrência do padrao_subistituivel
```
No caso de se querer subistituir o padrão no arquivo inteiro, inserir `/g` ao final da instrução, ficando assim:
```
:%s/padrao_subistituivel/padrao_subistituto/g
```

## Abrindo um novo arquivo splitando o atual para a direita
```
:vsp new file_name.extension
```

## Abrindo um arquivo existente splitando o atual para a direita
```
:vsp file_name.extension
```

---

# Dicas e truques no Pycharm

## Removendo o nome do autor de alterações no código que é mostrado acima do bloco de código

Para remover basta ir em: `Settings > Editor > Inlay Hints` e desmarcar a opção `Code vision

---

# Dicas e truques de uso da linha de comando do GNU/Linux 

Todos os comandos abaixo foram testados no Debian Sid

## Verificado quem depende de um determinado pacote:
```
apt rdepends <nome-do-pacote>
```

## Como renomear a máquina do linux

1. Editar os arquivos e mudar para o novo nome: /etc/hostname e /etc/hosts.
2. Executar: `hostname -F /etc/hostname`
3. Fechar e abrir o terminal e verás o hostname correto.

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

## Após instalar o Docker incluir o usuário no grupo docker
```
su -
usermod -aG docker $USER
exit
```
Obs.: Não esquecer de restartar a máquina para recarregar os grupos.

## Mudando a diretório principal do Docker para evitar lotar a partição `/`:

Seguir as instruções desse link: https://www.ibm.com/docs/en/z-logdata-analytics/5.1.0?topic=compose-relocating-docker-root-directory

## Arrumando para não receber mensagem DEPRECATED na atualização dos pacotes
Após a instalação de um pacote que não esteja nos repositórios oficiais é possível que o mesmo tenha colocado a chave de autenticação no modo que está sendo depreciado pelo gerenciador de pacotes `apt`, para que não tenhamos sempre a mensagem de que está depreciado essa forma de usar a chave, vejamos como proceder.

O primeiro passo é listar as chaves que estão configuradas em seu sistema, para pegarmos o ID da chave que está nos dando a mensagem. Para isso, executamos o comando a seguir, o qual também mostramos um exemplo da saída que o mesmo fornece:

```
sudo apt-key list
[...]
pub   rsa4096 2016-02-18 [SCEA]
      DB08 5A08 CA13 B8AC B917  E0F6 D938 EC0D 0386 51BD
uid           [ unknown] https://packagecloud.io/slacktechnologies/slack (https://packagecloud.io/docs#gpg_signing) <support@packagecloud.io>
sub   rsa4096 2016-02-18 [SEA]
[...]
```

As reticências indicam que há saída similhar antes de depois do que precisamos. E, nesse exemplo, pegamos os dois últimos conjuntos de 4 caracteres do ID da chave que, nesse caso, são: `0386 51BD`

Realizado o passo acima executmos o código para exportar a chave para `.gpg`, com o seguinte comando:

```
apt-key export 038651BD | gpg --dearmour -o slack.gpg
```

O nome que demos é bem ilustrativo, ou seja, demos o nome da aplicação para a qual estamos gerando a chave, nesse nosso exemplo, o Slack. Outra observação importante é que pegamos os dois conjuntos finais do ID da chave e juntamos em um único bloco de informação.

Uma coisa que fiz que, só para manter um padrão, foi passar esse arquivo, que foi gerado em minh pasta home e, portanto, é do meu usuário e do meu grupo de usuário, para o usuário e grupo `root` com o seguinte comando:

```
sudo chwon root:root slack.gpg 
```

Feito isso, basta enviar para o diretório: `/etc/apt/trusted.gpg.d/`, isso com o comando a seguir:

```
sudo mv slack.gpg /etc/apt/trusted.gpg.d/slack.gpg
```
