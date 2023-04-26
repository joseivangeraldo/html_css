FROM httpd:2.4
COPY ./website/ /usr/local/apache2/htdocs/

docker build .
 docker build -t apache-docker .   --> coloca uma tag para melhot identificação
 
--------------- RODAR IMAGEM EM UM CONTAINER ----------------------
docker run -dit --name NOME_BATISMO_CONTAINER -p 80:80 NOME_IMAGEM
docker stop ID_CONTAINER
docker start ID_CONTAINER   -----  docker start 8b978b41740a

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

*italicized text*

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

| Syntax | Description |
| ----------- | ----------- |
| Header | Title |
| Paragraph | Text |

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
