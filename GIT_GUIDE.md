# 🐙 Guia Essencial de Comandos e Fluxos de Trabalho Git & GitHub

Este guia reúne comandos fundamentais e os principais fluxos de trabalho (*Git Flow* e *GitHub Flow*) para referência rápida.

## Índice
- [1. Configuração e Inicialização](#1-configuração-e-inicialização)
  - [GIT INIT & CLONE](#git-init--clone)
  - [GIT CONFIG & .GITIGNORE](#git-config--gitignore)
- [2. Visualização, Status e Logs](#2-visualização-status-e-logs)
  - [GIT STATUS](#git-status)
  - [GIT DIFF & SHOW](#git-diff--show)
  - [GIT LOG & REFLOG](#git-log--reflog)
- [3. Ciclo de Vida do Arquivo](#3-ciclo-de-vida-do-arquivo)
  - [GIT ADD & COMMIT](#git-add--commit)
  - [GIT RESTORE](#git-restore-desfazer-não-commitados)
  - [GIT RESET](#git-reset-desfazer-commits-locais)
  - [GIT REVERT](#git-revert-desfazer-commits-remotos)
  - [GIT RM](#git-rm-remover-do-rastreamento)
- [4. Sincronização com Repositório Remoto](#4-sincronização-com-repositório-remoto)
  - [GIT PUSH & PULL](#git-push--pull)
  - [GIT REMOTE](#git-remote)
- [5. Gerenciamento de Branches](#5-gerenciamento-de-branches)
  - [GIT BRANCH & SWITCH](#git-branch--switch)
- [6. Merge e Rebase](#6-merge-e-rebase)
  - [GIT MERGE & REBASE](#git-merge--rebase)
- [7. Stash (Alterações Temporárias)](#7-stash-alterações-temporárias)
  - [GIT STASH](#git-stash)
- [8. Tags (Marcar Versões)](#8-tags-marcar-versões)
  - [GIT TAG](#git-tag)
- [9. Cherry-pick (Migrar Commit)](#9-cherry-pick-migrar-commit)
  - [GIT CHERRY-PICK](#git-cherry-pick)
- [10. Busca Binária](#10-busca-binária)
  - [GIT BISECT](#git-bisect)
- [11. Fluxos de Trabalho](#11-fluxos-de-trabalho)
  - [11.1. Fluxo Fork](#111-fluxo-fork-contribuição-para-projetos-externos)
  - [11.2. GitHub Flow (com Rebase)](#112-github-flow-com-rebase)
  - [11.3. GitHub Flow (com Merge Padrão)](#113-github-flow-com-merge-padrão)
  - [11.4. Git Flow (Feature → Develop)](#114-git-flow-default---feature--develop)
- [12. Glossário Rápido](#12-glossário-rápido)
- [13. Dicas Finais](#13-dicas-finais)

---

## 1. Configuração e Inicialização

### GIT INIT & CLONE

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git clone` | Clona um repositório remoto para o *workdir* local. | [cite_start]`git clone https://github.com/usuario/repo.git` [cite: 1] |
| `git clone --branch` | Clona apenas uma *branch* específica. | [cite_start]`git clone --branch dev https://github.com/Bussola2015/Git-Sketch.git` [cite: 1] |
| `git init` | Inicializa um repositório Git local no diretório atual (*workdir*). | [cite_start]`git init` [cite: 1] |
| `git init --bare` | Inicializa um servidor Git em um diretório (repositório *bare*, sem *workdir*). | [cite_start]`git init --bare` [cite: 1] |

> [cite_start]**OBS:** Se você já clonou um repositório com `git clone`, não precisa usar `git init`. [cite: 1]

---

### GIT CONFIG & .GITIGNORE

* **Configuração Global (Primeira Vez)**: Define a identidade para todos os repositórios na máquina.
    ```bash
    git config --global user.email "you@example.com" [cite: 14]
    git config --global user.name "Your Name" [cite: 14]
    ```
* **Configuração Local (Específica do Projeto)**: Sobrescreve a configuração global.
    ```bash
    git config user.email "you@example.com" [cite: 14]
    git config user.name "Your Name" [cite: 14]
    ```
* **Verificar Configurações**:
    ```bash
    git config --list [cite: 14]
    git config user.name [cite: 14]
    git config user.email [cite: 14]
    ```
* **`.gitignore`**: Arquivo no *workdir* ou em subdiretórios para desconsiderar arquivos do rastreamento Git. Exemplo: ignorar `node_modules/` ou `.env`. [cite: 15]

---

## 2. Visualização, Status e Logs

### GIT STATUS

* **Ver Status Atual**: Verifica o estado dos arquivos (modificados, *staged*, *untracked*).
    ```bash
    git status [cite: 2]
    ```

---

### GIT DIFF & SHOW

| Comando | Propósito | Comparação |
| :--- | :--- | :--- |
| `git diff` | Mostra alterações não commitadas no *workdir* em relação ao último *commit*. | [cite_start]*Workdir* vs. *HEAD* [cite: 3] |
| `git diff --staged` | Compara alterações na *staging area* com o último *commit*. | [cite_start]*Staging* vs. *HEAD* [cite: 3] |
| `git diff ID-commit..ID-commit` | Compara alterações entre dois *commits*. | [cite_start]*Commit A* vs. *Commit B* [cite: 3] |
| `git show ID-commit` | Exibe alterações e mensagem de um *commit* específico. | [cite_start]*Commit* vs. Ancestral [cite: 3] |
| `git show` | Exibe alterações do último *commit* (*HEAD*). | [cite_start]*HEAD* vs. *HEAD^1* [cite: 3] |

---

### GIT LOG & REFLOG

* **Histórico Completo**:
    ```bash
    git log [cite: 2]
    ```
* **Visualizações Úteis**:
    ```bash
    git log --oneline # Histórico em uma linha [cite: 2]
    git log --graph # Visualização gráfica (ASCII) [cite: 2]
    git log -p # Detalhes completos (com patches) [cite: 2]
    git log --author="user_name" # Filtra por autor [cite: 2]
    git log -n 2 # Mostra os últimos 2 commits [cite: 2]
    git log nome-branch # Log de uma branch específica [cite: 2]
    git log --pretty="format:%h %s" # Formato personalizado [cite: 2]
    ```
* **Reflog (Metadados do HEAD)**: Registra movimentos do *HEAD*, útil para recuperar *commits* perdidos após *resets*.
    ```bash
    git reflog [cite: 28]
    ```
    * **Exemplo**: Recuperar um *commit* perdido após `git reset --hard`:
      ```bash
      git reflog # Identifica o ID do commit
      git checkout ID-commit # Volta ao commit
      git branch nova-branch # Cria nova branch
      ```
* **GIT BLAME**: Mostra o autor de cada linha de um arquivo.
    ```bash
    git blame nome-arquivo [cite: 31]
    ```

---

## 3. Ciclo de Vida do Arquivo

### GIT ADD & COMMIT

* **Adicionar Arquivos ao Staging Area**: Prepara arquivos para o *commit*.
    ```bash
    git add nome-arquivo1 nome-arquivo2 [cite: 4]
    git add . [cite: 4]
    ```
* **Realizar Commit**:
    ```bash
    # Commita todos os arquivos no *staging*
    git commit -m "feat: adiciona nova funcionalidade" [cite: 6]
    # Commit individual
    git commit nome-arquivo1 -m "fix: corrige bug no arquivo 1" [cite: 6]
    git commit nome-arquivo2 -m "docs: atualiza documentação" [cite: 6]
    ```
* **Modificar o Último Commit (`--amend`)**:
    ```bash
    # Altera a mensagem do último commit
    git commit --amend -m "fix: ajusta mensagem do commit" [cite: 6]
    # Adiciona arquivo esquecido
    git add arquivo [cite: 7]
    git commit --amend [cite: 7]
    ```
* **Adicionar Co-autor**:
    ```bash
    git commit -m "feat: implementa login.

    Co-authored-by: Nome <nome@email.com>
    Co-authored-by: Outro Nome <outro@email.com>" [cite: 7]
    ```

---

### GIT RESTORE (Desfazer Não-Commitados)

O `restore` desfaz alterações no *workdir* e *staging area*, sem alterar o histórico de *commits*.

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git restore --staged` | Remove arquivo do *staging*, mantendo no *workdir*. | [cite_start]`git restore --staged index.html` [cite: 7] |
| `git restore nome-arquivo` | Desfaz alterações no *workdir* (volta ao *HEAD*). | [cite_start]`git restore index.html` [cite: 8] |
| `git restore .` | Desfaz todas as alterações no *workdir* (volta ao *HEAD*). | [cite_start]`git restore .` [cite: 9] |
| `git restore --source` | Restaura arquivo de um *commit* antigo para o *workdir*. | [cite_start]`git restore --source 7b6a9ba index.html` [cite: 11] |
| `git restore --source .` | Restaura todo o *workdir* para um *commit* antigo. | [cite_start]`git restore --source 7b6a9ba .` [cite: 12] |

---

### GIT RESET (Desfazer Commits Locais)

O `reset` desfaz *commits* locais, alterando o histórico da *branch*.

| Opção | Propósito | Efeito nas Alterações |
| :--- | :--- | :--- |
| `git reset nome-arquivo` | Remove arquivo do *staging* (igual a `git restore --staged`). | [cite_start]Mantém no *workdir*. [cite: 13] |
| `git reset --soft HEAD~1` | Desfaz o último *commit*, mantendo alterações no *staging*. | [cite_start]Alterações no *staging*. [cite: 13] |
| `git reset --mixed HEAD~1` | Desfaz o último *commit* e remove do *staging*. | [cite_start]Alterações no *workdir*. [cite: 13] |
| `git reset --hard HEAD~1` | Desfaz o último *commit* e descarta alterações. | [cite_start]Apaga alterações do *workdir*. [cite: 13] |

---

### GIT REVERT (Desfazer Commits Remotos)

Cria um novo *commit* que reverte alterações de um *commit* anterior, preservando o histórico.

```bash
git revert c23ae2d # Desfaz o commit c23ae2d [cite: 13]
git push # Envia a reversão [cite: 13]
```

---

### GIT RM (Remover do Rastreamento)

* Remove arquivo do controle de versão, mantendo-o no sistema de arquivos.
    ```bash
    git rm --cached nome-arquivo [cite: 4]
    git commit -m "chore: remove nome-arquivo do rastreamento" [cite: 4]
    ```

---

## 4. Sincronização com Repositório Remoto

### GIT PUSH & PULL

| Comando | Propósito | Exemplo | Observação |
| :--- | :--- | :--- | :--- |
| `git push origin main` | Envia *commits* locais para o remoto. | [cite_start]`git push origin main` [cite: 16] | Verifique conflitos antes. |
| `git push -u origin main` | Vincula *branch* local ao remoto. | [cite_start]`git push -u origin main` [cite: 16] | Permite `git push` sem especificar. |
| `git push --all origin` | Envia todas as *branches* locais. | [cite_start]`git push --all origin` [cite: 16] | Útil para novos repositórios. |
| `git push origin branch1 branch2` | Envia *branches* específicas. | [cite_start]`git push origin dev feature/login` [cite: 16] | Especifique múltiplas *branches*. |
| `git push origin main --force` | Força o envio, sobrescrevendo o remoto. | [cite_start]`git push origin main --force` [cite: 16] | Use `--force-with-lease` para segurança. |
| `git pull` | Sincroniza local com remoto (*fetch* + *merge*). | [cite_start]`git pull` [cite: 16] | Resolve conflitos localmente. |
| `git pull --rebase` | Sincroniza com *rebase* em vez de *merge*. | [cite_start]`git pull --rebase` [cite: 16] | Cria histórico linear. |
| `git pull origin branch` | Sincroniza uma *branch* específica. | [cite_start]`git pull origin dev` [cite: 16] | Especifique o remoto e a *branch*. |

---

### GIT REMOTE

| Comando | Propósito | Exemplo | Observação |
| :--- | :--- | :--- | :--- |
| `git remote add nome URL` | Adiciona um repositório remoto. | [cite_start]`git remote add origin https://github.com/usuario/repo.git` [cite: 16] | Use `origin` ou `upstream` como padrão. |
| `git remote -v` | Lista repositórios remotos vinculados. | [cite_start]`git remote -v` [cite: 16] | Verifica URLs de remotos. |
| `git remote remove nome` | Remove um repositório remoto. | [cite_start]`git remote remove origin` [cite: 16] | Remove vínculo local. |
| `git remote rename nome novo-nome` | Renomeia um repositório remoto. | [cite_start]`git remote rename origin novo-origin` [cite: 16] | Útil para reorganizações. |

---

## 5. Gerenciamento de Branches

### GIT BRANCH & SWITCH

| Comando | Propósito | Exemplo | Observação |
| :--- | :--- | :--- | :--- |
| `git branch` | Lista *branches* locais. | [cite_start]`git branch` [cite: 17] | Mostra *branch* atual com `*`. |
| `git branch -r` | Lista *branches* remotas. | [cite_start]`git branch -r` [cite: 17] | Inclui `origin/nome-branch`. |
| `git branch -a` | Lista todas (*local* e *remota*). | [cite_start]`git branch -a` [cite: 17] | Útil para visão geral. |
| `git branch nome-branch` | Cria uma nova *branch* local. | [cite_start]`git branch feature/nova-api` [cite: 17] | Não muda para a *branch*. |
| `git switch nome-branch` | Altera para a *branch* especificada. | [cite_start]`git switch feature/nova-api` [cite: 17] | Alternativa ao `checkout`. |
| `git switch -c nome-branch` | Cria e altera para a nova *branch*. | [cite_start]`git switch -c feature/login` [cite: 17] | Equivalente a `checkout -b`. |
| `git branch -d nome-branch` | Exclui *branch* local (se mesclada). | [cite_start]`git branch -d feature/old-task` [cite: 17] | Falha se não mesclada. |
| `git branch -D nome-branch` | Força exclusão de *branch* local. | [cite_start]`git branch -D feature/buggy-code` [cite: 17] | Remove mesmo sem mesclar. |
| `git branch -m nome-antigo novo-nome` | Renomeia uma *branch*. | [cite_start]`git branch -m feature/old feature/new` [cite: 17] | Atualiza localmente. |
| `git checkout ID-commit` | Move *HEAD* para um *commit* (modo *detached*). | [cite_start]`git checkout 7b6a9ba` [cite: 17] | Útil para inspeção. |

---

## 6. Merge e Rebase

### GIT MERGE & REBASE

| Comando | Propósito | Fluxo | Observação |
| :--- | :--- | :--- | :--- |
| `git merge nome-branch-origem` | Funde *branch-origem* na *branch-destino*. | [cite_start]Cria um *commit* de merge. [cite: 27] | Pode gerar conflitos. |
| `git rebase nome-branch-destino` | Reaplica *commits* da *branch* atual no topo da *branch-destino*. | [cite_start]Cria histórico linear. [cite: 27] | Evita *merge commits*. |
| `git rebase -i HEAD~N` | Edita, unifica (*squash*) ou reordena *commits*. | [cite_start]`git rebase -i HEAD~3` [cite: 27] | Útil para limpar histórico. |

* **Exemplo de Rebase Interativo**:
  ```bash
  git rebase -i HEAD~3
  # No editor, altere "pick" para "squash" para unificar commits
  # Salve e edite a mensagem final do commit
  ```

---

## 7. Stash (Alterações Temporárias)

### GIT STASH

| Comando | Propósito | Observação |
| :--- | :--- | :--- |
| `git stash` | Salva alterações do *workdir* ou *staging* temporariamente. | [cite_start]Ideal para trocar de *branch* sem commitar. [cite: 29] |
| `git stash list` | Lista *stashes* salvas. | [cite_start]`git stash list` [cite: 29] |
| `git stash apply ID-stash` | Reaplica *stash*, mantendo-o na lista. | [cite_start]`git stash apply stash@{0}` [cite: 29] |
| `git stash pop` | Reaplica o último *stash* e o remove. | [cite_start]`git stash pop` [cite: 29] |
| `git stash drop ID-stash` | Remove *stash* específico. | [cite_start]`git stash drop stash@{0}` [cite: 29] |
| `git stash clear` | Remove todos os *stashes*. | [cite_start]`git stash clear` [cite: 29] |

---

## 8. Tags (Marcar Versões)

### GIT TAG

| Comando | Propósito | Exemplo | Observação |
| :--- | :--- | :--- | :--- |
| `git tag -a nome-tag -m "mensagem"` | Cria tag anotada no *commit* atual (*HEAD*). | [cite_start]`git tag -a v1.0.0 -m "Versão 1.0.0"` [cite: 30] | Útil para *releases*. |
| `git tag -a nome-tag ID-commit -m "mensagem"` | Cria tag para *commit* específico. | [cite_start]`git tag -a v1.0.0 c23ae2d -m "Versão 1.0.0"` [cite: 30] | Especifique o *commit*. |
| `git tag` | Lista todas as tags. | [cite_start]`git tag` [cite: 30] | Mostra tags locais. |
| `git push origin nome-tag` | Envia tag ao remoto. | [cite_start]`git push origin v1.0.0` [cite: 30] | Necessário para *releases*. |
| `git push origin --tags` | Envia todas as tags locais. | [cite_start]`git push origin --tags` [cite: 30] | Envia todas de uma vez. |
| `git show nome-tag` | Exibe detalhes da tag. | [cite_start]`git show v1.0.0` [cite: 30] | Mostra *commit* associado. |
| `git tag -v nome-tag` | Verifica tag anotada. | [cite_start]`git tag -v v1.0.0` [cite: 30] | Confirma integridade. |
| `git tag -d nome-tag` | Remove tag local. | [cite_start]`git tag -d v1.0.0` [cite: 30] | Não afeta o remoto. |

---

## 9. Cherry-pick (Migrar Commit)

### GIT CHERRY-PICK

* Traz um *commit* específico de outra *branch* para a *branch* atual, criando um novo *commit*.
    ```bash
    git cherry-pick ID-commit [cite: 31]
    ```
    * **Exemplo**: Trazer um *commit* de correção de bug de `feature/bugfix` para `main`:
      ```bash
      git switch main
      git cherry-pick c23ae2d
      git push origin main
      ```

---

## 10. Busca Binária

### GIT BISECT

* Localiza o *commit* que introduziu um problema usando busca binária.

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git bisect start` | Inicia a busca binária. | [cite_start]`git bisect start` [cite: 32] |
| `git bisect bad HEAD` | Marca o *commit* atual (*HEAD*) como ruim. | [cite_start]`git bisect bad HEAD` [cite: 32] |
| `git bisect good ID-commit` | Marca um *commit* antigo como bom. | [cite_start]`git bisect good c23ae2d` [cite: 32] |
| `git bisect good` | Marca o *commit* atual como bom. | [cite_start]`git bisect good` [cite: 32] |
| `git bisect bad` | Marca o *commit* atual como ruim. | [cite_start]`git bisect bad` [cite: 32] |
| `git bisect reset` | Finaliza a busca e retorna ao estado original. | [cite_start]`git bisect reset` [cite: 32] |
| `git revert ID-commit` | Reverte o *commit* encontrado. | [cite_start]`git revert c23ae2d` [cite: 32] |

* **Exemplo**: Encontrar o *commit* que causou um bug:
  ```bash
  git bisect start
  git bisect bad HEAD
  git bisect good a1b2c3d
  # Teste o código e marque como good/bad
  git bisect good
  git bisect bad
  # Quando encontrar o commit
  git bisect reset
  git revert ID-commit
  ```

---

## 11. Fluxos de Trabalho

### Resumo dos Fluxos

| Fluxo | Branch Principal | Integração | Histórico | Uso Comum |
| :--- | :--- | :--- | :--- | :--- |
| *GitHub Flow (Rebase)* | `main` | *Rebase* antes do PR | Linear | Projetos ágeis, equipes pequenas |
| *GitHub Flow (Merge)* | `main` | *Merge* com *commit* de mesclagem | Preserva histórico | Projetos com auditoria |
| *Git Flow* | `develop` (e `main` para *releases*) | *Merge* na `develop` | Preserva histórico | Projetos complexos com *releases* |

---

### 11.1. Fluxo Fork (Contribuição para Projetos Externos)

O *Fluxo Fork* é ideal para contribuir com projetos sem permissão de escrita direta.

1.  **Fork no GitHub**:
    * Copie o repositório original para sua conta GitHub (*fork*).
2.  **Clonar o Fork para o Seu Ambiente Local**:
    ```bash
    git clone https://github.com/sua-conta/seu-fork.git [cite: 23]
    ```
3.  **Adicionar o Repositório Original como Remoto Adicional (`upstream`)**:
    ```bash
    git remote add upstream https://github.com/usuario-original/repo-original.git [cite: 23]
    git remote -v [cite: 23]
    ```
4.  **Verificar e Sincronizar as Alterações do Original (Upstream)**:
    ```bash
    git fetch upstream [cite: 24]
    git log upstream/main [cite: 25]
    ```
5.  **Aplicar Alterações do Repositório Original ao Seu Fork**:
    ```bash
    git merge upstream/main [cite: 26]
    git push origin main [cite: 26]
    ```

> [cite_start]**Resumo:** Use `git fetch upstream` para buscar alterações sem mudar o *workdir*. Depois, use `git merge upstream/main` e `git push origin main` para atualizar o *fork*. [cite: 26]

---

### 11.2. GitHub Flow (com Rebase)

Ideal para históricos lineares, usando *rebase* antes do Pull Request (PR).

#### FASE 1: Sincronizar Branch Principal
1.  **Selecionar `main` local:**
    ```bash
    git switch main [cite: 33]
    ```
2.  **Baixar alterações do remoto:**
    ```bash
    git pull origin main [cite: 33]
    ```

#### FASE 2: Criar Feature Branch e Commitar
1.  **Criar e mudar para a *branch* de *feature*:**
    ```bash
    git switch -c feature/minha-tarefa [cite: 33]
    ```
2.  **Trabalhar, adicionar e commitar:**
    ```bash
    git add . [cite: 33]
    git commit -m "feat: implementa nova funcionalidade" [cite: 33]
    ```
3.  **Publicar a *branch* no remoto:**
    ```bash
    git push -u origin feature/minha-tarefa [cite: 33]
    ```

#### FASE 3: Sincronizar e Rebasear
1.  **Voltar para `main` local:**
    ```bash
    git switch main [cite: 33]
    git pull origin main [cite: 33]
    ```
2.  **Voltar para a *branch* de *feature*:**
    ```bash
    git switch feature/minha-tarefa [cite: 33]
    ```
3.  **Rebase com `main` para histórico linear:**
    ```bash
    git rebase main [cite: 33]
    ```
4.  **Atualizar o remoto:**
    ```bash
    git push --force-with-lease [cite: 33]
    ```

#### FASE 4: Pull Request (PR) e Limpeza
1.  **Abrir PR no GitHub** (`feature/minha-tarefa` → `main`).
2.  **Após aprovação, fazer *merge* no GitHub.**
3.  **Atualizar e limpar localmente:**
    ```bash
    git switch main [cite: 33]
    git pull origin main [cite: 33]
    git branch -d feature/minha-tarefa [cite: 33]
    ```

---

### 11.3. GitHub Flow (com Merge Padrão)

Cria um *commit* de mesclagem, preservando o histórico de mesclagem.

#### FASE 1: Sincronizar Branch Principal
1.  **Selecionar `main` local:**
    ```bash
    git switch main [cite: 34]
    ```
2.  **Baixar alterações do remoto:**
    ```bash
    git pull origin main [cite: 34]
    ```

#### FASE 2: Criar Feature Branch e Commitar
1.  **Criar e mudar para a *branch* de *feature*:**
    ```bash
    git switch -c feature/nova-api [cite: 34]
    ```
2.  **Trabalhar, adicionar e commitar:**
    ```bash
    git add . [cite: 34]
    git commit -m "feat: implementa nova API" [cite: 34]
    ```
3.  **Publicar a *branch* no remoto:**
    ```bash
    git push -u origin feature/nova-api [cite: 34]
    ```

#### FASE 3: Sincronizar Feature com Main (Antes do PR)
**Opção 1: Uso Direto do `git pull`**
1.  **Mesclar `main` na *feature* local:**
    ```bash
    git pull origin main [cite: 34]
    ```
2.  **Enviar alterações para o remoto:**
    ```bash
    git push [cite: 34]
    ```

**Opção 2: Fluxo Explícito**
1.  **Atualizar `main` local:**
    ```bash
    git switch main [cite: 34]
    git pull origin main [cite: 34]
    ```
2.  **Voltar para a *feature*:**
    ```bash
    git switch feature/nova-api [cite: 34]
    ```
3.  **Mesclar `main` na *feature*:**
    ```bash
    git merge main [cite: 34]
    ```
    * **Exemplo de Resolução de Conflitos**:
      ```bash
      # Edite os arquivos com conflitos
      git add arquivo-conflito
      git commit -m "fix: resolve conflitos de merge"
      ```
4.  **Enviar alterações para o remoto:**
    ```bash
    git push origin feature/nova-api [cite: 34]
    ```

#### FASE 4: Pull Request (PR) e Limpeza
1.  **Abrir PR no GitHub** (`feature/nova-api` → `main`).
2.  **Após aprovação, fazer *merge* no GitHub.**
3.  **Atualizar e limpar localmente:**
    ```bash
    git switch main [cite: 34]
    git pull origin main [cite: 34]
    git branch -d feature/nova-api [cite: 34]
    git push origin :feature/nova-api [cite: 34]
    ```

---

### 11.4. Git Flow (Default - Feature → Develop)

Estrutura para projetos com *branches* `develop` e `main`.

#### FASE 1: Sincronizar Branch de Desenvolvimento
1.  **Mudar para `develop` local:**
    ```bash
    git switch develop [cite: 35]
    ```
2.  **Baixar alterações do remoto:**
    ```bash
    git pull origin develop [cite: 35]
    ```

#### FASE 2: Criar Feature Branch e Commitar
1.  **Criar e mudar para a *branch* de *feature*:**
    ```bash
    git switch -c feature/cadastro-usuario [cite: 35]
    ```
2.  **Trabalhar, adicionar e commitar:**
    ```bash
    git add . [cite: 35]
    git commit -m "feat: implementa cadastro de usuário" [cite: 35]
    ```
3.  **Publicar a *branch* no remoto:**
    ```bash
    git push -u origin feature/cadastro-usuario [cite: 35]
    ```

#### FASE 3: Sincronizar Feature com Develop
1.  **Atualizar `develop` local:**
    ```bash
    git switch develop [cite: 35]
    git pull origin develop [cite: 35]
    ```
2.  **Voltar para a *feature*:**
    ```bash
    git switch feature/cadastro-usuario [cite: 35]
    ```
3.  **Mesclar `develop` na *feature*:**
    ```bash
    git merge develop [cite: 35]
    ```
    * **Exemplo de Resolução de Conflitos**:
      ```bash
      # Edite os arquivos com conflitos
      git add arquivo-conflito
      git commit -m "fix: resolve conflitos de merge"
      ```
4.  **Enviar alterações para o remoto:**
    ```bash
    git push origin feature/cadastro-usuario [cite: 35]
    ```

#### FASE 4: Pull Request (PR) e Limpeza
1.  **Abrir PR no GitHub** (`feature/cadastro-usuario` → `develop`).
2.  **Após aprovação, fazer *merge* no GitHub.**
3.  **Atualizar e limpar localmente:**
    ```bash
    git switch develop [cite: 35]
    git pull origin develop [cite: 35]
    git branch -d feature/cadastro-usuario [cite: 35]
    ```

---

## 12. Glossário Rápido

- **Workdir**: Diretório de trabalho onde os arquivos do projeto estão localizados.
- **Staging Area**: Área intermediária onde arquivos são preparados para o *commit*.
- **HEAD**: Ponteiro para o *commit* atual (geralmente o último da *branch* ativa).
- **Commit**: Registro de alterações no repositório com uma mensagem descritiva.
- **Branch**: Linha de desenvolvimento independente no repositório.
- **Remote**: Repositório hospedado fora do *workdir* local (ex.: GitHub).
- **Merge**: Combinação de duas *branches*, criando um *commit* de mesclagem.
- **Rebase**: Reaplicação de *commits* em outra base, criando um histórico linear.

---

## 13. Dicas Finais

* **Evite `git push --force`**: Prefira `git push --force-with-lease` para evitar sobrescrever alterações alheias.
* **Commits Claros**: Use mensagens padronizadas (ex.: `feat:`, `fix:`, `docs:`) para maior legibilidade.
* **Resolva Conflitos Localmente**: Sempre sincronize antes de abrir PRs para minimizar conflitos.
* **Use `.gitignore`**: Ignore arquivos desnecessários como `.env`, `node_modules/`.
* **Nomenclatura de Branches**: Adote padrões como `feature/nome`, `bugfix/nome`, `hotfix/nome`.
* **Aliases Úteis**: Configure atalhos para comandos frequentes:
  ```bash
  git config --global alias.st status
  git config --global alias.co checkout
  git config --global alias.br branch
  ```

Este *cheat sheet* é seu guia para dominar o Git e o GitHub! 🚀
