# **Iniciando com Git**

## 1) Configurações básicas do Git

```
git config --global user.name "Nome Usuario"

git config --global user.email "nome.sobrenome@email.com"

git config --global core.editor "sublime"

git config --global color.ui "true"

git config --list
```
* **System**
  * Configuração Global utilizada por todo o PC.
  * /etc/gitconfig
  * `git config --system`
* **User**
  * Single User Configuration
  * ~/.gitconfig
  * `git config --global`
* **Project**
  * Configurações são aplicadas somente a um projeto especifico.
  * meu_projeto/.git/config
  * `git config`

1) `cat ~/.gitconfig` - Show de Configurations

## 2) AutoComplete no Git

1) No diretório $HOME digite o seguinte comando: `curl -OL https://github.com/git/git/raw/master/contrib/completion/git-completion.bash`

2) Renomeie o arquivo para oculta-lo: `mv ~/git-completion.bash ~/.git-completion.bash`

3) Adicione o seguinte código no seu arquivo **.bash_profile** localizado no diretório $HOME:

```
Editar o .bash_profile:
if [ -f ~/.git-completion.bash ]; then
	source ~/.git-completion.bash
fi
```

## 3) Iniciando um repositório

1) Crie uma nova pasta (O qual será o seu projeto).

2) Navegue até o diretório onde está o projeto (**Pelo terminal**).

3) E dentro da pasta digite o seguinte comando:  `git init`

