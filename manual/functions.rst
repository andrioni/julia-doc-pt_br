.. _man-functions:

***********
 Funções  
***********

Em Julia, uma função é um objeto que mapeia uma tupla de valores, os
argumentos, a um valor de retorno. As funções, em Julia, são diferentes das
funções matemáticas, pois as funções podem se alterar e afetadas pelo estado
global do programa. A sintaxe básica para definir uma funções em Julia é::

    function f(x,y)
      x + y
    end

Esta sintaxe é similar a do MATLAB, mas há algumas diferenças significativas:

- No MATLAB, esta definição deve ser salvar em um arquivo, nomeado ``f.m``,
  enquanto que que em Julia, esta declaração pode aparecer em qualquer lugar,
  incluindo em uma sessão interativa.
- No MATLAB, a declaração ``end`` final é opcional, sendo implicado pelo fim do
  arquivo. Em Julia, essa declaração ``end`` é obrigatória.
- No MATLAB, esta função irá imprimir o valor ``x + y`` mas não retornará
  nenhum valor, enquanto que em Julia, a última expressão avaliada é o valor de
  retorno da função.
- Os valores de uma expressão nunca são mostrados automaticamente exceto em
  sessões interativas. Ponto-e-vírgula são exigidos somente para separar
  expressões na mesma linha.

Geralmente, enquanto a sintaxe da definição de função é remanescente do MATLAB,
a similaridade é apenas superficial. Logo, ao invés de continuar comparando as
duas, a seguir, nós simplesmente descreveremos o comportamento das funções em
Julia.

Existe uma forma mais compacta de definir uma função em Julia.  A sintaxe
tradicional de declaração de função apresentada acima é equivalente a forma
compactada a seguir::

    f(x,y) = x + y

Nessa forma compacta, o corpo da função deve ser uma única expressão, embora
possa ser uma expressão composta (veja :ref:`man-compound-expressions`).
Definições de funções de forma curta e simples são comuns em Julia. A sintaxe
curta da função é bastante idiomática, reduzindo consideravelmente a digitação
e a poluição visual.

Uma função é chamada usando a sintaxe tradicional de parêntese::

    julia> f(2,3)
    5

Sem parênteses, a expressão ``f`` refere-se ao objeto da função, e pode ser
passada como qualquer valor::

    julia> g = f;

    julia> g(2,3)
    5

Há outras duas maneiras que as funções podem ser aplicadas: usando operadores
com sintaxe especial para certos nomes de funções (veja `Operadores são Funções
<#operators-are-functions>`_), ou com a função ``apply``::

    julia> apply(f,2,3)
    5

A função ``apply`` aplicam seu primeiro argumento - um objeto de função - a
seus argumentos restantes.

.. _man-return-keyword:

A declaração "return"
---------------------

O valor retornado por uma função é o valor da última expressão avaliada, que,
por padrão é a última expressão no corpo da definição da função. Na função de
exemplo, ``f ``, da seção anterior isto é o valor da expressão ``x + y``.
Como em C e na maioria das outras línguas imperativas ou funcionais, a
declaração ``return`` faz com que uma função retorne imediatamente,
fornecendo uma expressão cujo o valor será retornado::

    function g(x,y)
      return x * y
      x + y
    end

Como definições de funções podem ser feitas em sessões interativas, é fácil
comparar estas definições::

    f(x,y) = x + y

    function g(x,y)
      return x * y
      x + y
    end

    julia> f(2,3)
    5

    julia> g(2,3)
    6

Naturalmente, em uma função cujo corpo é linear como ``g``, o uso do ``return``
é injustificado pois a expressão ``x + y`` nunca é avaliada e nós poderíamos
simplesmente tornar ``x * y`` a última expressão na função e omitir ``return``.
Já em conjunto com outras declarações de controle deo fluxo, contudo, o
``return`` é do uso real. A seguir, por exemplo, está uma funciona que calcula
o comprimento da hipotenusa de um triângulo retângulo com lados de comprimento
*x* e *y*, evitando *overflow*::

    function hypot(x,y)
      x = abs(x)
      y = abs(y)
      if x > y
        r = y/x
        return x*sqrt(1+r*r)
      end
      if y == 0
        return zero(x)
      end
      r = x/y
      return y*sqrt(1+r*r)
    end

Há três possíveis pontos de retorno nesta função, retornando os valores de três
expressões diferentes, dependendo dos valores de *x* e *y*. O ``return`` na
última linha podia ser omitido pois ele é o último expressão.

Operadores são funções
----------------------

