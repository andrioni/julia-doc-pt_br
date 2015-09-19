.. _man-operacoes-matematicas:

*************************
 Operações Matemáticas  
*************************
Julia oferece uma coleção básica de aritmética e operadores bit a bit,
todos os tipos primitivos numéricos, bem como fornecendo, implementações
eficientes e uma abrangente coleção de funções matemáticas padrão.


Aritmética e operadores bit a bit
--------------------------------

Os seguintes `operadores
aritméticos <https://pt.wikipedia.org/wiki/S%C3%ADmbolos_matem%C3%A1ticos#Operadores_aritm.C3.A9ticos>`_
são suportados em todos os tipos primitivos:

-  ``+x`` — forma de demonstração que o valor da variável x é positiva.
-  ``-x`` — forma de demonstração que o valor da variável x é negativa.
-  ``x + y`` — forma de adição de valores.
-  ``x - y`` — forma de subtração de valores.
-  ``x * y`` — forma de multiplicação de valores.
-  ``x / y`` — forma de divisão de valores.

Os seguintes `operadores
bit a bit <https://pt.wikipedia.org/wiki/L%C3%B3gica_bin%C3%A1ria>`_
são suportados em todos os tipos primitivos inteiros:

-  ``~x`` — forma de operação bit a bit de negação.
-  ``x & y`` — forma de operação bit a bit de AND (e).
-  ``x | y`` — forma de operação bit a bit de OR (ou).
-  ``x $ y`` — forma de operação bit a bit de XOR.
-  ``x >>> y`` — `deslocamento
   lógico <http://en.wikipedia.org/wiki/Logical_shift>`_ para a direira.
-  ``x >> y`` — `deslocamento
   aritmético <http://en.wikipedia.org/wiki/Arithmetic_shift>`_ para a direita.
-  ``x << y`` — deslocamento lógico/aritmético para a esquerda.

Aqui estão alguns exemplos simples usando operadores aritméticos::

    julia> 1 + 2 + 3
    6

    julia> 1 - 2
    -1

    julia> 3*2/12
    0.5

(Por convenção, temos a tendência de adicionar espaço entre os operadores,
porém não há restrições sintáticas para o espaçamento entre os operadores)

A linguagem Julia provê de operações aritméticas que se misturam através de
argumentos "just work" bem naturalmente e automático. Veja em 
:ref:`man-conversion-and-promotion` para mais informações de como é feita a
conversão de tipos.

Aqui estão alguns exemplos com operadores bit a bit::

    julia> ~123
    -124

    julia> 123 & 234
    106

    julia> 123 | 234
    251

    julia> 123 $ 234
    145

    julia> ~uint32(123)
    0xffffff84

    julia> ~uint8(123)
    0x84

Toda aritmética e operação bit a bit também tem uma atualização
de versão que atribui o resultado da operação de volta a sua
esquerda. Por exemplo a forma de atualizar ``+`` is the ``+=``
operador.
Escrevendo ``x += 3`` é equivalente a dizer que ``x = x + 3``::

      julia> x = 1
      1

      julia> x += 3
      4

      julia> x
      4

Aqui estão todas as versões de atualização aritméticos e the bit a bit::

    +=  -=  *=  /=  &=  |=  $=  >>>=  >>=  <<=


.. _man-numeric-comparisons:

Comparação Numérica
-------------------

Operações de comparação padrão são definidas para todos os tipos númericos
primitivos:

-  ``==`` — igualdade.
-  ``!=`` — diferente de.
-  ``<`` — menor que.
-  ``<=`` — menor ou igual que.
-  ``>`` — maior que.
-  ``>=`` — maior ou igual que.

Aqui estão alguns exemplos::

    julia> 1 == 1
    true

    julia> 1 == 2
    false

    julia> 1 != 2
    true

    julia> 1 == 1.0
    true

    julia> 1 < 2
    true

    julia> 1.0 > 3
    false

    julia> 1 >= 1.0
    true

    julia> -1 <= 1
    true

    julia> -1 <= -1
    true

    julia> -1 <= -2
    false

    julia> 3 < -0.5
    false