4) A seguinte saida será exibida no console: **Initialized empty Git repository in /$HOME/SeuProjeto/.git/**

## 4) Criando nosso primeiro commit

1. Crie um arquivo dentro do diretório do seu projeto: **arquivo.txt**.

2. Digite o seguinte comando: `git add .` para adicionar a mudança ao **staging index**. (O ponto diz para juntar todas as mudanças feitas no **working directory**)

3. Em seguinda digite: `git commit -m "Mesagem do Commit"` para comitar a mudança.

#### 4.1) Visualizando os logs

1) Para ver os commits digite: `git log`

2) Para ver os commits **pelo autor**: `git log --author="nameOfAuthor"`

3) Para ver os commits **por data** desde: `git log --since=yyyy-mm-dd`

4) Usando global **regular expression**: `git log --grep="Texto a ser procurado nos commits"`

## 5) Arquitetura Git 3 camadas
  
* **Repository**

  * *change set commited* (cada change set tem um ID que é um SHA)
 
* **Staging Index**

  * `git commit -m "Mensagem do Commit"`

* **Working Directory**

  * `git add/rm file1, file2, file3...`



#### 5.1) Ponteiro para o HEAD

1) *HEAD* é uma **variavel de referencia** mantida pelo Git

2) Sempre aponta para o ultimo commit do branch atual(checkout branch)

3) É tido como o ultimo estado do repositorio

## 6) Algums Comandos de uso Frequente

* `git status` - Vai nos dizer se há diferença entre: working directory, staging index e repository

* `git add <file>` - Adiciona os arquivos que estão no working directory para o staging index

* `git commit -m <message>` - *"Snapshot"* dos arquivos para o repositório

* `git commit -am <message>` - Commita todas as mudanças de uma só vez "sem passar" pelo stage index (**new files and delete files não funciona, eh interessante para modifications files**)

* `git diff <file>` - Compara as mudanças feitas nos arquivos que estao no *Working Directory* **VS** *Repository* onde o ultimo HEAD aponta

* `git diff --color-words <file>` - Mostra as mudanças lado-a-lado de forma colorida

* `git diff --staged <file>` - Visualizando as mudanças feitas no *Staging Index* em comparação com o *Repository*

* `git rm <file>` - apaga os arquivos que estão no working directory e prepara o staging index com as mudanças para o commit

* `git mv <old-name> <new-name>` - Renomeia/*MOve* o arquivo para outro nome/*Lugar*

## 7) Desfazendo Mudanças

1) Buscando a versão do Repository para desfazer as mudanças/edições feitas em um arquivo no working directory. Utiliza-se o **--** para dizer que é do branch atual que estamos resgatando o arquivo.
    * `git checkout -- <file>`

2) Resgatando o arquivo do Staging Index para o Working Directory (Reset o HEAD para que fique igual ao do ultimo commit)
    * `git reset HEAD <file>`

3) "Editando" o ultimo commit e adicionando mudanças ao ultimo OBJETO(commit) no qual o HEAD aponta.

    * `git add <file>`

    * `git commit --amend -m <message>`


4) Mudando a mensagem do Ultimo Commit
  
    * `git commit --amend -m <message>`

#### 5) Trazendo de volta uma versão especifica DE UM ARQUIVO DE UM COMMIT

  1) Copie parte do SHA do commit que vc quer buscar a versão do arquivo. (Usando o `git log`)

  2) Em seguida utilize o SHA para buscar o arquivo em especifico:
    
      * `git checkout <SHA do Commit> -- <file>`

  3) O Arquivo Vai Para o **Stage Index**

  4) Veja as diferenças do arquivo para saber se eh a versão antiga:

      * `git diff --staged`

  5) Trazendo o arquivo do Staging Index para o Wokring Directory

        * `git reset HEAD <file>`

  6) Se desajar manter a versão atual e DESFAZER a mudança feita no Working Directory digite:

      * `git checkout -- <file>`


#### 7.1) REVERTENDO todas as mudanças feitas nos arquivos contidos em um commit

* COPIE parte do SHA do commit que vc quer reverter

* E digite o seguinte comando:

* `git revert <SHA>` ou 

* `git revert -n <SHA>` - Não vai commitar automaticamente, vai esperar vc fazer isso.

* PAra reverter o revert digite:

* Use `git revert --abort` to cancel the revert operation


#### 7.2) Usando Git RESET para apontar o HEAD para qualquer commit que vc quiser

* **SOFT**
  * > **Somente** Move o ponteiro para o commit desejado mas nao altera o STAGING INDEX ou o WORKING DIRECTORY  
  

* **MIXED**
  * > Altera o STAGING INDEX para ficar igual ao REPOSITORY (Onde o **HEAD** vai apontar para o COmmit)

  * > Não altera o WORKING DIRECTORY

* **HARD**
  * > **Destrutivamente** altera o STAGING INDEX e o WORKING DIRECTORY para ficar igual ao REPOSITORY(Para onde o **HEAD** aponta no commit)


#### 7.3) Git reset SOFT

* `git reset --soft <SHA>` - **Comando Principal**

* *Sempre que for fazer um git reset é aconselhavel copiar os ultimos commits em um arquivo texto.*

* Para saber onde o HEAD está apontando por ultimo:
  * `cat .git/HEAD`

* Vc pode commitar as mudanças e continuar a *gravar* dali pra frente ou pode usar o reset para voltar ao ultimo commit.



#### 7.4) Git reset MIXED

* `git reset --mixed <SHA>` - **Comando Principal**

* Faz a mesma coisa que o SOFT, porem o staging index fica do mesmo jeito que esta no **Repository**

* Sempre copie os 5 ultimos commits num arquivo texto com o comando `git log` antes de executar as operações de RESET

* Os arquivos voltam para o working directory


#### 7.5) Git reset HARD

* `git reset --hard <SHA>` - **Comando Principal**

* DESTROI TUDO, e deixa o Staging Index e o Working Directory IGUAL ao REPOSITORY (PARA ONDE O HEAD DO COMMIT FOI APONTADO)

* Começa a gravar dali(ONDE O HEAD FOI APONTADO) pra frente

* Digitando `git status` mostra que o working directory e o staging index estao vazios !!!


#### 7.6) Deixando de trackEAR arquivos do Working Directory** - (New Files in Working Directory)

1) Jogando arquivos **NO LIXO**(Apartir do Working Directory)

    * `git clean -n` - Don't actually remove anything, just show what would be done.

    * `git clean -f` - Aqui o bixo pega e remove para sempre !!!

## 8) Ignorando arquivos no Working Directory

1) Para dizer ao Git quais arquivos queremos que ele não TRAckei No working directory devemos criar o seguinte arquivo na raiz do nosso projeto:
    *  `.gitignore`

2) Ignorando alguns arquivos com expressões regulares:

    * `.java` - Negue qualquer arquivo com a extensao .java

    * `!main.java` - Menos o arquivo explicitado com exclamação

    * `package/tests/` - IGNORA todos os arquivos do diretorio com o ultimo slash **/**

