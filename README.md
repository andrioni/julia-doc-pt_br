### A documentação pode ser visualizada online no [ReadTheDocs](https://julia_pt-br.readthedocs.org/en/latest/)!

README da tradução brasileira
=============================

Contribuindo
------------

Contribuições são sempre bem-vindas, e podem ser feitas das seguintes maneiras,
em ordem de praticidade:
- Enviar um pull request diretamente aqui no GitHub. Isto pode ser feito 
  automaticamente se você possuir uma conta no site, basta editar um arquivo
  e enviar.
- Enviar um patch via diff ou git por e-mail em alessandroandrioni@gmail.com
- Enviar um arquivo inteiro traduzido no e-mail acima.

Recomendações
-------------

Para uniformizar e organizar a tradução, há algumas recomendações abaixo.
Todas elas podem ser quebradas caso o bom senso mande, mas em geral é bom
mantê-las para garantir um projeto fácil de ser cuidado e atualizado!

Linhas devem preferencialmente ter 71 caracteres ou menos, para facilitar
a edição do texto nos mais variados programas e celulares/tablets. Se duas
linhas estão em sequência, elas vão ser unidas no mesmo parágrafo 
automaticamente na documentação final.

Termos em língua estrangeira devem estar em itálico.

Na documentação existem alguns links para a Wikipédia inglesa. Esses links
devem ser mantidos e adicionados os correspondentes na Wikipédia portuguesa,
preferencialmente na forma de notas de rodapé.

Ao traduzir parte da documentação, preferencialmente utilize a lista, em ordem
alfabética dos termos em inglês, a seguir para alguns termos técnicos que não
possuem tradução para o português.

| Inglês                               | Português                            |
|--------------------------------------|--------------------------------------|
| API                                  | *API*                                |
| features                             | características                      |
| departures                           | características                      |
| environment                          | ambiente                             |
| just-in-time                         | *just-in-time*                       |
| multiple dispatch                    | *multiple dispatch*                  |
| performance                          | desempenho                           |
| prototyping                          | experimentação                       |
| wrapper                              | *wrapper*                            |
| string                               | *string*                             |
| typing                               | tipagem                              |
| tuple                                | enupla (ou tupla?)                   |
| hash table                           | tabela de espalhamento (ou manter hash table?)  |

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