Inteiros são comparados no modo convencional - por comparação de bits.
Número de ponto flutuante são comparados de acordo to the `IEEE 754
standard <http://en.wikipedia.org/wiki/IEEE_754-2008>`_:

-  números finitos são ordenados da maneira normal.
-  ``Inf`` é igual a si mesmo e maior do que o resto, exceto
   ``NaN``
-  ``-Inf`` é igual a si mesmo e menor do que todos os demais, exceto
   ``NaN``
-  ``NaN`` não é igual, menor ou maior do que todos, incluindo ele próprio.

O último ponto é potencialmente importante, e portanto, digno de nota::

    julia> NaN == NaN
    false

    julia> NaN != NaN
    true

    julia> NaN < NaN
    false

    julia> NaN > NaN
    false

Para situações onde se deseja comparar valores de ponto flutuante para que
``NaN`` é igual a ``NaN``, tais como comparacões chave de hash, a função
``isequal`` também é fornecido, o qual considera ``NaN``\ para ser igual 
entre si::

    julia> isequal(NaN,NaN)
    true

Comparações de tipo inteiro com misto, inteiros sem sinal, e flutuantes
pode ser muito complicado. Foram tomadas grandes observações para assegurar
que Julia faz-se corretamente.
Diferentemente da maioria das linguagesn com a notável exceção do Python
<http://en.wikipedia.org/wiki/Python_syntax_and_semantics#Comparison_operators>`_,
comparações podem ser arbritariamente unidas::

    julia> 1 < 2 <= 2 < 3 == 3 > 2 >= 1 == 1 < 3 != 5
    true

União de comparações muitas vezes é bastante conveniente em códigos numéricos.
Comparações numéricas unidas usa-se o operador ``&``, o que lhes permite
trabalhar em vetores. Por exemplo, ``0 < A < 1`` dá uma matriz de boolean cujo
entradas são verdadeiras, onde os elementos correspondentes do ``A`` estão entre 0
e 1.

Note-se a avaliação do comportamento de união de comparação ::

    v(x) = (println(x); x)

    julia> v(1) < v(2) <= v(3)
    2
    1
    3
    false

A expressão do meio é avaliada somente uma vez, duas vezes, em vez de como
ele seria se a expressão como foram escritas ``v(1) > v(2) & v(2) <= v(3)``.
No entanto, a ordem de avaliações em uma união de comparação é indefinido.
É altamente recomendável não usar expressões com efeitos colaterais
(como impressão) em união de comparações.
Se os efeitos secundários são necessários, o operador ``&&`` que deve-se
ser usado explicitamente (veja :ref:`man-short-circuit-evaluation`).  

Funções Matemáticas
----------------------

Julia oferece uma abrangente coleção de operação e funções matemáticas.
Estas operações matemáticas são definidas sobre mais ampla classe de valores
numéricos, incluindo inteiros, números de ponto flutuante, racionais.

-  ``round(x)`` — arredondamento de ``x`` para o número inteiro mais próximo.
-  ``iround(x)`` — arredondamento de ``x`` para o inteiro mais próximo, dando um
   resultado digitado em inteiro.
-  ``floor(x)`` — arredonda ``x`` em direção a ``-Inf``.
-  ``ifloor(x)`` — arredonda ``x`` em direção ``-Inf``,dando o resultado um número inteiro.
-  ``ceil(x)`` — arredonda ``x`` em direção ``+Inf``.
-  ``iceil(x)`` — arredonda ``x`` em direção ``+Inf``, dando o resultado um número inteiro. 
-  ``trunc(x)`` — arredonda ``x`` em direção a zero.
-  ``itrunc(x)`` — arredonda ``x`` em direção a zero, dando o resultado um número inteiro.
-  ``div(x,y)`` — trunca a divisão; deixando o quociente arredondado a zero.
-  ``fld(x,y)`` — pavimento da divisão; quociente arredondado em direção ``-Inf``.
-  ``rem(x,y)`` — restante da divisão; satisfaz ``x == div(x,y)*y + rem(x,y)``,
   implicando ao sinal que corresponde ``x``.
-  ``mod(x,y)`` — módulo; satisfaz ``x == fld(x,y)*y + mod(x,y)``,
   implicando ao sinal que corresponde ``y``.