3) Sites relevantes:

    * [Ignorando Arquivos](https://help.github.com/articles/ignoring-files)

    * [GitIgnore](https://github.com/github/gitignore)

#### 8.1) Ignorando arquivos Globalmente 

1) Vai ignorar os arquivos selecionado em todos os projetos Git do Computador

2) Eh settado no nas Configurações do GIT

3) Primeiro crie o arquivo `.gitignore_global` no seu diretorio de usuario

4) Depois de criado adicione-o nas configurações globais utilizando o seguinte commando:

    * `git config --global core.excludesfile .gitignore_global`

5) Verifique o sucesso do comando com o seguinte:

    * `cat /Users/Usuario/.gitconfig`

#### 8.2) Ignorando arquivos que já foram TRAckeaDoS

  1) Adicione um arquivo ao Repository para deixa-lo trackeado pelo git

  2) Agora vamos ignorar edições futuras neste arquivo com o seguinte passo:

      * `git rm --cached <file>` -- Ainda vai continuar uma copia No **Working Directory** e no **Repository**, menos no **Staging Index**

## 9) Navegando pelos COMMITS DO Git

1) Referenciando um commit PAI usando o Chapéu do Vovo:

    * `HEADˆ`, `ad3da3ˆ`, `masterˆ`

    * `HEAD~1 `, `HEAD~`

2) Referenciando o commit PAI DO PAI, ou seja, *O VOVOZÃO*:

    * `HEADˆˆ` `ad3da3ˆˆ` `masterˆˆ`

3) Referenciando mais pra **Baixo** ainda, vai **Descendo** uns degraus a mais:

    * `HEAD~3` - `HEADˆˆˆ` `ad3da3ˆˆˆ` `masterˆˆˆ`

#### 9.1 Listando o conteudo de um Objeto da Árvore

  * `git ls-tree <tree-ish>`

  * `git ls-tree HEAD` - lista o conteudo de um objeto da arvore (HEAD = current checkout Branch)

#### 9.2 Git Log Commands with more Options

* `git log --oneline` - Uma versão reduzida do comando `git log`

* `git log --oneline -5` - Mostra os 5 ultimos commits

* `git log --since="yyyy-mm-dd"`

* `git log --until="yyyy-mm-dd"`

* `git log --since="3 weeks ago" --until="1 days ago"`

* `git log --author="name"`

* `git log --grep="pattern"` - Global Regular Expression Search

* `git log <SHA1>..<SHA2> --oneline` - Show me the range between one SHA and another

* `git log <SHA>.. <file>` - Tell me what happens with the file since the sHA até o ultimo Commit(**Gives me the logs that affect that `<file>`**)

* `git log -p` - Mostra as mudanças nos arquivos utilizando o Diff

* `git log -p <SHA>.. <file>` - AS mudanças feitas no <file> desde o inicio do <SHA> (-p == Patch)

* `git log --stat --summary` - REsumão Estatistico

* `git log --format=oneline` - Formata tudo numa linha, exibindo o full SHA

* `git log --graph` - Mostra os commits em formato de linha de grafico

* `git log --oneline --graph --all --decorate`

