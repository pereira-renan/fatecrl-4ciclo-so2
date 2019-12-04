# Gabaritos - Sistemas Operacionais II


## Iniciando a construção de um shell script

Para iniciar a construção de um script devemos iniciar com o comando touch e editar o arquivo criado utilizando o editor de texto vi
```
touch nomedoarquivo.sh
vi nomedoarquivo.sh
```
Finalizada a edição, saímos do vi com 'Ctrl+C -> :wq' para sair e salvar o arquivo.

Feito isso, para habilitar a execução do script dentro da Conectiva10, executamos o comando abaixo para alteração de permissões:
```
chmod +x nomedoarquivo.sh
```

### Básicos do shell script

A primeira linha dentro do script é aonde acontece a chamada do interpretador que executará o script, neste caso é utilizado o bash
```
#!/bin/bash
```

Feito isso a estrutura segue da seguinte forma:

Estruturar uma nova função
```
menu () {
  echo
  echo "1) Identificar um usuário"
  echo "2) Criar um usuário"
  echo "3) Apagar um usuário"
  echo "4) Sair"
  echo
  
  read -p "Digite a opção desejada: " opt; echo
  case $opt in
    1) id;;
    2) criar;;
    3) apagar;;
    4) sair;;
    *) echo "Opção inválida. Tente novamente."; menu;;
  esac
}
```

A função acima exemplifica um menu que realiza chamadas para outras funções aceitando o input do usuário e validando em um condicional case. Caso encontrado, chamada a outra função, caso não encontrado, retorna uma mensagem de erro.

Abaixo segue exemplo de outra função que retorna ao menu geral para que o programa tenha continuidade.
```
id () {
  read -p "Digite o nome do usuário: " nome; echo
  if [ "$(cat /etc/passwd | grep -i $nome | wc-l)" = "1" ]; then
    echo "Usuário: $user"
    id $user
    sleep 3; menu
  else
    echo
    echo "Usuário $user não cadastrado"
    sleep 3; menu
  fi
}
```

Nesse trecho é executada uma condicional if e após cada verificação acontece uma chamada para a função menu, isso torna o programa mais fluído e permite que seja executado continuamente ou até que o usuário escolha encerrar o programa.

No final do script também é necessário adicionar uma linha que indica por qual função o script deve iniciar a execução.
```
sair () {
  echo
  echo "Finalizando"
  sleep 3;
  exit
}
menu <-- script iniciará executando a função menu
```

## Comandos mais utilizados

Echo - printa uma string na tela
```
echo "mensagem" || printa uma string em tela
echo -e "string1 \nstring2 \nstring3" || printa, entende \n como quebra e continua na próxima linha
echo -n "string" || printa e permanece na mesma linha
```
Read - lê e atribui um string a uma variável
```
read -p "string aqui: " variavel || printa o texto em tela, aceita e salva o input do usuário
read -sp "digite sua senha: " senha || printa o texto em tela, aceita e salva o input do usuário mas oculta os caracteres digitados
```

If
```
~STRINGS~
if [ -z nome ]; then || verifica se é nulo
if [ -n nome ]; then || verifica se NÃO é nulo
if [ nome = novoNome ]; then || verifica se é igual a
if [ nome != novoNome]; then || verifica se é diferente de
if [ nome != novoNome]; then || verifica se é diferente de

~NUMÉRICA~
if [ altura -lt novaAltura]; then || comparação é menor que tal
if [ altura -gt novaAltura]; then || comparação é maior que tal
if [ altura -le novaAltura]; then || comparação é menor ou igual a tal
if [ altura -ge novaAltura]; then || comparação é maior ou igual a tal
if [ altura -eq novaAltura]; then || comparação é igual a tal
if [ altura -ne novaAltura]; then || comparação é diferente de tal

~OPERADORES LÓGICOS~
~= - NOT
&& - AND
|| - OR

if [ -z nome ]; then || condicional de entrada
elif [ -n nome ]; then || segunda condicional
fi || encerrar condicional
```