Em Julia, a maioria dos operadores são apenas funções com suport para sintaxe
especial. As exceções são operadores com semântica especial como o ``&&`` e
``||``. Estes operadores não podem ser funções pois o *short-circuit
evaluation* (veja :ref:`man-short-circuit-evaluation`) exige que seus operandos
não sejam avaliados antes da avaliação do operador.  Logo, você também pode
aplicá-los usam uma lista de argumento entre parênteses, de forma semelhante
como qualquer outra função::

    julia> 1 + 2 + 3
    6

    julia> +(1,2,3)
    6

A forma infixa é exatamente equivalente a forma padrão - na verdade a primeira
forma é convertida para uma chamada de função internamente.  Isto significa que
você também pode atribuir e passar operadores como ``+`` e ``*`` da mesma forma
como você faria para outra função::

    julia> f = +;

    julia> f(1,2,3)
    6

Sob o nome ``f ``, a função suporta a forma infixa.

.. _man-anonymous-functions:

Anonymous Functions
-------------------

Functions in Julia are first-class objects: they can be assigned to
variables, called using the standard function call syntax from the
variable they have been assigned to. They can be used as arguments, and
they can be returned as values. They can also be created anonymously,
without giving them a name::

    julia> x -> x^2 + 2x - 1
    #<function>

This creates an unnamed function taking one argument and returning the
value of the polynomial *x*\ ^2 + 2\ *x* - 1 at that value. The primary
use for anonymous functions is passing them to functions which take
other functions as arguments. A classic example is the ``map`` function,
which applies a function to each value of an array and returns a new
array containing the resulting values::

    julia> map(round, [1.2,3.5,1.7])
    3-element Float64 Array:
     1.0
     4.0
     2.0

This is fine if a named function effecting the transform one wants
already exists to pass as the first argument to ``map``. Often, however,
a ready-to-use, named function does not exist. In these situations, the
anonymous function construct allows easy creation of a single-use
function object without needing a name::

    julia> map(x -> x^2 + 2x - 1, [1,3,-1])
    3-element Int64 Array:
     2
     14
     -2

An anonymous function accepting multiple arguments can be written using
the syntax ``(x,y,z)->2x+y-z``. A zero-argument anonymous function is
written as ``()->3``. The idea of a function with no arguments may seem
strange, but is useful for "delaying" a computation. In this usage, a
block of code is wrapped in a zero-argument function, which is later
invoked by calling it as ``f()``.

Multiple Return Values
----------------------

In Julia, one returns a tuple of values to simulate returning multiple
values. However, tuples can be created and destructured without needing
parentheses, thereby providing an illusion that multiple values are
being returned, rather than a single tuple value. For example, the
following function returns a pair of values::

    function foo(a,b)
      a+b, a*b
    end

If you call it in an interactive session without assigning the return
value anywhere, you will see the tuple returned::

    julia> foo(2,3)
    (5,6)

A typical usage of such a pair of return values, however, extracts each
value into a variable. Julia supports simple tuple "destructuring" that
facilitates this::

    julia> x, y = foo(2,3);

    julia> x
    5

    julia> y
    6

You can also return multiple values via an explicit usage of the
``return`` keyword::

    function foo(a,b)
      return a+b, a*b
    end

This has the exact same effect as the previous definition of ``foo``.

Varargs Functions
-----------------

It is often convenient to be able to write functions taking an arbitrary
number of arguments. Such functions are traditionally known as "varargs"
functions, which is short for "variable number of arguments". You can
define a varargs function by following the last argument with an
ellipsis::

    bar(a,b,x...) = (a,b,x)

The variables ``a`` and ``b`` are bound to the first two argument values
as usual, and the variable ``x`` is bound to an iterable collection of
the zero or more values passed to ``bar`` after its first two arguments::

    julia> bar(1,2)
    (1,2,())

    julia> bar(1,2,3)
    (1,2,(3,))

    julia> bar(1,2,3,4)
    (1,2,(3,4))

    julia> bar(1,2,3,4,5,6)
    (1,2,(3,4,5,6))

In all these cases, ``x`` is bound to a tuple of the trailing values
passed to ``bar``.

On the flip side, it is often handy to "splice" the values contained in
an iterable collection into a function call as individual arguments. To
do this, one also uses ``...`` but in the function call instead::

    julia> x = (3,4)
    (3,4)

    julia> bar(1,2,x...)
    (1,2,(3,4))

In this case a tuple of values is spliced into a varargs call precisely
where the variable number of arguments go. This need not be the case,
however::

    julia> x = (2,3,4)
    (2,3,4)

    julia> bar(1,x...)
    (1,2,(3,4))

    julia> x = (1,2,3,4)
    (1,2,3,4)

    julia> bar(x...)
    (1,2,(3,4))

