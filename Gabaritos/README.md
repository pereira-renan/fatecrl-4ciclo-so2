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

## Sumário Gabaritos

* [Criação de usuários membros e grupos](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_07.doc)
* [Acessos, comentários e permissões](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_08.doc)
* [Script 1 - conversão segundos -> minutos+segundos | Script 2 - Autenticação no Linux](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_09.doc)
* [Script - menu + manipulação de usuário](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_10.doc)
* [Script - input caminho e nome do arquivo, inclusão de XvariavelX em cada palavra do arquivo](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_11.doc)
* [Script - verificação passwd e exibir em tela usuários com id 500+](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_12.doc)
* [Script - input diretório e extensão do arquivo, substituir - por _](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_13.doc)
* [Script - input diretório, retornar ls diversos tipos](https://github.com/pereira-renan/fatecrl-4ciclo-so2/blob/master/Gabaritos/GabLista_15.doc)

## Authors

* **Renan Pereira**