Cálculo básico atribuído a uma variável
```
imc=`echo "scale=2; $peso/($altura * $altura)" | bc | sed ´s/\.//´`
scale define quantos decimais são utilziados, bc é um comando utilizado para realizar cálculos no interpretador, sed é um editor que funciona em linha de comando, utizado aqui para transformar um real em inteiro
```

Validação em arquivos
```
if [ -b arquivo ]; then || É um dispositivo de bloco
if [ -c arquivo ]; then || É um dispositivo de caractere
if [ -d arquivo ]; then || É um diretório
if [ -e arquivo ]; then || O arquivo existe
if [ -f arquivo ]; then || É um arquivo normal
if [ -g arquivo ]; then || O bit SGID está ativado
if [ -G arquivo ]; then || O grupo do arquivo é o do usuário atual
if [ -k arquivo ]; then || O sticky-bit está ativado
if [ -L arquivo ]; then || O arquivo é um link simbólico
if [ -O arquivo ]; then || O dono do arquivo é o usuário atual
if [ -p arquivo ]; then || O arquivo é um named pipe
if [ -r arquivo ]; then || O arquivo tem permissão de leitura
if [ -s arquivo ]; then || O tamanho do arquivo é maior que zero
if [ -S arquivo ]; then || O arquivo é um socket
if [ -t arquivo ]; then || O descritor de arquivos N é um terminal
if [ -u arquivo ]; then || O bit SUID está ativado
if [ -w arquivo ]; then || O arquivo tem permissão de escrita
if [ -x arquivo ]; then || O arquivo tem permissão de execução
if [ -nt arquivo ]; then || O arquivo é mais recente (NewerThan)
if [ -ot arquivo ]; then || O arquivo é mais antigo (OlderThan)
if [ -ef arquivo ]; then || O arquivo é o mesmo (EqualFile)
```

Exemplo com validação em arquivos
```
read -p "Nome do arquivo: " arq; echo
if [ -z $arq ]; then
  echo "Arquivo não informado"
else
  if [ -e $arq ]; then
    if [ -r $arq ]; then
      echo "O arquivo tem permissão"
    else
      echo "O arquivo não tem permissão"
    fi
  else
    echo "O arquivo não existe nesse diretorio"
  fi
fi
```

For - laço de repetição
```
echo "TABUADA"; echo
read -p "Digite um número: " valor; echo
for (( i=1; i<=10; i++ )); do
  echo "$valor x $i = $[valor*i]"
done
echo

read -p "Digite o diretorio" dir; echo
if [ -d $dir ]; then
  contador=0;
  for i in `ls $dir`; do
    contador=$((contador+1))
  done
  if [ "$contador" -eq 0 ]; then
    echo "Nada"
  elif [ "$contador" -eq 1 ]; then
    echo "Apenas 1"
  else
    echo "Encontrados $contador arquivos"
  fi
else
  echo "Não existe
fi
echo
```

Cut
```
abcdef

cut -c3 || extrair a coluna de número X de uma string -> c
cut -c3-5 || extrair coluna 3 a 5 da string -> cde
cut -c3- || extrair coluna 3 até o fim -> cdef
cut -c-3 || extrair todas colunas até a coluna 3 -> abc
cut -d: -f1 /etc/passwd || extrair apenas o primeiro campo do etc/passwd, verifica o delimitador como :
date | cut -d: -f1 || retorna a data até o primeiro :
```
## Links úteis

GREP
https://www.hostinger.com.br/tutoriais/comando-grep-linux/
SED
http://rberaldo.com.br/o-comando-sed-do-linux/
https://terminalroot.com.br/2015/07/30-exemplos-do-comando-sed-com-regex.html
TR
http://www.bosontreinamentos.com.br/linux/comando-tr-substituir-e-excluir-caracteres-em-arquivos-no-linux/
UNIQ + SORT
https://pt.wikibooks.org/wiki/Guia_do_Linux/Iniciante%2BIntermedi%C3%A1rio/Comandos_diversos/uniq
https://www.dicas-l.com.br/arquivo/o_comando_sort_e_seus_muitos_recursos.php
PASTE
https://www.geeksforgeeks.org/paste-command-in-linux-with-examples/

## Sumário Gabaritos

* [Criação de usuários membros e grupos](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_07.doc)
* [Acessos, comentários e permissões](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_08.doc)
* [Script 1 - conversão segundos -> minutos+segundos | Script 2 - Autenticação no Linux](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_09.doc)
* [Script - menu + manipulação de usuário](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_10.doc)
* [Script - input caminho e nome do arquivo, inclusão de XvariavelX em cada palavra do arquivo](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_11.doc)
* [Script - verificação passwd e exibir em tela usuários com id 500+](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_12.doc)
* [Script - input diretório e extensão do arquivo, substituir - por _](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_13.doc)
* [Script - input diretório, retornar ls diversos tipos](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_15.doc)

## Autor

* **Renan Pereira**
