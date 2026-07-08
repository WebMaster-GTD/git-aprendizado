# Resumo Didatico de Git e GitHub

Este documento resume o caminho praticado neste projeto, do basico ao avancado.
Ele serve para consulta rapida, revisao e novos exercicios.

## 1. Ideia central do Git

Git registra a historia de um projeto.

Voce edita arquivos, escolhe o que quer salvar e cria commits.
Cada commit e como uma foto do projeto naquele momento.

Fluxo principal:

```bash
editar arquivos
git status
git diff
git add arquivo
git commit -m "Mensagem clara"
git push
```

## 2. Criacao do projeto

Comandos usados no inicio:

```bash
mkdir git-aprendizado
cd git-aprendizado
git init
touch README.md tarefas.md clientes.md anotacoes.md
mkdir docs
touch docs/plano.md
```

Primeiro commit:

```bash
git status
git add .
git commit -m "Cria projeto inicial de aprendizado Git"
```

Licao:

- `git init` transforma uma pasta em repositorio Git.
- `git status` mostra o estado atual.
- `git add` prepara arquivos para commit.
- `git commit` salva uma versao no historico.

## 3. Ciclo basico do dia a dia

Use esta sequencia sempre:

```bash
git status
git diff
git add arquivo
git commit -m "Mensagem clara"
git log --oneline
```

O que cada comando faz:

- `git status`: mostra arquivos modificados, preparados ou nao rastreados.
- `git diff`: mostra exatamente o que mudou antes do commit.
- `git add arquivo`: coloca o arquivo na area de commit.
- `git commit -m "mensagem"`: cria um commit.
- `git log --oneline`: mostra o historico resumido.

Exemplo de mensagem boa:

```bash
git commit -m "Adiciona lista inicial de tarefas"
```

## 4. Area de trabalho, stage e commit

Pense em tres areas:

```text
Arquivo editado -> Stage -> Commit
```

Exemplo:

```bash
git status
git add tarefas.md
git status
git commit -m "Atualiza tarefas"
```

Se o arquivo aparece em `Changes not staged for commit`, ele foi editado, mas ainda nao esta preparado.

Se aparece em `Changes to be committed`, ele ja esta no stage.

## 5. Desfazendo mudancas com seguranca

### Desfazer mudanca nao commitada

```bash
git restore arquivo
```

Exemplo:

```bash
git restore anotacoes.md
```

Isso descarta a alteracao local e volta ao ultimo commit.

### Tirar arquivo do stage

```bash
git restore --staged arquivo
```

Exemplo:

```bash
git restore --staged anotacoes.md
```

Isso tira o arquivo da area de commit, mas mantem a alteracao no arquivo.

### Corrigir o ultimo commit

```bash
git commit --amend -m "Mensagem corrigida"
```

Ou, para corrigir autor do ultimo commit:

```bash
git commit --amend --reset-author --no-edit
```

Licao importante:

- `--amend` recria o ultimo commit.
- Use antes de enviar para o GitHub.
- Evite usar em commits que outras pessoas ja puxaram.

## 6. Branches

Branch e uma linha separada de trabalho.

Criar e entrar em uma branch nova:

```bash
git switch -c feature/minha-branch
```

Entrar em uma branch existente:

```bash
git switch nome-da-branch
```

Ver branches:

```bash
git branch
```

Apagar branch local:

```bash
git branch -d nome-da-branch
```

Forcar apagar branch local:

```bash
git branch -D nome-da-branch
```

Licao:

- `git switch -c nome` cria branch nova.
- `git switch nome` entra em branch existente.
- Commits ficam na branch onde voce esta.

## 7. Merge

Merge junta o trabalho de uma branch em outra.

Fluxo comum:

```bash
git switch main
git merge feature/minha-branch
```

Quando aparece `Fast-forward`, significa que a `main` apenas avancou ate o commit da outra branch.

Exemplo:

```bash
git switch main
git merge feature-novos-clientes
```

## 8. Conflitos