Furthermore, the iterable object spliced into a function call need not
be a tuple::

    julia> x = [3,4]
    2-element Int64 Array:
     3
     4

    julia> bar(1,2,x...)
    (1,2,(3,4))

    julia> x = [1,2,3,4]
    4-element Int64 Array:
     1
     2
     3
     4

    julia> bar(x...)
    (1,2,(3,4))

Also, the function that arguments are spliced into need not be a varargs
function (although it often is)::

    baz(a,b) = a + b

    julia> args = [1,2]
    2-element Int64 Array:
     1
     2

    julia> baz(args...)
    3

    julia> args = [1,2,3]
    3-element Int64 Array:
     1
     2
     3

    julia> baz(args...)
    no method baz(Int64,Int64,Int64)

As you can see, if the wrong number of elements are in the spliced
container, then the function call will fail, just as it would if too
many arguments were given explicitly.

Optional Arguments
------------------

In many cases, function arguments have sensible default values and therefore
might not need to be passed explicitly in every call. For example, the
library function ``parseint(num,base)`` interprets a string as a number
in some base. The ``base`` argument defaults to ``10``. This behavior can be
expressed concisely as::

    function parseint(num, base=10)
        ###
    end

With this definition, the function can be called with either one or two
arguments, and ``10`` is automatically passed when a second argument is not
specified::

    julia> parseint("12",10)
    12

    julia> parseint("12",3)
    5

    julia> parseint("12")
    12

Optional arguments are actually just a convenient syntax for writing
multiple method definitions with different numbers of arguments
(see :ref:`man-methods`).


Named Arguments
---------------

Some functions need a large number of arguments, or have a large number of
behaviors. Remembering how to call such functions can be difficult. Named
arguments, also called keyword arguments, can make these complex interfaces
easier to use and extend by allowing arguments to be identified by name
instead of only by position.

For example, consider a function ``plot`` that
plots a line. This function might have many options, for controlling line
style, width, color, and so on. If it accepts named arguments, a possible
call might look like ``plot(x, y, width=2)``, where we have chosen to
specify only line width. Notice that this serves two purposes. The call is
easier to read, since we can label an argument with its meaning. It also
becomes possible to pass any subset of a large number of arguments, in
any order.

Functions with named arguments are defined using a semicolon in the
signature::

    function plot(x, y; style="solid", width=1, color="black")
        ###
    end

Extra named arguments can be collected using ``...``, as in varargs
functions::

    function f(x; args...)
        ###
    end

Inside ``f``, ``args`` will be a collection of ``(key,value)`` tuples,
where each ``key`` is a symbol. Such collections can be passed as named
arguments using a semicolon in a call, ``f(x; k...)``. Dictionaries
can be used for this purpose.


Block Syntax for Function Arguments
-----------------------------------

Passing functions as arguments to other functions is a powerful technique,
but the syntax for it is not always convenient. Such calls are especially
awkward to write when the function argument requires multiple lines. As
an example, consider calling ``map`` on a function with several cases::

    map(x->begin
               if x < 0 && iseven(x)
                   return 0
               elseif x == 0
                   return 1
               else
                   return x
               end
           end,
        [A, B, C])

Julia provides a reserved word ``do`` for rewriting this code more clearly::

    map([A, B, C]) do x
        if x < 0 && iseven(x)
            return 0
        elseif x == 0
            return 1
        else
            return x
        end
    end

The ``do x`` syntax creates an anonymous function with argument ``x`` and
passes it as the first argument to ``map``. This syntax makes it easier to
use functions to effectively extend the language, since calls look like
normal code blocks. There are many possible uses quite different from ``map``,
such as managing system state. For example, the standard library provides
a function ``cd`` for running code in a given directory, and switching back
to the previous directory when the code finishes or aborts. There is also
a definition of ``open`` that runs code ensuring that the opened file is
eventually closed. We can combine these functions to safely write a file
in a certain directory::

    cd("data") do
        open("outfile", "w") do f
            write(f, data)
        end
    end

The function argument to ``cd`` takes no arguments; it is just a block of
code. The function argument to ``open`` receives a handle to the opened
file.


Further Reading
---------------

We should mention here that this is far from a complete picture of
defining functions. Julia has a sophisticated type system and allows
multiple dispatch on argument types. None of the examples given here
provide any type annotations on their arguments, meaning that they are
applicable to all types of arguments. The type system is described in
:ref:`man-types` and defining a function in terms of methods chosen
by multiple dispatch on run-time argument types is described in
:ref:`man-methods`.
