# 🐙 Guia Essencial de Comandos e Fluxos de Trabalho Git & GitHub

Este guia reúne comandos fundamentais e os principais fluxos de trabalho (Git Flow e GitHub Flow) para referência rápida.

---

## 1. Configuração e Inicialização

### GIT INIT & CLONE

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git clone` | Clona um repositório remoto para o seu `workdir` local. | `git clone https://github.com/usuario/repo.git` |
| `git clone --branch` | Clona apenas uma *branch* específica. | [cite_start]`git clone --branch dev https://github.com/Bussola2015/Git-Sketch.git` [cite: 1] |
| `git init` | Inicializa um repositório Git local no diretório atual (o *workdir*). | [cite_start]`git init` [cite: 1] |
| `git init --bare` | Inicializa um servidor Git em um diretório (repositório *bare*, sem *workdir*). | [cite_start]`git init --bare` [cite: 1] |

> [cite_start]**OBS:** Se você já clonou um repositório com `git clone`, não precisa usar `git init`. [cite: 1]

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
    git config --list 
    git config user.name 
    ```
* **`.gitignore`**: Arquivo no *workdir* ou em subdiretórios para desconsiderar arquivos do rastreamento Git. 

---

## 2. Visualização, Status e Logs

### GIT STATUS

* **Ver Status Atual**: Sempre usado para verificar o estado dos arquivos (modificados, *staged*, *untracked*).
    ```bash
    git status [cite: 2]
    ```

### GIT DIFF & SHOW

| Comando | Propósito | Comparação |
| :--- | :--- | :--- |
| `git diff` | Mostra as alterações não commitadas no *workdir* (compara com o último *commit*). | [cite_start]*Workdir* vs. Repo Local (*HEAD*) [cite: 3] |
| `git diff --staged` | Compara o que está no *staging area* (compara com último no *commit*). | [cite_start]*Staging* vs. Repo Local (*HEAD*) [cite: 3] |
| `git diff ID-commit..ID-commit` | Mostra a diferença em um intervalo de *commits*. | [cite_start]*Commit* A vs. *Commit* B [cite: 3] |
| `git show ID-commit` | Mostra as mudanças (e a mensagem) em um *commit* específico. | [cite_start]*Commit* vs. Seu Ancestral [cite: 3] |
| `git show` | Mostra as mudanças do último *commit* (o `HEAD`). | [cite_start]*HEAD* vs. *HEAD*^ [cite: 3] |

### GIT LOG & REFLOG

* **Histórico Completo**:
    ```bash
    [cite_start]git log [cite: 2]
    ```
* **Visualizações Úteis**:
    ```bash
    git log --oneline # Histórico em uma linha [cite: 2]
    git log --graph # Visualização gráfica (ASCII) [cite: 2]
    git log -p # Mais detalhes (com patches) [cite: 2]
    git log --author="user_name" # Log de autor específico [cite: 2]
    git log -n 2 # Mostra somente os últimos 2 commits [cite: 2]
    git log nome-branch # Log de branch específica [cite: 2]
    git log --pretty="format:%h %s" # Formata a visualização [cite: 2]
    ```
* **Reflog (Metadados do HEAD)**: Mantém o histórico de todos os `HEAD` (útil para recuperação).
    ```bash
    git reflog 
    ```
* **GIT BLAME**: Encontra o responsável (autor do *commit*) pelas modificações em cada linha de um arquivo.
    ```bash
    git blame nome-arquivo [cite: 31]
    ```

---

## 3. Ciclo de Vida do Arquivo

### GIT ADD & COMMIT

* **Adicionar Arquivos ao Staging Area**: Faz com que o Git passe a monitorar o arquivo, deixando-o pronto para o *commit*.
    ```bash
    git add nome-arquivo1 nome-arquivo2 [cite: 4]
    git add . [cite: 4]
    ```
* **Realizar Commit**:
    ```bash
    # Commita todos os arquivos no Staging Area
    git commit -m "Minha mensagem" [cite: 6]
    #commit individual
    git commit nome-arquivo1 -m "Descrição do commit para o arquivo 1"
    git commit nome-arquivo2 -m "Descrição do commit para o arquivo 2"
    ```
* **Modificar o Último Commit (`--amend`)**:
    ```bash
    # Altera apenas a mensagem do último commit
    git commit --amend -m "Trocando a mensagem do último commit" [cite: 6]

    # Adiciona um arquivo esquecido ao último commit
    git add arquivo [cite: 7]
    git commit --amend [cite: 7]
    ```
* **Adicionar Co-autor**: Inclui colaboradores no *commit* (pular duas linhas após a mensagem).
    ```bash
    git commit -m "Adicionar nova funcionalidade.

    Co-authored-by: NOME <nome@email.com>
    Co-authored-by: OUTRO-NOME <outro@email.com>" [cite: 7]
    ```

### GIT RESTORE (Desfazer não-commitados)

O `restore` desfaz alterações no escopo do *workdir* e *staging area*, **sem alterar o histórico de commits**.

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git restore --staged` | Retira o arquivo do *staging* (volta para o *workdir*). | [cite_start]`git restore nome-arquivo --staged nome-arquivo` [cite: 7] |
| `git restore nome-arquivo` | Desfaz as alterações do arquivo no *workdir* (volta ao estado do `HEAD`). | [cite_start]`git restore index.html` [cite: 8] |
| `git restore .` | Desfaz **todas** as alterações não commitadas no *workdir* (volta ao estado do `HEAD`). | [cite_start]`git restore .` [cite: 9] |
| `git restore --source` | Traz a versão de um arquivo de um *commit* antigo para o local atual. | [cite_start]`git restore --source 7b6a9ba index.html` [cite: 11] |
| `git restore --source .` | Restaura todos os arquivos do *workdir* para o estado de um *hash* específico. | [cite_start]`git restore --source 7b6a9ba .` [cite: 12] |

