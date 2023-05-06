<a name="ancora"></a>
# Indice 
- [Tecnologia](#tecnologia)
- [Instação do Ambiente](#instalacao)
- [Acrescentar editar arquivos](#acrescentareditar)

### <a id="tecnologia"> 🛠 Tecnologias</a>

As seguintes ferramentas foram usadas na construção do projeto:

- [Apache](https://httpd.apache.org/)
- [CSS](https://www.w3.org/Style/CSS/Overview.en.html/)
- [Docker](https://www.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [HTML](https://www.w3.org/html/)
- [Visual Studio](https://expo.io](https://code.visualstudio.com/docs)/)
>[Topo](#ancora)

## <a id="instalacao">🔨 Instalação do Ambiente</a>
 Instalação Imagem Docker, para programar páginas diretamente em um servidor Web.
Obs: Estes exemplos foram formulados totalmente em um ambiente computacional dentro da nuvem. Particulamente no codespace do Github e Gitpod. Na atualidade são as melhores plataformas para isto, balanceando os custos e beneficios. Se você não conhece ou não sabe como acessa-los, [clique aqui.](https://docs.github.com/en/codespaces/developing-in-codespaces/opening-an-existing-codespace/)

No codespace abra um terminal ou teclas use <kbd>Ctrl</kbd> + <kbd>'</kbd> 
Vamos ver antes se existem imagens criadas, só para verificar
```
$ docker images
```
Se não tiver nenhuma imagem docker será mostrado uma tabela vazia:
```
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

Após crie a pasta de trabalho:
```
$ mkdir ServerApache
```
O nome do diretório pode ser este ou qualquer outro de sua preferência.

Depois entre dentro deste diretório:
```
$ cd ServerApache
```
Criar uma página chamada index.html básica dentro deste diretório, para verificar que ocorreu uma instalação perfeita.
Segue um modelo básico abaixo:
```
<html>
    <head> </head>
    <body>
        <H1> 
            A Pagina Inicial Funcionou
        </H1>
    </body>
</html>
```

Crie um arquivo Dockerfile. O Codespace GitHub e Gitpod já vem com Visual Studio configurado.
Tem de ter este nome 'Dockerfile' sem nenhuma extensão, assim digite:
```
$ code Dockerfile
```

Este Dockerfile, que orquestrará todas as dependencias do ambiente para a rodar a imagem Docker.
Seque a sintaxe do modelo:
```
 FROM httpd:2.4  
 COPY ./ /usr/local/apache2/htdocs/ 
 RUN ["apt-get", "update"]  
 RUN ["apt-get", "install", "-y", "vim"]
 ```
 Em COPY , copiamos tudo que esta na pasta e coloca na pasta aonde são publicada as paginas web do container.
 Nas duas ultimas linhas estamos atualizando a distribuição Linux, em seguida instalando o editor VIM, caso necessitar de editar algum código no Shell.

Montar a Imagem, importante estar dentro do diretório que foi criado, e o Dockerfile estar dentro do mesmo diretório:
```
docker build -t apache-docker .
```
Detalhe o sinal de ponto, ao final, informar para pegar tudo que esta dentro do diretório.
É colocado uma tag de identificação para melhot localização, com -t , pode ser o nome de sua preferência. 

Será mostrado mensagens da evolução do processo como abaixo:
```
[+] Building 1.1s (10/10) FINISHED                                                                                                         
 => [internal] load build definition from Dockerfile                                                                                  0.2s
 => => transferring dockerfile: 37B                                                                                                   0.0s
 => [internal] load .dockerignore                                                                                                     0.3s
 => => transferring context: 2B                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/httpd:2.4                                                                          0.3s
 => [auth] library/httpd:pull token for registry-1.docker.io                                                                          0.0s
 => [internal] load build context                                                                                                     0.1s
 => => transferring context: 60B                                                                                                      0.0s
 => [1/4] FROM docker.io/library/httpd:2.4@sha256:a182ef2350699f04b8f8e736747104eb273e255e818cd55b6d7aa50a1490ed0c                    0.0s
 => CACHED [2/4] COPY ./ /usr/local/apache2/htdocs/                                                                                   0.0s
 => CACHED [3/4] RUN ["apt-get", "update"]                                                                                            0.0s
 => CACHED [4/4] RUN ["apt-get", "install", "-y", "vim"]                                                                              0.0s
 => exporting to image                                                                                                                0.2s
 => => exporting layers                                                                                                               0.0s
 => => writing image sha256:d0b5d542ef584608a2b54597c2a3b85dd5ab06814ab7b349f38d2b0cf279b61d                                          0.0s
 => => naming to docker.io/library/apache-docker                                           0.0s
```
Verificar se existe a imagem no sistema:
```
$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
apache-docker   latest    d0b5d542ef58   16 minutes ago   199MB
```
Neste momento já temos a nossa imagem no sistema, precisamos agora rodar ela em um Docker container.
Verificar agora se existe algum contâiner rodando:
```
$ docker ps
```
Se não existir nenhum container rodando, vai ser retornado uma tabela vazia:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
Agora vamos colocar para rodar um container pegando como argumento o ID da imagem Docker,
como podemos conferir pelo comando : docker images, feito acima:
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
apache-docker   latest    d0b5d542ef58   16 minutes ago   199MB
Pegamos a coluna IMAGE ID , como argumento para o comando abaixo:
docker run -dit --name NOME_BATISMO_CONTAINER -p 80:80 IMAGE ID 
```
$ docker run -dit --name apache-docker -p 80:80 d0b5d542ef58
```
Vai ser montado na porta 80 local, comunicando com a porta 80 do container.

Após isto será exibido a mensagem que a porta 80 está sendo acionada.  <br/> 
![Imagem Port open codespace github](/images/Port_open_codespace.jpg)  <br/>

Ao clicar ou seguir a porta 80 será exibida pagina index.html, que foi criada anteriormente.  <br/>
![Imagem Pagina Index.html](/images/pagina_funcionou.jpg)  <br/>
[Topo](#ancora)


<a id="acrescentareditar"></a>
## 📁 Acrescentar e Editar arquivos

> Para acrescentar e editar arquivos no servidor, precisamos entrar no docker container.
> Vamos acessar ele através de seu bash.Para isto utilizamos o comando 'exec' do docker.
```
 $ docker exec -it 1e936a216edd bash
```
Vai aparecer a seguir a linha de comando bash do container docker.
**O simbolo '#' é o seu status de usuario, que indica ser root, o que terá de ser digitado é somente o texto que está após este sinal.
Se existir conteúdo abaixo do console é somente a visualização da saida do comando.**
```
root@1e936a216edd:/usr/local/apache2#
```
Vamos só para visualizar o conjunto. Listar os arquivos:
```
root@1e936a216edd:/usr/local/apache2# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs  modules
```
Abrir a pasta publica e a seguir listar os arquivos desta pasta.Vamos encontrar os arquivos que criamos da pasta da maquina local.
```
root@1e936a216edd:/usr/local/apache2# cd htdocs
root@1e936a216edd:/usr/local/apache2/htdocs# ls
Dockerfile  index.html
```

Agora vamos entrar no arquivo que queremos editar, ou criar um novo, aí fica conforme a demanda de seu projeto ou  trabalho.
Para isso vamos usar o VIM, que é um dos primeiros editores Shell nativo. Explicaremos aqui comandos básicos para editar e salvar seus documentos.Se quiser buscar [informações completas](https://www.vim.org/).  </br>

Ao abrir o VIM parece uma coisa meio estranha, pois é  um editor feito para ambiente de linha de comando, e os usuarios acostumados com editores graficos ficam bem confusos. Ao iniciar o VIM aguarda oque o usuário deseja fazer, pode ser edição,  visualização ou navegação. Estes comandos são  divididos em comandos de exibição, edição e navegação.
Vamos editar agora o nosso arquivo index.html, para isto digite:
```
root@1e936a216edd:/usr/local/apache2/htdocs#  vi index.html
```

Então o arquivo index.html é exibido, assim:

```
colocar imagem aqui
```
Repare no rodapé,  é nesta área da interface do programa que precisamos prestar atenção, aí é mostrado qual a ação que estamos fazendo.
 Vamos iniciar a edição. A tecla esc funciona como uma baliza, em uma analogia mais simplório é como fosse o ponto neutro do câmbio de um carro, mais para frente vai ficar claro esta comparação.

aperte a tecla esc, para ter certeza que está no menu básico. Em seguida aperte a tecla 'e'. No rodapé do editor aparecerá a palavra INSERT, significando que está  no modo de edição.
root@1e936a216edd:/usr/local/apache2/htdocs# 

> [Topo](#ancora)

## Mizukage

> Loren Ipsum
> Donec nec mattis dui, quis sagittis magna. Praesent in sollicitudin erat, non molestie velit. Nam tempor metus et laoreet sodales. Sed eu mauris odio. Maecenas at feugiat mi. Nam venenatis accumsan mi, in dictum nisl. Phasellus laoreet nec sem at volutpat.
> [Topo](#ancora)
> <a id="ancora4"></a>

## Raikage

> Loren Ipsum
> Donec nec mattis dui, quis sagittis magna. Praesent in sollicitudin erat, non molestie velit. Nam tempor metus et laoreet sodales. Sed eu mauris odio. Maecenas at feugiat mi. Nam venenatis accumsan mi, in dictum nisl. Phasellus laoreet nec sem at volutpat.
> [Topo](#ancora)
> <a id="ancora5"></a>

## Tsuchikage

> Loren Ipsum
> Donec nec mattis dui, quis sagittis magna. Praesent in sollicitudin erat, non molestie velit. Nam tempor metus et laoreet sodales. Sed eu mauris odio. Maecenas at feugiat mi. Nam venenatis accumsan mi, in dictum nisl. Phasellus laoreet nec sem at volutpat.
> [Topo](#ancora)

FROM httpd:2.4
COPY ./website/ /usr/local/apache2/htdocs/

docker build .
docker build -t apache-docker . --> coloca uma tag para melhot identificação

--------------- RODAR IMAGEM EM UM CONTAINER ----------------------
docker run -dit --name NOME_BATISMO_CONTAINER -p 80:80 NOME_IMAGEM
docker stop ID_CONTAINER
docker start ID_CONTAINER ----- docker start 8b978b41740a

Remover todas as imagens ao mesmo tempo
Para remover todas as imagens, há um comando que faz isso: docker rmi $(docker images -q)

Parar todos os contêineres em execução: docker stop $(docker ps -a -q)
Excluir todos os contêineres parados: docker rm $(docker ps -a -q)

git reset --hard origin/main

Our workflow is such. We have a branch called dev which I can reach at origin/dev. When we do changes, we create a branch off dev:

git checkout -b FixForBug origin/dev

First I pull down any changes from origin/dev and do a rebase:

git pull --rebase
Then I push the changes to a remote branch of the same name:

git push origin FixForBug
Now, there's a branch on the remote server and I can create a pull request for that change to be approved and merged back in to the dev branch. I don't ever push anything to origin/dev myself. I'm guessing this is as pretty common workflow.

The first time I do a git push, it works fine and creates the remote branch. However, if I push a second time (let's say during code-review, someone points out a problem), I get the following error:

error: failed to push some refs to 'https://github.mydomain.info/Product/product.git'
hint: Updates were rejected because the tip of your current branch is behind its remote counterpart. Integrate the remote changes (e.g. hint: 'git pull ...') before pushing again.
See the 'Note about fast-forwards' in 'git push --help' for details.

However, if I do a git status it says I'm ahead of origin/dev by 1 commit (which makes sense) and if I follow the hint and run git pull, it says everything is up to date. I think this is because I'm pushing to a different branch than my upstream branch. I can fix this issue by running:

git push -f origin FixForBug

In that case, it'll push the changes to the remote branch, saying (forced update) and everything appears to be good on the remote branch.

My Questions:

Why is -f required in this scenario? Usually when you're forcing something, it's because you were doing something wrong or at least against standard practice. Am I ok doing this, or will it mess up something in the remote branch or create a hassle for whoever has to eventually merge my stuff into dev?

None selected

Skip to content
Using Gmail with screen readers
1 of 18
Md file modelo
Inbox

## Basic Syntax

These are the elements outlined in John Gruber’s original design document. All Markdown applications support these elements.

### Heading

# H1

## H2

### H3

### Bold

**bold text**

### Italic

_italicized text_

### Blockquote

> blockquote

### Ordered List

1. First item
2. Second item
3. Third item

### Unordered List

- First item
- Second item
- Third item

### Code

`code`

### Horizontal Rule

---

### Link

[Markdown Guide](https://www.markdownguide.org)

### Image

![alt text](https://www.markdownguide.org/assets/images/tux.png)

## Extended Syntax

These elements extend the basic syntax by adding additional features. Not all Markdown applications support these elements.

### Table

| Syntax    | Description |
| --------- | ----------- |
| Header    | Title       |
| Paragraph | Text        |

### Fenced Code Block

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

### Footnote

Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.

### Heading ID

### My Great Heading {#custom-id}

### Definition List

term
: definition

### Strikethrough

~~The world is flat.~~

### Task List

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### Emoji

That is so funny! :joy:

(See also [Copying and Pasting Emoji](https://www.markdownguide.org/extended-syntax/#copying-and-pasting-emoji))

### Highlight

I need to highlight these ==very important words==.

### Subscript

H~2~O

### Superscript

X^2^
markdown-cheat-sheet.md
Displaying markdown-cheat-sheet.md.

Logo ou Banner
Status: Opcional

Se seu projeto já tem uma logo adicione no README. Pode ser um banner também. Você pode criar uma logo ou banner usando o Canva.

A logo ou banner podem substituir o título, mas não a descrição do projeto, veja um exemplo. O bom de manter o título e descrição em texto é que ajuda no SEO do Github, o Google vai ajudar indexar melhor seu projeto nas primeiras páginas de pesquisas, além de dar um resumo sobre o seu projeto.

Exemplos de projetos com logo e banner.

✅ Servidor Apache rodando em Docker container
Status: em desenvolvimento

Título: Server Apache Docker

Descrição: Montar umservidor Apache, totalmente configurtavel, dentro de um container docker. Plataforma de desenvolvimento Codespace Github.

<h1 align="center">Servidor Apache</h1>
Servidor Apache dentro de um container, que roda diretamente no codespace do Github, Gitpod, ou em maquinas fisicas de sua preferencia.

## Descrição do Projeto

<p align="center">Escrever uma breve descrição</p>
Escrever uma breve descrição

<h1 align="center">
    <a href="https://pt-br.reactjs.org/">🔗 React</a>
</h1>
<p align="center">🚀 Criação do Ambiente: </p>
🔗 React
🚀 lib para construir interfaces do usuário com componentes reutilizáveis

✅ Badges
Status: Opcional

É uma questão de gosto pessoal e comunicação. As badges são úteis para indicar o status do projeto: você pode colocar a versão dele, link para licença, quantidade de issues, status da build, status dos testes. Vale muito a pena colocar.

As badges podem ficar no topo antes do título ou abaixo da descrição.

Use o site shields.io para gerar suas badges.

https://blog.rocketseat.com.br/content/images/2020/07/como-escreve-um-bom-read-me-image-1.png
Você pode criar a sua própria badge:

Os ícones de várias logos de produtos e tecnologias você encontra aqui: simpleicons.org.

Podemos customizar partir da URL a abaixo:

https://img.shields.io/static/v1?label=<LABEL>&message=<MESSAGE>&color=<COLOR>&style=<STYLE>&logo=<LOGO>
Os parâmetros significam:

LABEL: texto do campo esquerdo
MESSAGE: texto do campo direto
COLOR: cor do campo direito, pode usar RGB.
STYLE: estilo do badge inteiro. Podemos ter: plastic, flat, for-the-badge, social ou flat-square. Teste cada uma delas.
LOGO: logo do campo esquerdo
Como exemplo, vou escolher os seguintes parâmetros:

LABEL: como Blog
MESSAGE: como Rocketseat
COLOR: 7159c1
STYLE: como for-the-badge
LOGO: como GHOST
Podemos colocar ele no README assim em HTML:

<img src="https://img.shields.io/static/v1?label=Blog&message=Rocketseat&color=7159c1&style=for-the-badge&logo=ghost"/>
ou ainda em Markdown:

![Badge](https://img.shields.io/badge/Blog-Rocketseat-%237159c1?style=for-the-badge&logo=ghost)
Pronto. Veja como ficou o badge personalizado:

Legal que no site shields.io tem o input search / project URL que você cola o link do projeto do seu Github e ele sugere alguns badges.

https://blog.rocketseat.com.br/content/images/2020/07/como-escreve-um-bom-read-me-shields.png
Tem esse repositório com algumas badges prontas: Naereen/badges

✅ Tabela de Conteúdos
Status: Obrigatório

É ótimo colocar os índices de conteúdos, que é tabela onde a pessoa clica e vai para o tópico específico.

Exemplo com markdown:

# Tabela de conteúdos

<!--ts-->

- [Sobre](#Sobre)
- [Tabela de Conteudo](#tabela-de-conteudo)
- [Instalação](#instalacao)
- [Como usar](#como-usar)
  - [Pre Requisitos](#pre-requisitos)
  - [Local files](#local-files)
  - [Remote files](#remote-files)
  - [Multiple files](#multiple-files)
  - [Combo](#combo)
- [Tests](#testes)
- [Tecnologias](#tecnologias)
  <!--te-->
  https://github.com/ekalinin/github-markdown-toc#table-of-contents
  Resultado:

Tabela de conteúdos
Sobre
Tabela de Conteudo
Instalação
Como usar
Pre Requisitos
Local files
Remote files
Multiple files
Combo
Tests
Tecnologias
Se o README tiver poucos tópicos pode fazer inline, com HTML:

<p align="center">
 <a href="#objetivo">Objetivo</a> •
 <a href="#roadmap">Roadmap</a> • 
 <a href="#tecnologias">Tecnologias</a> • 
 <a href="#contribuicao">Contribuição</a> • 
 <a href="#licenc-a">Licença</a> • 
 <a href="#autor">Autor</a>
</p>
https://github.com/animavita/animavita
Resultado:

Objetivo • Roadmap • Tecnologias • Contribuição • Licença • Autor

No README.md do Github você pode usar HTML o que ajuda muito. 👌

✅ Status do Projeto
Status: Obrigatório

Indica se o projeto está em desenvolvimento ou já foi concluído.

<h4 align="center"> 
	🚧  React Select 🚀 Em construção...  🚧
</h4>
Resultado:

🚀 Em construção... 🚧
✅ Features
Status: Funcionalidades
É uma facilidade você conseguir transportar todo o seu ambiente para onde quiser, sem incovenientes de ter de instalar tudo novamente, além do fato que normalmente nos deparamos com estções de trabalho que não são de nossa propriedade e fica muti incoveniente, instalar aplicativos novos.
Estou acrescentando sempre atualizações sempre melhorando o projeto..

Exemplo:

### Features

- [x] Cadastro de usuário
- [x] Cadastro de cliente
- [ ] Cadastro de produtos

No Github ou gist fica com essa aparência abaixo. Resultado:

https://blog.rocketseat.com.br/content/images/2020/07/Screen-Shot-2020-07-10-at-14.03.41.png
✅ Demonstração da aplicação
Status: Opcional

Se for um projeto web e estiver hospedado em algum lugar, forneça o link. Se o deploy foi feito no Netlify tem um badge para isso.

Se for uma API backend pode customizar um badge com um ícone do heroku. Pode também colocar o arquivo do Insomnia para ficar mais rápido para o usuário testar a API — Fica muito bom.

Se a aplicação estiver em desenvolvimento, se for um app mobile ou website coloque os prints da 

```bash
# Clone este repositório
$ git clone <https://github.com/tgmarinho/nlw1>
```

MIT License

Copyright (c) <2023> <José Ivan G Silva>


THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