Conflito acontece quando duas branches alteram a mesma parte do mesmo arquivo.

Durante o conflito, o arquivo pode aparecer assim:

```text
<<<<<<< HEAD
Versao da branch atual
=======
Versao da outra branch
>>>>>>> nome-da-branch
```

Como resolver:

1. Abra o arquivo.
2. Escolha a versao final.
3. Apague as marcas `<<<<<<<`, `=======` e `>>>>>>>`.
4. Salve.
5. Rode:

```bash
git add arquivo
git commit -m "Resolve conflito no arquivo"
```

Comando util para ver o estado:

```bash
git status
```

Se quiser desistir do merge:

```bash
git merge --abort
```

## 9. GitHub: remote, push e pull

Conectar repositorio local ao GitHub:

```bash
git remote add origin https://github.com/USUARIO/repositorio.git
git remote -v
```

Enviar pela primeira vez:

```bash
git push -u origin main
```

Depois da primeira vez:

```bash
git push
```

Trazer atualizacoes do GitHub:

```bash
git pull
```

Licao:

- `origin` e o apelido do repositorio remoto.
- `push` envia commits locais.
- `pull` traz commits do GitHub.
- `-u` conecta a branch local com a branch remota.

## 10. Token e login no GitHub

GitHub nao aceita mais senha comum no terminal via HTTPS.
No campo de senha, use um Personal Access Token.

No macOS, para salvar o token:

```bash
git config --global credential.helper osxkeychain
```

Se o terminal mostrar `#`, voce provavelmente esta como administrador/root.
O ideal e sair:

```bash
exit
```

Depois trabalhar como usuario normal.

Se aparecer erro de permissao em arquivos `.git`, corrija dono da pasta:

```bash
sudo chown -R leonardobarreto:staff /Users/leonardobarreto/Documents/AprendizadoGIT/git-aprendizado
```

## 11. Quando o push e rejeitado

Erro comum:

```text
non-fast-forward
fetch first
```

Significa que o GitHub tem commits que voce ainda nao tem localmente.

Solucao comum:

```bash
git pull
git push
```

Se os historicos foram criados separadamente:

```bash
git pull --no-rebase origin main --allow-unrelated-histories
```

Se aparecer conflito, resolva antes de tentar outro `push`.

## 12. Stash

`stash` guarda uma alteracao temporariamente sem criar commit.

Guardar:

```bash
git stash
```

Trazer de volta:

```bash
git stash pop
```

Ver lista de stashes:

```bash
git stash list
```

Uso comum:

```bash
git status
git stash
git pull
git stash pop
```

Licao:

- Use `stash` quando voce comecou algo, mas precisa trocar de contexto.
- Depois de `stash pop`, revise com `git status` e `git diff`.

## 13. Tags

Tag marca uma versao importante do projeto.

Criar tag:

```bash
git tag v0.1
```

Listar tags:

```bash
git tag
```

Ver detalhes:

```bash
git show v0.1
```

Enviar uma tag:

```bash
git push origin v0.1
```

Enviar todas as tags:

```bash
git push --tags
```

Uso comum:

- `v0.1`
- `v1.0`
- `v1.1`

## 14. Git show

Mostra detalhes de um commit, tag ou objeto.

Exemplo:

```bash
git show ec99098b
git show v0.1
```

Mostra:

- autor;
- data;
- mensagem;
- arquivos alterados;
- linhas adicionadas e removidas.

## 15. Git blame

Mostra qual commit alterou cada linha de um arquivo.

```bash
git blame tarefas.md
git blame anotacoes.md
```

Uso:

- descobrir quando uma linha nasceu;
- encontrar o commit responsavel;
- depois usar `git show COMMIT` para ver detalhes.

Fluxo:

```bash
git blame arquivo
git show codigo-do-commit
```

## 16. Revert

`git revert` desfaz um commit criando outro commit.
E o jeito seguro de desfazer algo que ja foi enviado ao GitHub.

Desfazer ultimo commit:

```bash
git revert HEAD
```

Depois:

```bash
git push
```

Licao:

- `revert` nao apaga historico.
- Ele cria um novo commit que desfaz o anterior.
- E recomendado para commits ja publicados.

## 17. Reflog

`git reflog` mostra os movimentos recentes da sua `HEAD`.
Ele ajuda a recuperar commits que sumiram do `git log`.

```bash
git reflog
```

Exemplo de recuperacao:

```bash
git branch recuperacao CODIGO_DO_COMMIT
git switch recuperacao
git log --oneline
```

Depois, se era so para inspecionar:

```bash
git switch main
git branch -D recuperacao
```

Licao:

- `git log` mostra a historia atual.
- `git reflog` mostra por onde voce passou.
- E muito util apos `amend`, `reset`, troca de branch ou erro.

## 18. Cherry-pick

`cherry-pick` pega um commit especifico e aplica na branch atual.

```bash
git cherry-pick CODIGO_DO_COMMIT
```

Fluxo:

```bash
git switch main
git cherry-pick 9a6f5b7
git status
```

Licao:

- O commit aplicado recebe um novo codigo.
- O conteudo e copiado para a branch atual.
- Use quando quiser apenas um commit, nao uma branch inteira.

## 19. Rebase

`rebase` reorganiza commits de uma branch por cima de outra.

Exemplo seguro:

```bash
git switch minha-branch
git rebase main
```

Depois, para integrar:

```bash
git switch main
git merge minha-branch
```

Licao:

- Use `rebase` em branches suas.
- Evite `rebase` em branch compartilhada.
- `rebase` deixa a historia mais linear.
- `merge` preserva a historia de juncao.

Ver grafo:

```bash
git log --oneline --graph --decorate -8
```

## 20. Bisect

`git bisect` ajuda a descobrir em qual commit um erro apareceu.

Fluxo:

```bash
git bisect start
git bisect bad
git bisect good COMMIT_BOM
```

Depois voce testa o projeto.

Se o erro existe:

```bash
git bisect bad
```

Se o erro nao existe:

```bash
git bisect good
```

Ao terminar:

```bash
git bisect reset
```

Licao:

- Voce informa um commit bom e um commit ruim.
- O Git procura o primeiro commit que introduziu o problema.
- E muito util em projetos grandes.

## 21. Pull Request no GitHub

Fluxo profissional:

```bash
git switch -c feature/minha-mudanca
editar arquivos
git add .
git commit -m "Mensagem clara"
git push -u origin feature/minha-mudanca
```

Depois no GitHub:

1. Abrir Pull Request.
2. Escrever titulo e descricao.
3. Revisar arquivos alterados.
4. Fazer merge.

Depois no terminal:

```bash
git switch main
git pull
git branch -d feature/minha-mudanca
git push origin --delete feature/minha-mudanca
git status
```

Licao:

- Branch local: existe no seu computador.
- Branch remota: existe no GitHub.
- Apague a branch depois que o PR for mesclado.

## 22. Erros comuns que aconteceram

### Digitar arquivo como comando

```bash
tarefas.md
```

Isso nao abre o arquivo.

No macOS, use:

```bash
open tarefas.md
```

### Digitar `--online`

Errado:

```bash
git log --online
```

Certo:

```bash
git log --oneline
```

### Usar um traco em `amend`

Errado:

```bash
git commit -amend
```

Certo:

```bash
git commit --amend
```

### Digitar mensagem do Git como comando

Mensagem:

```text
both added: README.md
```

Isso nao e comando.
E apenas uma informacao do Git.

### Usar `:wq` no lugar errado

`:wq` so serve quando o editor Vim esta aberto.
Nao use `:wq` como usuario do GitHub.

### Nome de arquivo com maiusculas e minusculas

O Git pode diferenciar:

```text
README.md
readme.md
```

Use o nome exato do arquivo.

## 23. Comandos de diagnostico

Quando algo parecer estranho, rode:

```bash
git status
git branch
git remote -v
git log --oneline --decorate -8
```

Para ver grafo:

```bash
git log --oneline --graph --decorate -10
```

Para ver arquivos rastreados:

```bash
git ls-files
```

Para ver diferenca ainda nao preparada:

```bash
git diff
```

Para ver diferenca ja no stage:

```bash
git diff --staged
```

## 24. Rotina recomendada no dia a dia

Antes de comecar:

```bash
git switch main
git pull
```

Criar branch:

```bash
git switch -c feature/nome-da-tarefa
```

Durante o trabalho:

```bash
git status
git diff
git add arquivo
git commit -m "Mensagem clara"
```

Enviar:

```bash
git push -u origin feature/nome-da-tarefa
```

Depois do PR:

```bash
git switch main
git pull
git branch -d feature/nome-da-tarefa
git push origin --delete feature/nome-da-tarefa
```

## 25. Boas praticas

- Faca commits pequenos.
- Escreva mensagens claras.
- Rode `git status` com frequencia.
- Revise `git diff` antes do commit.
- Use branch para cada tarefa.
- Use Pull Request para revisar mudancas.
- Use `revert` para desfazer commits ja enviados.
- Use `reflog` para recuperar caminhos perdidos.
- Use `stash` para guardar trabalho temporario.
- Evite `reset --hard` em projeto real sem backup.

## 26. Mensagens de commit boas

Use verbo no presente:

```bash
git commit -m "Adiciona lista inicial de tarefas"
git commit -m "Corrige texto do README"
git commit -m "Cria documentacao de clientes"
git commit -m "Remove tarefa duplicada"
git commit -m "Atualiza plano de estudos"
```

Evite:

```bash
git commit -m "alteracoes"
git commit -m "teste"
git commit -m "arruma coisa"
```

## 27. Exercicios para repetir

### Exercicio 1: ciclo basico

```bash
open anotacoes.md
git status
git diff
git add anotacoes.md
git commit -m "Atualiza anotacoes"
git push
```

### Exercicio 2: branch e merge

```bash
git switch -c feature/teste
open tarefas.md
git add tarefas.md
git commit -m "Atualiza tarefas em branch"
git switch main
git merge feature/teste
git branch -d feature/teste
```

### Exercicio 3: desfazer mudanca local

```bash
open anotacoes.md
git status
git diff
git restore anotacoes.md
git status
```

### Exercicio 4: stash

```bash
open tarefas.md
git status
git stash
git status
git stash pop
git status
```

### Exercicio 5: Pull Request

```bash
git switch -c feature/exercicio-pr
open README.md
git add README.md
git commit -m "Atualiza README em exercicio de PR"
git push -u origin feature/exercicio-pr
```

Depois abra o PR no GitHub, faca merge, volte ao terminal e limpe a branch.

## 28. Proximos temas para aprofundar

- `.gitignore`
- `git fetch` versus `git pull`
- Pull Request com revisao
- resolver conflito dentro de Pull Request
- squash merge
- merge commit
- rebase merge
- GitHub Releases
- tags anotadas
- branch protection rules
- GitHub Actions
- Conventional Commits
- trabalhar com forks
- desfazer commits com `reset`, `revert` e `restore`

## 29. Cola rapida

```bash
git status
git diff
git add .
git commit -m "Mensagem clara"
git push
```

```bash
git pull
git switch -c feature/nome
git merge feature/nome
git branch -d feature/nome
```

```bash
git restore arquivo
git restore --staged arquivo
git commit --amend -m "Mensagem corrigida"
```

```bash
git stash
git stash pop
git tag v0.1
git show COMMIT
git blame arquivo
```

```bash
git revert HEAD
git reflog
git cherry-pick COMMIT
git rebase main
git bisect start
```

## 30. Regra de ouro

Antes de qualquer decisao importante, rode:

```bash
git status
```

Depois pense:

- O que eu mudei?
- O que quero salvar?
- Em qual branch estou?
- Isso ja foi enviado ao GitHub?

Com essas quatro perguntas, voce evita a maioria dos problemas em Git.
