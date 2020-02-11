---
title: Compactando e descompactando arquivos no Linux
date: 2020-02-06
tags: linux
---

Para compactar arquivos no terminal do Linux, podem ser usados os comandos `zip` e `tar`.

## zip

```bash
# compactar arquivos
zip <arquivoZipado.zip> <arquivoA> <arquivoB> <arquivoC>

#compactar um diretorio e seus arquivos: -r(recursivo)
zip -r <arquivoZipado.zip> <diretorioAZipar>

# listar arquivos dentro do zip 
unzip -l nome.zip

# descompactar arquivos
unzip nome.zip

#descompactar arquivos: -q(quiet, não mostra log)
unzip -q nome.zip
```

## tar

O comando `tar` empacota vários arquivos em um único, porém, **sem realizar a compactação**. Se for necessário compactar, utilizar o parâmetro `-z`.

```bash
# compactar: -c(create) -z(zip)
tar -cz <nome-do-diretorio>  >  <diretorio-zipado.tar.gz>

# compactar: -c(create) -z(zip) -f(file, compactar para um arquivo)
tar -cz  <diretorio-zipado.tar.gz> <nome-do-diretorio>

#descompatar: -x(extract)
tar -x < <diretorio-zipado.tar.gz>

#descompatar: -x(extract) -f(file, descompactar do arquivo)
tar -xf <diretorio-zipado.tar.gz>
```
