README da Documentação de Julia
===============================

A documentação de Julia está escrita em reStructuredText, e uma boa referência 
é o capítulo [Documenting Python](http://docs.python.org/devguide/documenting.html)
do Python Developer's Guide.

Compilando a documentação
-------------------------

A documentação é compilada usando o [Sphinx](http://sphinx.pocoo.org/) e LaTeX.
No Ubuntu, você precisa ter os seguintes pacotes instalados:

    python-sphinx
    texlive
    texlive-latex-extra

E então rodar

    $ make helpdb.jl
    $ make html
    $ make latexpdf

Estrutura dos arquivos
----------------------

    conf.py             Configuração do Sphinx
    helpdb.jl           REPL help database
    sphinx/             Extensões e plugins do Sphinx
    sphinx/jlhelp.py    Plugin Sphinx para compilar helpdb.jl
    stdlib/             Documentação da biblioteca padrão de Julia
    _themes/            Temas html do Sphinx

README da tradução brasileira
=============================

Termos em língua extrangeira devem estar em itálico.

Na documentação existem alguns links para a Wikipédia inglesa. Esses links
devem ser mantidos e adicionados os correspondentes na Wikipédia portuguesa,
preferencialmente na forma de notas de rodapé.

Ao traduzir parte da documentação, preferencialmente utilize a lista, em ordem
alfabética dos termos em inglês, a seguir para alguns termos técnicos que não
possuem tradução para o português.

+--------------------------------------+--------------------------------------+
+ Inglês                               + Português                            +
+--------------------------------------+--------------------------------------+
+ API                                  + *API*                                +
+--------------------------------------+--------------------------------------+
+ features                             + características                      +
+--------------------------------------+--------------------------------------+
+ departures                           + características                      +
+--------------------------------------+--------------------------------------+
+ environment                          + ambiente                             +
+--------------------------------------+--------------------------------------+
+ just-in-time                         + *just-in-time*                       +
+--------------------------------------+--------------------------------------+
+ metaprogramming                      + *metaprogramming*                    +
+--------------------------------------+--------------------------------------+
+ multiple dispatch                    + *multiple dispatch*                  +
+--------------------------------------+--------------------------------------+
+ performance                          + desempenho                           +
+--------------------------------------+--------------------------------------+
+ prototyping                          + experimentação                       +
+--------------------------------------+--------------------------------------+
+ wrapper                              + *wrapper*                            +
+--------------------------------------+--------------------------------------+

Julia Documentation README
==========================

Julia's documentation is written in reStructuredText, a good reference for which
is the [Documenting Python](http://docs.python.org/devguide/documenting.html)
chapter of the Python Developer's Guide.


Building the documentation
--------------------------

The documentation is built using [Sphinx](http://sphinx.pocoo.org/) and LaTeX.
On ubuntu, you'll need the following packages installed:

    python-sphinx
    texlive
    texlive-latex-extra

Then run

    $ make helpdb.jl
    $ make html
    $ make latexpdf


File layout
-----------

    conf.py             Sphinx configuration
    helpdb.jl           REPL help database
    sphinx/             Sphinx extensions and plugins
    sphinx/jlhelp.py    Sphinx plugin to build helpdb.jl
    stdlib/             Julia standard library documentation
    _themes/            Sphinx html themes