* `git show <SHA>` - **Examine the commit deeply** com Diff/Comparações

* `git show --format=oneline HEAD`

* Navegando nos commits profundamente:
  ---

    * `git ls-tree [master]|[SHA]` - Vai listar todos os arquivos/diretorios contidos no Branch Atual

    * `git show <SHA>` - Utiliza o SHA de um arquivo/diretorio especifico (BLOB, TREE, TAGS, COMMITS) **para ver seu conteudo**

#### 9.3 Comparando dois commits diferentes

1) `git log --oneline` - Para pegar o SHA do commit a ser comparado.

2) `git diff <SHA>` - Vai mostrar todas as mudanças feitas naquele commit até os dias atuais(ultimo commit).

3) `git diff <SHA> <file>` - Sendo mais especifico e escolhendo um arquivo para visualizar suas mudanças no tempo.

4) `git diff <SHA1>..<SHA2>` - Utilizando o range para comparar 2 commits.

5) `git diff <SHA1>..<SHA2> <file>` - Sendo mais especifico e escolhendo um arquivo na *linha do tempo* SHA1...SHA2

6) `git diff --stat --summary <SHA1>..<SHA2>` - Opções com estatisticas

7) `git diff --ignore-space-change <SHA1>..<SHA2>` - Ignora espaços em branco feito nas modificações dos arquivos.

8) `git diff [-b] | [-w] <SHA1>..<SHA2>` - B ignora espaços em branco e o W ignora todos os espaços.

## 10) Branches

* Tentar novas ideias
* Ao inves de usar o branch master para commitar um monte de porcarias, usa-se o recurso de Braches para alterações e ver se vai funcionar ou nao
* Se funcionar as alterações feitas no Branch são MErgeadas com o Branch MASTER.
* O HEAD só vai apontar para o novo branch quando novos commits foram feitos, caso contrario ele ainda estara no Master

```
Branch Master - r4f2sd ---> fj32od2 ---> 224k3h ---> b4t2fo ---> ad3h4n ---> 3nv4op ---> HEAD~
                                                                                /
                                                                               /
Alternative Branch -                                             dn34f45  --->     MERGING
```

* `git branch` - Listar todos os branches

* `git branch <nome_do_branch>` - Criar um novo branch
    * As alterações/Commits feitas no branch de origem são trazidas para o **novo** branch a ser criado
    * O Working Directory precisa estar "LIMPO"(**Sem Conflito**, new files são permitidos pois nao tem conflito com nada) antes do switching.

* `git checkout <nome_do_branch>` - Trocando de branch

* `git checkout -b <nome_do_branch>` - Criando um branch e usando-o num só commando

* `git diff <branch_1>..<branch_2>` - Compara 2 branches

* `git diff --color-words <branch_1>..<branch_2>` - Compara os 2 branches mostrando o resultado em UMA linha.

* `git branch --merged` - Mostra os branches que foram **mergeados/incluidos** com o branch checkouted.
    * Todos os commits feitos no branch de origem estao no branch atual.

* `git branch -m <old_name_branch> <new_name_branch>` - Renomeia o nome do branch.(-m stands for Move)

* `git branch -d <name_of_branch>` - Deletando um branch
    * Não eh possivel deletar o branch no qual vc se encontra, vc tem que sair dele primeiro.
    * Se houver commits em um branch e vc tentar deletar ele sem **mergea-lo** com o seu branch de origem, um erro irá aparecer na tela.

* `git branch -D <name_of_branch>` - MAS se vc quer realmente exlcuir o branch, vá em frente com a opção -D

## 11) Merging

* Antes de começar o merging, deve ser feito **o checkout para o branch** que irá receber as atualizações.

* `git merge <branch_com_as_atualizações>`

* Fast-Forward merge - Signigica que não há conflitos e que nada foi commitado no master e por isso o branch mergeia de forma tranquila com o master.

