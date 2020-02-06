---
title: Ler conteúdo de um arquivo no terminal do Linux
date: 2020-02-06
tags: linux
---

Para ler o conteúdo de arquivos no terminal, utilizar algum dos comandos a seguir:

```bash
# ler todo o conteúdo de um arquivo
cat arquivo.txt

# ler somente 10 primeiras linhas do arquivo
head arquivo.txt

# ler 3 primeiras linhas do arquivo (-n permite especificar o número de linhas)
head -n 3 arquivo.txt

# ler 10 últimas linhas do arquivo
tail arquivo.txt

# ler 5 últimas linhas do arquivo (-n permite especificar o número de linhas)
tail -n 5 arquivo.txt

# navegar pelo texto com as setas do teclado (para sair, usar tecla q)
less arquivo.txt
```