### GIT RESET (Desfazer commits locais)

O `reset` é ideal para desfazer *commits* locais, alterando o histórico do *branch*. Use `HEAD~N` para desfazer os últimos N *commits*.

| Opção | Propósito | Efeito nas Alterações |
| :--- | :--- | :--- |
| `git reset nome-arquivo` | Retira o arquivo da *staging area* (igual a `git restore --staged`). | [cite_start]Mantém no *workdir*. [cite: 13] |
| `git reset --soft HEAD~1` | Desfaz o último *commit* (move o `HEAD` para trás). | [cite_start]Mantém as modificações no **staging area**. [cite: 13] |
| `git reset --mixed HEAD~1` | Desfaz o último *commit* e tira do *staging*. | [cite_start]Mantém as modificações no **workdir**. [cite: 13] |
| `git reset --hard HEAD~1` | Desfaz o último *commit* e descarta as modificações. | [cite_start]**Apaga** as modificações do *workdir*. [cite: 13] |

### GIT REVERT (Desfazer commits remotos)

Cria um **novo commit** que desfaz as alterações de um *commit* anterior, **preservando o histórico**. Ideal para *commits* que já foram enviados (`push`).

```bash
[cite_start]git revert c23ae2d # Desfaz o commit c23ae2d e cria um novo commit [cite: 13]
[cite_start]git push # Envia o novo commit de reversão [cite: 13] 
```
### GIT REMOTE (Conexão)

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git remote add <nome> <URL>` | Adiciona um repositório remoto ao workdir local. | `git remote add upstream https://github.com/original/repo.git` |
| `git remote -v` | Lista os repositórios remotos vinculados. | `git remote -v` |
| `git remote remove <nome>` | Remove um repositório remoto. | `git remote remove upstream` |
| `git fetch <nome-remoto>` | Baixa os objetos (commits) do remoto para o local, mas não mescla (merge). | `git fetch origin` |

### GIT BRANCH & SWITCH

| Comando | Propósito | Exemplo |
| :--- | :--- | :--- |
| `git branch` | Lista as branches locais existentes. | `git branch` |
| `git branch -r` | Lista as branches remotas. | `git branch -r` |
| `git branch -a` | Lista todas (local e remota). | `git branch -a` |
| `git branch <nome-branch>` | Cria uma nova branch local. | `git branch hotfix/bug-01` |
| `git switch <nome-branch>` | Altera para a branch especificada. | `git switch feature/nova-api` |
| `git switch -c <nova-branch>` | Cria e altera para a nova branch (atalho para branch + switch). | `git switch -c feature/login` |
| `git branch -d <nome-branch>` | Exclui a branch local (se estiver mesclada). | `git branch -d feature/old-task` |
| `git branch -D <nome-branch>` | Força a exclusão da branch local. | `git branch -D feature/buggy-code` |
| `git push origin :<nome-branch>` | Exclui a branch no repositório remoto. | `git push origin :feature/done` |

### GIT MERGE & REBASE (Fundindo Branches)

| Comando | Propósito | Fluxo |
| :--- | :--- | :--- |
| `git merge <branch-origem>` | Funde o histórico da *branch-origem* na *branch-destino*. | Cria um **novo commit de merge**. |
| `git rebase <branch-destino>` | Move/reaplica os commits do *branch* atual no topo do *branch-destino*. | Cria um **histórico linear** e limpo. |
| `git rebase -i HEAD~N` | *Rebase* interativo. Usado para editar, unificar (`squash`), ou reordenar os últimos `N` commits antes de enviá-los. | |

