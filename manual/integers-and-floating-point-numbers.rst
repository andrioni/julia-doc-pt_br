.. _man-integers-and-floating-point-numbers:

***************************************
 Números Inteiros e de Ponto Flutuante  
***************************************

Valores inteiros e de ponto flutuante são as fundações da aritmética
e computação. Representações embutidas de tais valores são chamadas
de primitivas numérica, enquanto representações de inteiros e
de números de ponto flutuante como valores imediatos no código 
são conhecidas como literais numéricos. Por exemplo, ``1`` é um 
literal numérico, enquanto ``1.0`` é um literal de ponto flutuante;
suas representações binárias na memória como objetos são primitivas
numéricas. Julia provê uma grande amplitude de tipos primitivos
numéricos, e um conjunto completo de operadores aritméticos e bit a 
bit, e também funções matemáticas padrões, são definidas sobre eles.
A seguir são apresentados os tipos numéricos primitvos de Julia: 

-  **Tipos de inteiros:**

   -  ``Int8`` — inteiros 8-bit com sinal variando de -2^7 a 2^7 - 1.
   -  ``Int8`` — inteiros 8-bit sem sinal variando de 0 a 2^8 - 1.
   -  ``Int16`` — inteiros 16-bit com sinal variando de -2^15 a 2^15 - 1.
   -  ``Int16`` — inteiros 16-bit sem sinal variando de 0 a 2^16 - 1.
   -  ``Int32`` — inteiros 32-bit com sinal variando de -2^31 a 2^31 - 1.
   -  ``Int32`` — inteiros 32-bit sem sinal variando de 0 a 2^32 - 1.
   -  ``Int64`` — inteiros 64-bit com sinal variando de -2^63 a 2^63 - 1.
   -  ``Int64`` — inteiros 64-bit sem sinal variando de 0 a 2^64 - 1.
   -  ``Int128`` — inteiros 128-bit com sinal variando de -2^127 a 2^127 - 1.
   -  ``Int128`` — inteiros 128-bit sem sinal variando de 0 a 2^128 - 1.
   -  ``Bool`` — valendo ou ``true`` (verdadeiro) ou ``false`` (falso),
      que correspondem numericamente a 1 ou 0, respectivamente.
   -  ``Char`` — um tipo numérico de 32 bits representando um `caracter
      Unicode <http://en.wikipedia.org/wiki/Unicode>`_ (veja
      :ref:`man-strings` para mais detalhes).

-  **Tipos de ponto flutuante:**

   -  ``Float32`` — `Números de ponto flutuante 32-bit seguindo o padrão
      IEEE 754
      <http://en.wikipedia.org/wiki/Single_precision_floating-point_format>`_.
   -  ``Float64`` — `Números de ponto flutuante 64-bit seguindo o padrão 
      IEEE 754 <http://en.wikipedia.org/wiki/Double_precision_floating-point_format>`_.


Adicionalmente, estruturas para :ref:`man-complex-and-rational-numbers` 
são construídas sobre esses tipos numéricos primitivos. Todos os tipos
numéricos interoperam naturalmente sem conversão de tipos explícita, 
graças a um sistem flexível de promoção de tipos. Esse sistema, 
detalhado em :ref:`man-conversion-and-promotion`, pode ser estendido,
possibilitando que tipos numéricos definidos pelos usuários possam 
interoperar tão naturalmente quanto os tipos embutidos.

Inteiros
--------

Literais inteiros são representados da maneira padrão::

    julia> 1
    1

    julia> 1234
    1234

O tipo padrão para um literal inteiro depende do sistema, isto é, se 
ele usa uma arquitetura de 32 bits ou de 64 bits::

    # sistema 32-bit:
    julia> typeof(1)
    Int32

    # sistema 64-bit:
    julia> typeof(1)
    Int64

Use ``WORD_SIZE`` para descobrir se um sistema é de 32 ou 64 bits.
O tipo ``Int`` é um *alias* para o tipo inteiro nativo do sistema::

    # sistema 32-bit:
    julia> Int
    Int32

    # sistema 64-bit:
    julia> Int
    Int64

Similarmente, ``Uint`` é um *alias* para o tipo inteiro sem sinal 
nativo do sistema::

    # sistema 32-bit:
    julia> Uint
    Uint32

    # sistema 64-bit:
    julia> Uint
    Uint64

Literais inteiros que não conseguem ser representados usando somente 32
bits, mas podem ser representados com 64 bits sempre criam inteiros de
64 bits, independentemente do tipo de sistema::

    # sistema 32-bit ou 64-bit:
    julia> typeof(3000000000)
    Int64

Inteiros sem sinal são inseridos e imprimidos usando o prefixo ``0x``
e com dígitos hexadecimais (base 16) ``0-9a-f`` (você também pode usar
``A-F`` para a inserção). O tamanho do valores sem sinal é determinado
pelo número de dígitos hexadecimais utilizados::

    julia> 0x1
    0x01

    julia> typeof(ans)
    Uint8

    julia> 0x123
    0x0123

    julia> typeof(ans)
    Uint16

    julia> 0x1234567
    0x01234567

    julia> typeof(ans)
    Uint32

    julia> 0x123456789abcdef
    0x0123456789abcdef

    julia> typeof(ans)
    Uint64