* `git merge --no-ff <branch_com_as_atualizações>` - Força a realização de um Commit de Verdade no MAster, ao inves de fazer um FAst-Forward que é só um apontamento do HEAD para o ultimo commit do master.

* `git merge --ff-only <branch_com_as_atualizações>` - Faça o merge somente se vc conseguir fazer um FASt-fOrWard, caso contrario aborte a operação.

#### 11.1 Resolvendo Conflitos

1) Voce pode abortar o Merge
    * `git merge --abort`

2) Voce pode resolver os conflitos manualmente(Eh o que vc mais vai fazer)
    * Após resolver os conflitos:
    * `git add <file>` - Para colocar o arquivo no Staging Index e 
    * Digite: `git commit` e uma mensagem padrão de commit de merge será adicionada

3) Voce pode usar uma ferramenta bonita para resolver o merge
    * `git mergetool --tool=<nome-do-programa>`


## 12) Stashing as Mudanças

* Quando vc quiser esconder uma coisa e não quer que ela esteja visivel no working directory **Temporariamente**(até vc decidir traze-las de volta) vc as coloca no *Stash*

* Quando vc esta no branch filho com um arquivo **modificado** no working directory e retorna para o master sem resolver a pendencia com o arquivo, a seguinte mensagem eh exibida:

    > error: Your local changes to the following files would be overwritten by checkout: `<file>`.
        Please commit your changes or stash them before you switch branches.
        Aborting.

* Tudo que vc salva no stash fica visivel nos demais branches, nao somente no branch onde vc fez o stash save

* `git stash save "<message>"` - Salva as modificações em um lugar sombrio até vc decidir o que fazer com elas.

* `git stash push "<message>"` - Esse eh o novo modelo !

* `git stash list` - Lista todas as coisas escondidas no Stash

* `git stash show stash@{0}` - Mostra um resumo bem fraco do que foi alterado

* `git stash show -p stash@{0}` - **p** stands for patch and show you more details on code edits.

* `git stash pop stash@{number}` - Traz a mudança de volta e apaga o que estava la no esconderijo do stash

* `git stash apply stash@{number}` - Traz a mudança de volta e **Não** apaga o que esta escondido no stash

* `git stash drop stash@{number}`  - Apaga o conteudo que ficou no escondido lah no stash

* `git stash clear` - TOtalmente destrutivo, limpa por completo tudo que esta escondido no stash

## 13) Repositórios Remotos

1) **origin/master** - Branch criado pelo Git na nossa maquina local que referencia o branch do servidor remoto
2) **push** - Empurra todos os commits feitos na maquina local para o repositorio remoto
3) **fetch** - Busca atualizações feitas no servidor remoto e traz para o nosso repositorio local, atualizando primeiramente o **orgin/master**.
4) **merge** - Após a sincronização do **remote server** com o **origin/master** a atualização esta na nossa maquina mas nao no nosso **branch master local**, para isso eh preciso fazer um *merge*.

_______

* **Vc precisa estar no diretorio raiz do projeto para executar os comandos a seguir**.

* `git remote` - Shows us all the remotes repo like git branch command

* `git remote add <alias> <url>` - O alias geralmente é nomeado como *origin* mas vc pode colocar qlqr nome, ja o url vc pega no repositorio criado no github.

* `git remote -v` - Mostra mais infos.

* `cat .git/config` - Mostra info do branch remoto.

* `git remote rm <alias>` - Remove/Deleta o link com o repositorio remoto.

* `git push -u <alias_remote_branch> <local_branch>` - Empurra(push) nosso codigo no ***local_branch*** para o repositorio remoto(**GERALMENTE CHAMADO DE ORIGIN**).
    * **Branch 'master' set up to track remote branch 'master' from 'origin'**.

    * **Quando o branch já estiver Trackeado, não há mais necessidade de digitar -u, apenas digite**: `git push` **para enviar os commits para o servidor remoto.**