### GIT STASH (Salvar Alterações Temporariamente)

| Comando | Propósito | Observação |
| :--- | :--- | :--- |
| `git stash` | Salva temporariamente alterações modificadas ou *staged* para voltar a elas depois. | Ideal para trocar de *branch* sem commitar. |
| `git stash list` | Lista as alterações salvas. | |
| `git stash apply <ID-stash>` | Reaplica as alterações salvas, mantendo o *stash* na lista. | |
| `git stash pop` | Reaplica as alterações e remove o *stash* da lista. | |
| `git stash drop <ID-stash>` | Remove manualmente o *stash*. | |

### GIT CHERRY-PICK (Migrar Commit)

Traz um commit específico de outra *branch* para a *branch* atual (destino), criando um novo commit com o mesmo conteúdo.

```bash
git cherry-pick <ID-commit>
```

### GIT TAG (Marcar Versões)

Usado para marcar versões de *releases*, *milestones*, etc.

* `git tag -a <nome-tag> -m "mensagem"`: Adiciona uma tag anotada (versão) ao commit atual (`HEAD`).
    * **Exemplo:** `git tag -a v1.0.0 -m "Versão de produção 1.0.0"`.
* `git push origin <nome-tag>`: Envia uma tag específica para o remoto.
* `git push origin --tags`: Envia todas as tags locais para o remoto.
* `git tag -d <nome-tag>`: Remove a tag local.
* `git tag -v <nome-tag>`: verificar a tag criada anotada local.

### 7.1. Fluxo Fork (Contribuição para Projetos Externos)

O *Fluxo Fork* é ideal para contribuir com projetos onde você não tem permissão de escrita direta no repositório original.

1.  **Fork no GitHub**:
    * O repositório original é copiado para a sua conta no GitHub (*fork*).
    * Agora você tem controle total sobre o *fork* na sua conta.

2.  **Clonar o Fork para o Seu Ambiente Local**:
    * Você clona o repositório *forkado* (o seu) para o seu computador (`workdir`).
    ```bash
    git clone [https://github.com/sua-conta/seu-fork.git](https://github.com/sua-conta/seu-fork.git)
    ```

3.  **Adicionar o Repositório Original como Remoto Adicional (`upstream`)**:
    * Para acompanhar as alterações que ocorrem no repositório original, adicione-o como um remoto chamado `upstream`.
    ```bash
    git remote add upstream [https://github.com/usuario-original/repo-original.git](https://github.com/usuario-original/repo-original.git)
    ```
    * **Verificação:** Para confirmar que o remoto foi adicionado:
    ```bash
    git remote -v
    ```

4.  **Verificar e Sincronizar as Alterações do Original (Upstream)**:
    * Use `git fetch upstream` para buscar as alterações do repositório original (`upstream`) sem alterar seu `workdir` ou *branch* local.
    ```bash
    git fetch upstream
    ```
    * **O que acontece?** O comando apenas atualiza as referências locais para as *branches* do repositório original.
    * Você pode inspecionar as alterações disponíveis sem aplicá-las:
    ```bash
    git log upstream/main
    ```

5.  **Aplicar Alterações do Repositório Original ao Seu Fork**:
    * **Mescle** as alterações do repositório original (`upstream/main`) com sua *branch* local (`main`):
    ```bash
    git merge upstream/main
    ```
    * **Envie** as alterações mescladas para o seu *fork* no GitHub (mantendo-o atualizado):
    ```bash
    git push origin main
    ```

> **Resumo da Sincronização:** Use `git fetch upstream` para buscar as alterações do original sem alterar nada no seu repositório local. Depois, use `git merge upstream/main` e `git push origin main` para aplicar e enviar as atualizações.


### 7.2. GitHub Flow (Com REBASE)

Este fluxo é ideal para manter um histórico de *commits* limpo e linear na *branch* principal, utilizando `git rebase` antes de abrir o Pull Request (PR).

#### FASE 1: Sincronizar Branch Principal

1.  **Garantir que a *branch* principal (`main`) local esteja selecionada:**
    ```bash
    git switch main
    ```
2.  **Baixar as alterações mais recentes do repositório remoto:**
    ```bash
    git pull origin main
    ```

#### FASE 2: Criar Feature Branch e Commitar

1.  **Criar e mudar para a nova *branch* de *feature*:**
    ```bash
    git switch -c feature/minha-tarefa
    # ou
    # git checkout -b feature/minha-tarefa
    ```