Esse comportamento é basedo na observação de que quando uma pessoa usa
literais hexadecimais para valores inteiros, ela tipicamente os usa 
para representar uma sequência de bytes fixa ao invés de apenas um
valor inteiro.

Literais binários e octais também são suportados::

    julia> 0b10
    0x02

    julia> 0o10
    0x08

Os valores mínimos e máximos representáveis dos tipos numéricos 
primitivos (por exemplo, inteiros) são dados pelas funções ``typemin``
(valor mínimo) e ``typemax`` (valor máximo)::

    julia> (typemin(Int32), typemax(Int32))
    (-2147483648,2147483647)

    julia> for T = {Int8,Int16,Int32,Int64,Int128,Uint8,Uint16,Uint32,Uint64,Uint128}
             println("$(lpad(T,6)): [$(typemin(T)),$(typemax(T))]")
           end

       Int8: [-128,127]
      Int16: [-32768,32767]
      Int32: [-2147483648,2147483647]
      Int64: [-9223372036854775808,9223372036854775807]
     Int128: [-170141183460469231731687303715884105728,170141183460469231731687303715884105727]
      Uint8: [0x00,0xff]
     Uint16: [0x0000,0xffff]
     Uint32: [0x00000000,0xffffffff]
     Uint64: [0x0000000000000000,0xffffffffffffffff]
    Uint128: [0x00000000000000000000000000000000,0xffffffffffffffffffffffffffffffff]

Os valores retornados por ``typemin`` e ``typemax`` são sempre do mesmo
tipo dos argumentos dados. A expressão acima usa várias características
que ainda introduziremos, incluindo :ref:`loops for <man-loops>`,
:ref:`man-strings`, and :ref:`man-string-interpolation`, mas deve ser
fácil de entender para alguém com alguma experiência em programação.

Números de Ponto Flutuante
--------------------------

Números literais de ponto flutuante são representados nos seguintes
formatos padrões::

    julia> 1.0
    1.0

    julia> 1.
    1.0

    julia> 0.5
    0.5

    julia> .5
    0.5

    julia> -1.23
    -1.23

    julia> 1e10
    1e+10

    julia> 2.5e-4
    0.00025

The above results are all ``Float64`` values. There is no literal format
for ``Float32``, but you can convert values to ``Float32`` easily::

    julia> float32(-1.5)
    -1.5

    julia> typeof(ans)
    Float32

There are three specified standard floating-point values that do not
correspond to a point on the real number line:

-  ``Inf`` — positive infinity — a value greater than all finite
   floating-point values
-  ``-Inf`` — negative infinity — a value less than all finite
   floating-point values
-  ``NaN`` — not a number — a value incomparable to all floating-point
   values (including itself).

For further discussion of how these non-finite floating-point values are
ordered with respect to each other and other floats, see
:ref:`man-numeric-comparisons`. By the
`IEEE 754 standard <http://en.wikipedia.org/wiki/IEEE_754-2008>`_, these
floating-point values are the results of certain arithmetic operations::

    julia> 1/0
    Inf

    julia> -5/0
    -Inf

    julia> 0.000001/0
    Inf

    julia> 0/0
    NaN

    julia> 500 + Inf
    Inf

    julia> 500 - Inf
    -Inf

    julia> Inf + Inf
    Inf

    julia> Inf - Inf
    NaN

    julia> Inf/Inf
    NaN

The ``typemin`` and ``typemax`` functions also apply to floating-point
types::

    julia> (typemin(Float32),typemax(Float32))
    (-Inf32,Inf32)

    julia> (typemin(Float64),typemax(Float64))
    (-Inf,Inf)

Note that ``Float32`` values have the suffix ``32: ``NaN32``, ``Inf32``, and ``-Inf32``.

Floating-point types also support the ``eps`` function, which gives the
distance between ``1.0`` and the next larger representable
floating-point value::

    julia> eps(Float32)
    1.192092896e-07

    julia> eps(Float64)
    2.22044604925031308e-16

These values are ``2.0^-23`` and ``2.0^-52`` as ``Float32`` and ``Float64``
values, respectively. The ``eps`` function can also take a
floating-point value as an argument, and gives the absolute difference
between that value and the next representable floating point value. That
is, ``eps(x)`` yields a value of the same type as ``x`` such that
``x + eps(x)`` is the next representable floating-point value larger
than ``x``::

    julia> eps(1.0)
    2.22044604925031308e-16

    julia> eps(1000.)
    1.13686837721616030e-13

    julia> eps(1e-27)
    1.79366203433576585e-43

    julia> eps(0.0)
    5.0e-324

As you can see, the distance to the next larger representable
floating-point value is smaller for smaller values and larger for larger
values. In other words, the representable floating-point numbers are
densest in the real number line near zero, and grow sparser
exponentially as one moves farther away from zero. By definition,
``eps(1.0)`` is the same as ``eps(Float64)`` since ``1.0`` is a 64-bit
floating-point value.


