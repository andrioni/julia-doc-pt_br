.. _man-getting-started:

***********
 Começando  
***********

A instalação de Julia é direta, seja com binário pré-compilados, seja
compilando o código-fonte. Faça download e instale Julia seguindo as 
instruções (em inglês) em `http://julialang.org/downloads/ <http://julialang.org/downloads/>`_.

Julia installation is straightforward, whether using precompiled binaries or compiling from source. Download and install Julia by following the instructions at
`http://julialang.org/downloads/ <http://julialang.org/downloads/>`_.

A maneira mais fácil de aprender e experimentar com Julia é iniciando 
sessão interativa (também conhecida como *read-eval-print loop* ou *"repl"*)::

The easiest way to learn and experiment with Julia is by starting an
interactive session (also known as a read-eval-print loop or "repl")::

    $ julia
                   _
       _       _ _(_)_     |
      (_)     | (_) (_)    |  A fresh approach to technical computing.
       _ _   _| |_  __ _   |
      | | | | | | |/ _` |  |  Version 0 (pre-release)
      | | |_| | | | (_| |  |  Commit 61847c5aa7 (2011-08-20 06:11:31)*
     _/ |\__'_|_|_|\__'_|  |
    |__/                   |

    julia> 1 + 2
    3

    julia> ans
    3

Para encerrar a sessão interative, digite ``^D```- a tecla *Ctrl* 
junto da tecla ``d`` ou digite ``quit()``. Quando rodado no modo 
interativo, ``julia`` mostra um banner e espera o usuário digitar.
Uma vez que o usuário digitou uma expressão completa, como `1 + 2`, e
apertou *enter*, a sessão interativa calcula a expressão e mostra o 
seu valor. Se uma expressão é inserida em uma sessão interativa com um
ponto-e-vírgula no final, seu valor não é mostrado. À variável ``ans``
é atribuído o valor da última expressão calculada, tendo sido mostrada
ou não.

To exit the interactive session, type ``^D`` — the control key
together with the ``d`` key or type ``quit()``. When run in interactive
mode, ``julia`` displays a banner and prompts the user for input. Once
the user has entered a complete expression, such as ``1 + 2``, and
hits enter, the interactive session evaluates the expression and shows
its value. If an expression is entered into an interactive session
with a trailing semicolon, its value is not shown. The variable
``ans`` is bound to the value of the last evaluated expression whether
it is shown or not.

Para calcular expressões escritas em um arquivo ``file.jl``, digite
``include("file.jl")``.

To evaluate expressions written in a source file ``file.jl``, write
``include("file.jl")``.

Para rodar código em um arquivo de maneira não-interativa, você pode
usá-lo como o primeiro argumento no comando julia::

To run code in a file non-interactively, you can give it as the first
argument to the julia command::

    $ julia script.jl arg1 arg2...

Como mostra o exemplo, os argumentos da linha de comando subsequentes
são tomados como argumentos para o programa ``script.jl``, passados na
constante global ``ARGS``. ``ARGS`` é também definida quando o código
do *script* é dado usando a opção da linha de comando ``-e`` (veja a saída de 
ajuda de ``julia`` abaixo). Por exemplo, para só imprimir os argumentos
dados a um *script*, você pode fazer::

As the example implies, the following command-line arguments to julia
are taken as command-line arguments to the program ``script.jl``, passed
in the global constant ``ARGS``. ``ARGS`` is also set when script code
is given using the ``-e`` option on the command line (see the ``julia``
help output below). For example, to just print the arguments given to a
script, you could do this::

    $ julia -e 'for x in ARGS; println(x); end' foo bar
    foo
    bar

Ou pode colocar esse código em um *script* e rodá-lo::

Or you could put that code into a script and run it::

    $ echo 'for x in ARGS; println(x); end' > script.jl
    $ julia script.jl foo bar
    foo
    bar

Há várias maneiras de rodar código em Julia e dar opções, semelhantes
àquelas disponívels para os programas ``perl`` e ``ruby``:

There are various ways to run Julia code and provide options, similar to
those available for the ``perl`` and ``ruby`` programs::

    julia [options] [program] [args...]
     -v --version             Display version information
     -q --quiet               Quiet startup without banner
     -H --home=<dir>          Load files relative to <dir>
     -T --tab=<size>          Set REPL tab width to <size>

     -e --eval=<expr>         Evaluate <expr>
     -E --print=<expr>        Evaluate and show <expr>
     -P --post-boot=<expr>    Evaluate <expr> right after boot
     -L --load=file           Load <file> right after boot
     -J --sysimage=file       Start up with the given system image file

     -p n                     Run n local processes
     --machinefile file       Run processes on hosts listed in file

     --no-history             Don't load or save history
     -f --no-startup          Don't load ~/.juliarc.jl
     -F                       Load ~/.juliarc.jl, then handle remaining inputs

     -h --help                Print this message


Tutoriais
---------

Alguns guias passo-a-passo estão disponíveis online:

A few walkthrough-style tutorials are available online:

- `Começando com Julia para usuários de MATLAB <http://www.ime.unicamp.br/~ra092767/tutoriais/julia/>`_
- `Forio Julia Tutorials (em inglês) <http://forio.com/julia/tutorials-list>`_
- `Tutorial for Homer Reid's numerical analysis class (em inglês) <http://homerreid.ath.cx/teaching/18.330/JuliaProgramming.shtml#SimplePrograms>`_

Diferenças nótáveis em relação ao MATLAB
----------------------------------------

Usuários de MATLAB podem achar a sintaxe de Julia familar, porém Julia
não é de maneira alguma um clone de MATLAB: há grandes diferenças
sintáticas e funcionais. Apresentadas a seguir estão algumas 
importantes ressalvas que podem confundir usuários de Julia 
acostumados com MATLAB:

MATLAB users may find Julia's syntax familiar. However,
Julia is in no way a MATLAB clone: there are major syntactic and
functional differences. The following are some noteworthy
differences that may trip up Julia users accustomed to MATLAB:

-  *Arrays* são indexados com colchetes, ``A[i,j]``.
-  A unidade imaginária ``sqrt(-1)`` é representada em Julia com 
   ``im``.
-  Múltiplos valores são retornados e atribuídos com parênteses,
   ``return (a, b)`` e ``(a, b) = f(x)``.
-  Valores são passados e atribuídos por referência. Se uma função 
   modifica um *array*, as mudanças serão visíveis para quem chamou.
-  Julia tem *arrays* unidimensionais. Vetores-coluna são de tamanho 
   ``N``, não ``Nx1``. Por exemplo, ``rand(N)`` cria um array 
   unidimensional.
-  Concatenar escalares e *arrays* com a sintaxe ``[x,y,z]`` concatena
   na primeira dimensão ("verticalmente"). Para a segunda dimensão,
   ("horizontalmente"), use espaços, como em ``[x y z]``. Para 
   construir matrizes em blocos (concatenando nas duas primeiras 
   dimensões), é usada a sintaxe ``[a b; c d]`` para evitar confusão.
-  Dois-pontos ``a:b`` e ``a:b:c`` constroem objetos ``Range``. Para 
   construir um vetor completo, use ``linspace``, ou "concatene" o
   intervalo colocando-o em colchetes, ``[a:b]``.
-  Funções retornam valores usando a palavra-chave ``return``, ao 
   invés de por citações a seus nomes na definição da função (veja
   :ref:`man-return-keyword` para mais detalhes).
-  Um arquivo pode conter um número qualquer de funções, e todas as 
   definições vão ser visíveis de fora quando o arquivo for carregado.
-  Reduções como ``sum``, ``prod``, e ``max`` são feitas sobre cada 
   elemento de um *array* quando chamadas com um único argumento, como
   em ``sum(A)``.