2.  **Trabalhar nos arquivos, adicionar ao *staging* e commitar:**
    ```bash
    git add .
    git commit -m "Implementando feature X"
    ```
3.  **Publicar as mudanças, criando a *branch* no remoto (`-u` define o *upstream*):**
    ```bash
    git push -u origin feature/minha-tarefa
    ```

#### FASE 3: Sincronizar e Rebasear (Limpeza de Histórico)

1.  **Voltar para a *branch* principal local:**
    ```bash
    git switch main
    ```
2.  **Sincronizar `main` novamente para garantir que está com as últimas alterações:**
    ```bash
    git pull origin main
    ```
3.  **Voltar para a *branch* de *feature*:**
    ```bash
    git switch feature/minha-tarefa
    ```
4.  **Sincronizar com a `main` via Rebase:**
    * Este comando envia os *commits* da sua *feature* para o topo da `main` local, eliminando o *commit* de *merge* e criando um histórico linear.
    ```bash
    git rebase main
    ```
5.  **Atualizar o *branch* remoto:**
    * Como o histórico foi reescrito (rebase), é necessário forçar o *push*. O `--force-with-lease` é o mais seguro.
    ```bash
    git push --force-with-lease
    ```

#### FASE 4: Pull Request (PR) e Limpeza

1.  **Abrir Pull Request no GitHub** (`feature/minha-tarefa` $\rightarrow$ `main`).
    * Após aprovação, o *merge* deve ser feito no GitHub (tipicamente com a opção *Squash and Merge* ou *Rebase and Merge*).
2.  **Voltar para `main` local e atualizar:**
    ```bash
    git switch main
    git pull origin main
    ```
3.  **Limpar a *branch* de *feature* local (se o *merge* já ocorreu):**
    ```bash
    git branch -d feature/minha-tarefa
    ```
### 7.3. GitHub Flow (Com MERGE Padrão)

Este fluxo cria um *commit* de mesclagem (merge commit) na *branch* de destino, preservando o histórico exato de quando as *branches* foram combinadas.

#### FASE 1: Sincronizar Branch Principal

1.  **Garantir que a *branch* principal (`main`) local esteja selecionada:**
    ```bash
    git switch main
    ```
2.  **Baixar as alterações mais recentes do repositório remoto:**
    ```bash
    git pull origin main
    ```

#### FASE 2: Criar Feature Branch e Commitar

1.  **Criar e mudar para a nova *branch* de *feature*:**
    ```bash
    git switch -c feature/nova-api
    # ou
    # git checkout -b feature/nova-api
    ```
2.  **Trabalhar nos arquivos, adicionar ao *staging* e commitar:**
    ```bash
    git add .
    git commit -m "Implementando feature X"
    ```
3.  **Publicar as mudanças, criando a *branch* no remoto (`-u` define o *upstream*):**
    ```bash
    git push -u origin feature/nova-api
    ```

#### FASE 3: Sincronizar Feature com Main (Antes do PR)

É altamente recomendado atualizar a *branch* de *feature* com as últimas alterações da `main` antes de abrir o PR para resolver conflitos localmente.

**Opção 1: Uso Direto do `git pull`**

1.  Estando na `feature/nova-api`, o `git pull origin main` buscará as alterações da `main` remota e as mesclará imediatamente na sua *feature* local.
    ```bash
    git pull origin main
    ```
2.  Envie as mudanças (incluindo o novo *merge commit*) para o remoto:
    ```bash
    git push
    ```

**Opção 2: Fluxo Explícito (Mais Didático)**

1.  **Sair da *feature* e atualizar `main` local:**
    ```bash
    git switch main
    git pull origin main
    ```
2.  **Voltar para a *feature*:**
    ```bash
    git switch feature/nova-api
    ```
3.  **Mesclar `main` na *feature*:**
    ```bash
    git merge main
    ```
4.  **Enviar as mudanças para o remoto:**
    ```bash
    git push origin feature/nova-api
    ```

#### FASE 4: Pull Request (PR) e Limpeza

1.  **PULL REQUEST no GitHub:**
    * Acessar a interface Web e fazer a solicitação para unir a *branch* (`feature/nova-api` $\rightarrow$ `main`).
    * Após aprovação, o *merge* é feito no GitHub.
2.  **Voltar para `main` local e atualizar:**
    ```bash
    git switch main
    git pull origin main
    ```
3.  **Limpar a *branch* de *feature* local (se o *merge* já ocorreu):**
    ```bash
    git branch -d feature/nova-api
    ```
### 7.4. Git Flow (Default) - Feature $\rightarrow$ Develop

