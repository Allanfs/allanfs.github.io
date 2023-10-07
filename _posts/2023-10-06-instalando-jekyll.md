---
layout: post
title:  "Instalando o Jekyll no Mac"
date:   2023-10-06 23:15:00 -0300
categories: jekyll install
---

Não seguindo todas as orientações de instalação do Jekyll tentei instala-lo com a versão do ruby e gem que já vem instaladas no Mac. Eis que me deparo com um _amigavel erro_  de permissão negada:

```
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/gems/2.6.0 directory.
```

Bem, vi que mesmo utilizando sudo o erro persistia, por isso pesquisei e descobri que o [próprio Mac não permite fazer alterações na instalação nativa do Ruby](https://stackoverflow.com/a/54873916). E por isso é recomendado utilizar um gerenciador de versão.

Por isso realizei o seguinte:

```sh
brew install chruby ruby-install
```

E adicionei o seguinte ao `~/.zshrc`:
```sh
# chruby
source /opt/homebrew/opt/chruby/share/chruby/chruby.sh
```

Reinicei o terminal e instalei a versão estável do Ruby no momento (3.2.2). Com isso o `gem` foi atualizado também.

Em seguida, segui [os passos de instalação](https://jekyllrb.com/docs/#instructions) e tive o Jekyll funcionando como esperado.