-  Funções como ``sort`` que operam por padrão em colunas
   (``sort(A)`` é equivalente a ``sort(A,1)``) não possuem 
   comportamento especial para *arrays* 1xN; o argumento é retornado
   inalterado, já que a operação feita foi ``sort(A,1)``. Para ordenar
   uma matriz 1xN como um vetor, use ``sort(A,2)``.
-  Parênteses devem ser usados para chamar uma função com zero 
   argumentos, como em``tic()`` and ``toc()``.
-  Não use ponto-e-vírgula para encerrar declarações. Os resultados 
   de declarações não são automaticamente impressos (exceto no prompt 
   interativo), e linhas de código não precisam terminar com 
   ponto-e-vírgula. A função ``println`` pode ser usada para imprimir 
   um valor seguido de uma nova linha.
-  Se ``A`` e ``B`` são *arrays*, ``A == B`` não retorna um *array* de
   booleanos. Use ``A .== B`` no lugar. O mesmo vale para outros 
   operaores booleanos, ``<``, ``>``, ``!=``, etc.
-  Os elementos de uma coleção podem ser passados como argumentos para
   uma função usando ``...``, como em ``xs=[1,2]; f(xs...)``.
-  A função ``svd`` de Julia retorna os valores singulares como um
   vetor, e não como uma matriz diagonal.

-  Arrays are indexed with square brackets, ``A[i,j]``.
-  The imaginary unit ``sqrt(-1)`` is represented in julia with ``im``.
-  Multiple values are returned and assigned with parentheses,
   ``return (a, b)`` and ``(a, b) = f(x)``.
-  Values are passed and assigned by reference. If a function modifies
   an array, the changes will be visible in the caller.
-  Julia has 1-dimensional arrays. Column vectors are of size ``N``, not
   ``Nx1``. For example, ``rand(N)`` makes a 1-dimensional array.
-  Concatenating scalars and arrays with the syntax ``[x,y,z]``
   concatenates in the first dimension ("vertically"). For the second
   dimension ("horizontally"), use spaces as in ``[x y z]``. To
   construct block matrices (concatenating in the first two dimensions),
   the syntax ``[a b; c d]`` is used to avoid confusion.
-  Colons ``a:b`` and ``a:b:c`` construct ``Range`` objects. To
   construct a full vector, use ``linspace``, or "concatenate" the range
   by enclosing it in brackets, ``[a:b]``.
-  Functions return values using the ``return`` keyword, instead of by
   listing their names in the function definition (see
   :ref:`man-return-keyword` for details).
-  A file may contain any number of functions, and all definitions will
   be externally visible when the file is loaded.
-  Reductions such as ``sum``, ``prod``, and ``max`` are performed over
   every element of an array when called with a single argument as in
   ``sum(A)``.
-  Functions such as ``sort`` that operate column-wise by default
   (``sort(A)`` is equivalent to ``sort(A,1)``) do not have special
   behavior for 1xN arrays; the argument is returned unmodified since it
   still performs ``sort(A,1)``. To sort a 1xN matrix like a vector, use
   ``sort(A,2)``.
-  Parentheses must be used to call a function with zero arguments, as
   in ``tic()`` and ``toc()``.
-  Do not use semicolons to end statements. The results of statements are
   not automatically printed (except at the interactive prompt), and
   lines of code do not need to end with semicolons. The function
   ``println`` can be used to print a value followed by a newline.
-  If ``A`` and ``B`` are arrays, ``A == B`` doesn't return an array of
   booleans. Use ``A .== B`` instead. Likewise for the other boolean
   operators, ``<``, ``>``, ``!=``, etc.
-  The elements of a collection can be passed as arguments to a function
   using ``...``, as in ``xs=[1,2]; f(xs...)``.
-  Julia's ``svd`` returns singular values as a vector instead of as a
   full diagonal matrix.

Noteworthy differences from R
-----------------------------

One of Julia's goals is to provide an effective language for data analysis and statistical programming. For users coming to Julia from R, these are some noteworthy differences:

- Julia uses ``=`` for assignment. Julia does not provide any operator like ``<-`` or ``<-``.
- Julia constructs vectors using brackets. Julia's ``[1, 2, 3]`` is the equivalent of R's ``c(1, 2, 3)``.
- Julia's matrix operations are more like traditional mathematical notation than R's. If ``A`` and ``B`` are matrices, then ``A * B`` defines a matrix multiplication in Julia equivalent to R's ``A %*% B``. In R, this some notation would perform an elementwise Hadamard product. To get the elementwise multiplication operation, you need to write ``A .* B`` in Julia.
- Julia performs matrix transposition using the ``'`` operator. Julia's ``A'`` is therefore equivalent to R's ``t(A)``.
- Julia does not require parentheses when writing ``if`` statements or ``for`` loops: use ``for i in [1, 2, 3]`` instead of ``for (i in c(1, 2, 3))`` and ``if i == 1`` instead of ``if (i == 1)``.
- Julia does not treat the numbers ``0`` and ``1`` as Booleans. You cannot write ``if (1)`` in Julia, because ``if`` statements accept only booleans. Instead, you can write ``if true``.
- Julia does not provide ``nrow`` and ``ncol``. Instead, use ``size(M, 1)`` for ``nrow(M)`` and ``size(M, 2)`` for ``ncol(M)``.
- Julia's SVD is not thinned by default, unlike R. To get results like R's, you will often want to call ``svd(X, true)`` on a matrix ``X``.
- Julia is very careful to distinguish scalars, vectors and matrices. In R, ``1`` and ``c(1)`` are the same. In Julia, they can not be used interchangeably. One potentially confusing result of this is that ``x' * y`` for vectors ``x`` and ``y`` is a 1-element vector, not a scalar. To get a scalar, use ``dot(x, y)``.
- Julia's ``diag()`` and ``diagm()`` are not like R's.
- Julia cannot assign to the results of function calls on the left-hand of an assignment operation: you cannot write ``diag(M) = ones(n)``.
- Julia discourages populating the main namespace with functions. Most statistical functionality for Julia is found in `packages <http://docs.julialang.org/en/latest/packages/packagelist/>`_ like the DataFrames and Distributions packages:
	- Distributions functions are found in the `Distributions package <https://github.com/JuliaStats/Distributions.jl>`_
	- The `DataFrames package <https://github.com/HarlanH/DataFrames.jl>`_ provides data frames.
	- Formulas for GLM's must be escaped: use ``:(y ~ x)`` instead of ``y ~ x``.
- Julia provides tuples and real hash tables, but not R's lists. When returning multiple items, you should typically use a tuple: instead of ``list(a = 1, b = 2)``, use ``(1, 2)``. 
- Julia encourages all users to write their own types. Julia's types are much easier to use than S3 or S4 objects in R. Julia's multiple dispatch system means that ``table(x::TypeA)`` and ``table(x::TypeB)`` act like R's ``table.TypeA(x)`` and ``table.TypeB(x)``.
- In Julia, values are passed and assigned by reference. If a function modifies an array, the changes will be visible in the caller. This is very different from R and allows new functions to operate on large data structures much more efficiently.
- Concatenation of vectors and matrices is done using ``hcat`` and ``vcat``, not ``c``, ``rbind`` and ``cbind``.
- A Julia range object like ``a:b`` is not shorthand for a vector like in R, but is a specialized type of object that is used for iteration without high memory overhead. To convert a range into a vector, you need to wrap the range with brackets ``[a:b]``.
- Julia has several functions that can mutate their arguments. For example, it has ``sort(v)`` and ``sort!(v)``.
- ``colMeans()`` and ``rowMeans()``, ``size(m, 1)`` and ``size(m, 2)``
- In R, performance requires vectorization. In Julia, almost the opposite is true: the best performing code is often achieved by using devectorized loops.
- Unlike R, there is no delayed evaluation in Julia. For most users, this means that there are very few unquoted expressions or column names.
- Julia does not ``NULL`` type.
- There is no equivalent of R's ``assign`` or ``get`` in Julia.