Este é um fluxo de trabalho mais estruturado, onde o desenvolvimento principal acontece na *branch* `develop`.

#### FASE 1: Sincronizar Branch de Desenvolvimento

1.  **Mudar para a *branch* principal de desenvolvimento (`develop`):**
    ```bash
    git switch develop
    # ou
    # git checkout develop
    ```
2.  **Baixar as alterações mais recentes do repositório remoto:**
    ```bash
    git pull origin develop
    ```

#### FASE 2: Criar Feature Branch e Commitar

1.  **Criar a *branch* de *feature* a partir da `develop` e mudar para ela:**
    ```bash
    git switch -c feature/cadastro-usuario
    ```
2.  **Trabalhar nos arquivos, adicionar ao *staging* e commitar:**
    ```bash
    git add .
    git commit -m "Implementando cadastro-usuario"
    ```
3.  **Publicar as mudanças, criando a *branch* no remoto (`-u` define o *upstream*):**
    ```bash
    git push -u origin feature/cadastro-usuario
    ```

#### FASE 3: Sincronizar Feature com Develop (Antes do PR)

É comum atualizar a *branch* de *feature* com as últimas alterações da `develop` antes de mesclar.

1.  **Voltar para a `develop` local e atualizar:**
    ```bash
    git switch develop
    git pull origin develop
    ```
2.  **Voltar para a *branch* de *feature*:**
    ```bash
    git switch feature/cadastro-usuario
    ```
3.  **Mesclar `develop` na *feature* (para resolver conflitos localmente):**
    ```bash
    git merge develop
    ```
4.  **Enviar as mudanças para o remoto:**
    ```bash
    git push origin feature/cadastro-usuario
    ```

#### FASE 4: Pull Request (PR) e Limpeza

1.  **PULL REQUEST no GitHub:**
    * Acessar a interface Web e fazer a solicitação para unir a *branch* (`feature/cadastro-usuario` $\rightarrow$ `develop`).
    * Após aprovação, o *merge* é feito no GitHub.
2.  **Voltar para `develop` local e atualizar:**
    ```bash
    git switch develop
    git pull origin develop
    ```
3.  **Limpar a *branch* de *feature* local (se o *merge* já ocorreu):**
    ```bash
    git branch -d feature/cadastro-usuario
    ```

### 7.3. GitHub Flow (Com MERGE Padrão/Default)

Este fluxo utiliza a *branch* principal (`main`) como base e integra as *features* via *merge commit*, preservando um histórico completo de mesclagem.

#### FASE 1: Sincronizar Branch Principal

1.  **Garantir que a *branch* principal (`main`) local esteja selecionada:**
    ```bash
    git switch main
    ```
2.  **Baixar as alterações mais recentes do repositório remoto:**
    ```bash
    git pull origin main
    ```

#### FASE 2: Criar Feature Branch e Commitar

1.  **Criar e mudar para a nova *branch* de *feature*:**
    ```bash
    git switch -c feature/otimizar-performance
    ```
    * *(Edição de código...)*
2.  **Trabalhar nos arquivos, adicionar ao *staging* e commitar:**
    ```bash
    git add .
    git commit -m "Implementando otimizacao de cache"
    ```
3.  **Publicar as mudanças, criando a *branch* no remoto:**
    ```bash
    git push -u origin feature/otimizar-performance
    ```

#### FASE 3: Sincronizar Feature com Main (Antes do PR)

É crucial atualizar a *branch* de *feature* com as últimas mudanças da `main` antes de abrir o PR.

1.  **Voltar para a `main` local e atualizar:**
    ```bash
    git switch main
    git pull origin main
    ```
2.  **Voltar para a *feature*:**
    ```bash
    git switch feature/otimizar-performance
    ```
3.  **Mesclar `main` na *feature* (para resolver conflitos localmente):**
    ```bash
    git merge main
    ```
4.  **Atualizar a *branch* remota com o *merge commit*:**
    ```bash
    git push origin feature/otimizar-performance
    ```

#### FASE 4: Pull Request (PR) e Limpeza

1.  **PULL REQUEST no GitHub:**
    * Acessar a interface Web e fazer a solicitação para unir a *branch* (`feature/otimizar-performance` $\rightarrow$ `main`).
    * Após aprovação, o *merge* é feito no GitHub.
2.  **Voltar para `main` local e atualizar:**
    ```bash
    git switch main
    git pull origin main
    ```
3.  **Limpar a *branch* de *feature* local (se o *merge* já ocorreu):
    ```bash
    git branch -d feature/otimizar-performance
    ```
4.  **Remover a *branch* do repositório remoto:**
    ```bash
    git push origin :feature/otimizar-performance
    ```