Background and References
~~~~~~~~~~~~~~~~~~~~~~~~~

For a brief but lucid presentation of how floating-point numbers are
represented, see John D. Cook's
`article <http://www.johndcook.com/blog/2009/04/06/anatomy-of-a-floating-point-number/>`_
on the subject as well as his
`introduction <http://www.johndcook.com/blog/2009/04/06/numbers-are-a-leaky-abstraction/>`_
to some of the issues arising from how this representation differs in
behavior from the idealized abstraction of real numbers. For an
excellent, in-depth discussion of floating-point numbers and issues of
numerical accuracy encountered when computing with them, see David
Goldberg's paper `What Every Computer Scientist Should Know About
Floating-Point
Arithmetic <http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.102.244&rep=rep1&type=pdf>`_.
For even more extensive documentation of the history of, rationale for,
and issues with floating-point numbers, as well as discussion of many
other topics in numerical computing, see the `collected
writings <http://www.cs.berkeley.edu/~wkahan/>`_ of `William
Kahan <http://en.wikipedia.org/wiki/William_Kahan>`_, commonly known as
the "Father of Floating-Point". Of particular interest may be `An
Interview with the Old Man of
Floating-Point <http://www.cs.berkeley.edu/~wkahan/ieee754status/754story.html>`_.

.. _man_arbitrary_precision_arithmetic:

Arbitrary Precision Arithmetic
------------------------------

To allow computations with arbitrary precision integers and floating point numbers, 
Julia wraps the `GNU Multiple Precision Arithmetic Library, GMP <http://gmplib.org>`_. 
The `BigInt` and `BigFloat` types are available in Julia for arbitrary precision 
integer and floating point numbers respectively. 

Constructors exist to create these types from primitive numerical types, or from ``String``. 
Once created, they participate in arithmetic with all other numeric types thanks to Julia's 
type promotion and conversion mechanism. ::

    julia> BigInt(typemax(Int64)) + 1
    9223372036854775808

    julia> BigInt("123456789012345678901234567890") + 1
    123456789012345678901234567891

    julia> BigFloat("1.23456789012345678901")
    1.23456789012345678901

    julia> BigFloat(2.0^66) / 3
    24595658764946068821.3

    julia> factorial(BigInt(40))
    815915283247897734345611269596115894272000000000

.. _man-numeric-literal-coefficients:

Numeric Literal Coefficients
----------------------------

To make common numeric formulas and expressions clearer, Julia allows
variables to be immediately preceded by a numeric literal, implying
multiplication. This makes writing polynomial expressions much cleaner::

    julia> x = 3
    3

    julia> 2x^2 - 3x + 1
    10

    julia> 1.5x^2 - .5x + 1
    13.0

It also makes writing exponential functions more elegant::

    julia> 2^2x
    64

The precedence of numeric literal coefficients is the same as that of unary
operators such as negation. So ``2^3x`` is parsed as ``2^(3x)``, and
``2x^3`` is parsed as ``2*(x^3)``.

You can also use numeric literals as coefficients to parenthesized
expressions::

    julia> 2(x-1)^2 - 3(x-1) + 1
    3

Additionally, parenthesized expressions can be used as coefficients to
variables, implying multiplication of the expression by the variable::

    julia> (x-1)x
    6

Neither juxtaposition of two parenthesized expressions, nor placing a
variable before a parenthesized expression, however, can be used to
imply multiplication::

    julia> (x-1)(x+1)
    type error: apply: expected Function, got Int64

    julia> x(x+1)
    type error: apply: expected Function, got Int64

Both of these expressions are interpreted as function application: any
expression that is not a numeric literal, when immediately followed by a
parenthetical, is interpreted as a function applied to the values in
parentheses (see :ref:`man-functions` for more about functions).
Thus, in both of these cases, an error occurs since the left-hand value
is not a function.

The above syntactic enhancements significantly reduce the visual noise
incurred when writing common mathematical formulae. Note that no
whitespace may come between a numeric literal coefficient and the
identifier or parenthesized expression which it multiplies.

Syntax Conflicts
~~~~~~~~~~~~~~~~

Juxtaposed literal coefficient syntax conflicts with two numeric literal
syntaxes: hexadecimal integer literals and engineering notation for
floating-point literals. Here are some situations where syntactic
conflicts arise:

-  The hexadecimal integer literal expression ``0xff`` could be
   interpreted as the numeric literal ``0`` multiplied by the variable
   ``xff``.
-  The floating-point literal expression ``1e10`` could be interpreted
   as the numeric literal ``1`` multiplied by the variable ``e10``, and
   similarly with the equivalent ``E`` form.

In both cases, we resolve the ambiguity in favor of interpretation as a
numeric literals:

-  Expressions starting with ``0x`` are always hexadecimal literals.
-  Expressions starting with a numeric literal followed by ``e`` or
   ``E`` are always floating-point literals.