-  ``gcd(x,y...)`` — maior divisor comum de ``x``, ``y``... with
   sign matching ``x``.
-  ``lcm(x,y...)`` — minimo múltiplo comum de ``x``, ``y``... with sign
   matching ``x``.
-  ``abs(x)`` — valor positivo com a magnitude de ``x``.
-  ``abs2(x)`` — o quadrado da magnitude de ``x``.
-  ``sign(x)`` — indica o sinal de ``x``, retornando -1, 0, or +1.
-  ``signbit(x)`` — indica se o bit é de sinal (1) de ligado ou (0) de desligado.
-  ``copysign(x,y)`` — valor com magnitude de ``x`` e com sinal 
   de ``y``.
-  ``flipsign(x,y)`` — valor com magnitude de ``x`` e com sinal
   de ``x*y``.
-  ``sqrt(x)`` — raiz quadrade de ``x``.
-  ``cbrt(x)`` — raiz cúbica de ``x``.
-  ``hypot(x,y)`` — exato``sqrt(x^2 + y^2)`` para todos os valores de ``x``
   e ``y``.
-  ``exp(x)`` — função exponencial natural de ``x``.
-  ``expm1(x)`` — exato``exp(x)-1`` de ``x`` perto de zero.
-  ``ldexp(x,n)`` — ``x*2^n`` calculado de forma eficiente para valores inteiros 
  de ``n``.
-  ``log(x)`` — logaritmo natural de ``x``.
-  ``log(b,x)`` — logaritmo de ``x`` com base ``b``.
-  ``log2(x)`` — logaritmo de base 2 de ``x``.
-  ``log10(x)`` — logaritmo de base 10 de ``x``.
-  ``log1p(x)`` — exato ``log(1+x)`` de ``x`` próximo a zero.
-  ``logb(x)`` — retorna o expoente binário de ``x``.
-  ``erf(x)`` — função de 
   erro <http://en.wikipedia.org/wiki/Error_function>`_ de ``x``.
-  ``erfc(x)`` — exato ``1-erf(x)`` para um grande ``x``.
-  ``gamma(x)`` — função
   gama <http://en.wikipedia.org/wiki/Gamma_function>`_ de ``x``.
-  ``lgamma(x)`` — exato ``log(gamma(x))`` para um grande ``x``.

Para uma visão geral de como funciona as funções ``hypot``, ``expm1``, ``log1p``,
e ``erfc`` consulte o post de John D.Cook sobre o assunto `expm1, log1p,
erfc <http://www.johndcook.com/blog/2010/06/07/math-library-functions-that-seem-unnecessary/>`_,
and
`hypot <http://www.johndcook.com/blog/2010/06/02/whats-so-hard-about-finding-a-hypotenuse/>`_.

Todas as funções trigonométricas padrão são definidas como::

    sin    cos    tan    cot    sec    csc
    sinh   cosh   tanh   coth   sech   csch
    asin   acos   atan   acot   asec   acsc
    acoth  asech  acsch  sinc   cosc   atan2

Essas são todas as funções de argumento único, com a exceção de 
`atan2 <http://en.wikipedia.org/wiki/Atan2>`_, o que dá o angulo em 
`radianos <http://en.wikipedia.org/wiki/Radian>`_ entre *x* e o ponto
especificado pelos seus argumentos, interpretados como *x* e *y* coordenada.
Para calcular funções trigonométricas com graus em vez de radianos, sufixo da 
função ``d``. Por exemplo, ``sind(x)`` calcula o sendo de ``x`` onde ``x`` é
especificado em degraus.

Por conveniência de notação, a função ``rem`` tem uma forma de operador:

-  ``x % y`` é equivale a ``rem(x,y)``.

The spelled-out ``rem`` é a forma "canonica", enquanto ``%`` é a forma do operador
que é mantida para compatibilidade com outros sistemas. Como a aritmética dos operadores
bit a bit ``%`` e ``^`` também atualiza formas. Tal como acontece com outras formas de atualização,
``x %= y`` significa ``x = x % y`` e ``x ^= y`` significa ``x = x^y``::

    julia> x = 2; x ^= 5; x
    32

    julia> x = 7; x %= 4; x
    3

