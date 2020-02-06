---
title: Editar arquivos no Vi
date: 2020-02-06
tags: linux
---

Para abrir um arquivo no editor Vi:

```bash
vi <nome-do-arquivo>
```

## Comandos de edição

- `i` - entrar no modo inserção de texto na posição do cursor
- `a` - entrar no modo inserção de texto na posição seguinte ao cursor
- `x` - excluir caracter na posição do cursor
- `10x` - excluir 10 caracteres
- `dd` - excluir linha
- `3dd` - excluir 3 linhas
- `ESC` - voltar ao modo de navegação
- `:w` - salvar alterações
- `:q` - sair do editor e voltar ao terminal
- `:q!` - sair do editor sem salvar
- `:wq` - salvar e sair

## Comandos de navegação

- Setas do teclado: navega pela texto
- `G` - última linha do arquivo
- `3G` - terceira linha do arquivo
- `0` - começo da linha
- `$` - final da linha

## Comandos de busca de palavras

- `/google` - busca ocorrências para a palavra 'google'
- `n` - vai para a próxima ocorrência da palavra
- `N` - vai para a ocorrência anterior

## Comandos para copiar e colar texto

- `yy` - copiar linha
- `3yy` - copiar 3 linhas
- `p` - colar conteúdo copiado
- `3p` - colar 3 vezes o conteúdo copiado
