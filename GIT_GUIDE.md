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