* `git branch -r` - Mostra os branches remotos.

* `git branch -a` - Mostra os branches remotos e locais.

* **Fetch** eh o que sincroniza o origin/master com o que esta no Repositorio Remoto
    * origin/master não reflete o que esta no Repositorio Remoto, precisamos fazer um Sync
    * `git fetch origin` - Vai até o Github e baixa tudo para sincronizar com o local.
    * origin = aponta/apelido para o Github como demostrado no **.git/config** file.

    * Se vc fizer um `git log` depois do fetch, ele soh atualizou o origin/master, para atualizar o master local eh preciso fazer um **MERGE** (Lembre da figura - **remote branch -> origin/master -> master**).
    * `git merge origin/master`
    * `git pull` = (`git fetch` + `git merge`) - Faz os dois comandos de uma vez só.

#### Colaborando com um projeto Open Source

1) Escolha o projeto no github e copie o caminho para o repositorio do projeto.

2) Em seguida, digite: `git clone <url_do_projeto> | <nome-da-pasta>`

3) Um diretorio será criado com o mesmo nome do projeto que esta lah no github
____
* Voce vai escolher o projeto que quer "trabalhar"/"commitar" na página do Github

* Vai até o botão de **Fork** para vc ter uma Cópia do projeto na sua conta pessoal

* Após as suas modificações/melhorias no projeto, vc retorna a pagina original do projeto e vai no botão **Pull Request**

#### Trackeando um branch remoto Não Trackeado

* SE vc utilizou o comando `git push` sem o **-u** option, o branch nao será trackeado com o que esta remotamente no github.

* Para votlar a trackear digite: `git branch --set-upstream <nome_do_branch> origin/<nome_do_branch>` 

#### Guidelines git fetch

* Sempre comece o dia usando o `git fetch` antes de iniciar qualquer trabalho.
* Sempre use o `git fetch` antes do `git push`
* Fetch Sempre Sempre Sempre, eh de graça

#### Como CheckoutEaR Branchs Remotos

* `git branch -r` - Para ver os branchs remotos

* `git branch <criando-novo-nome-do-branch> <origin/nome-do-branch-remoto>` - Os nomes podem ser os mesmos.
    * **Branch `<new-name>` set up to track remote branch `<remote-name>` from origin**

* `git branch -d <new-name>` - Deleta o branch recem criado com base no branch remoto.

* `git checkout -b <criando-novo-nome-do-branch> <origin/nome-do-branch-remoto>` - Faz tudo num comando só

### Algumas notas com relação a pushing para um branch remoto com muitas atualizações !!!

1) O git nunca tenta fazer um **merge** durante uma operação de **push** onde o branch remoto possui muitas atualizações
2) Você precisa fazer um **fetch** com as mudanças que estão no servidor remoto
3) Depois disso vc faz um **merge** das mudanças locais com o **origin/master** e cria um novo **merge commit** 
4) DAi sim eh possivel fazer o **push** para o servidor remoto com as atualizações

#### Deletando um Branch Remoto

1) `git push origin :<nome-do-branch-remoto>` - Push to origin(REMOTE) 'NADA/VAzio/NuLo' para o branch `<nome-do-branch-remoto>`.

2) `git push origin --delete <nome-do-branch-remoto>` - Mais intuitvo do que o de cima

## 14) General Tips and Tricks(stay tunned)

* Alguns comandos repetitivos/verbosos podem ser customizados criando-se um **alias**

* Para criar um alias a nível de usuário `cat ~/.gitconfig` usa-se o seguinte commando:
    * `git config --global alias.<apelido-para-o-comando> <comando-propriamente-dito>`
    * Se o comando do git contem espaços então será necessário coloca-lo entre "aspas"
    * **Ex:** `git config --global alias.logg "log --grap --decorate --all -oneline --abbrev-commit" `

* Gerando chaves SSH