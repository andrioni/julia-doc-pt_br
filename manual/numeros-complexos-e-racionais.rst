.. _man-numeros-complexos-e-racionais:

******************************
 Complex and Rational Numbers  
******************************

Julia comporta tipos pré-definidos que representam tanto números complexos
e racionais como também as operações matemáticas discutidas em :ref:`man-mathematical-operations`.
O desenvolvimento foi definido de modo que as operações em qualquer combinação de tipos numéricos
prédefinidos como primitivo ou composto, se comportem como esperado.

.. _man-complex-numbers:

Números Complexos
---------------

A constante global ``im`` está ligado ao número complexo *i*, representando
uma das raízes quadradas de -1. Foi considerado prejudicial para cooptar o
nome ``i`` para uma constante global, uma vez que é popular nome de variável
de índice. Desde que Julia permitiu literais numéricos para ser :ref:`justapostos
como identificadores de coeficientes <man-numeric-literal-coefficients>`, isto é
suficiente para fornecer ligação sintaxe conveniente para números complexos,
semelhante à notação matemática tradicional.::

    julia> 1 + 2im
    1 + 2im

Você pode executar todas as operações aritmêticas padrões com
números complexos::

    julia> (1 + 2im)*(2 - 3im)
    8 + 1im

    julia> (1 + 2im)/(1 - 2im)
    -0.6 + 0.8im

    julia> (1 + 2im) + (1 - 2im)
    2 + 0im

    julia> (-3 + 2im) - (5 - 1im)
    -8 + 3im

    julia> (-1 + 2im)^2
    -3 - 4im

    julia> (-1 + 2im)^2.5
    2.729624464784009 - 6.9606644595719im

    julia> (-1 + 2im)^(1 + 1im)
    -0.27910381075826657 + 0.08708053414102428im

    julia> 3(2 - 5im)
    6 - 15im

    julia> 3(2 - 5im)^2
    -63 - 60im

    julia> 3(2 - 5im)^-1.0
    0.20689655172413793 + 0.5172413793103449im

O mecanismo de desenvolvimento permite que as combinações de operandos
trabalhe com diferentes tipos::

    julia> 2(1 - 1im)
    2 - 2im

    julia> (2 + 3im) - 1
    1 + 3im

    julia> (1 + 2im) + 0.5
    1.5 + 2.0im

    julia> (2 + 3im) - 0.5im
    2.0 + 2.5im

    julia> 0.75(1 + 2im)
    0.75 + 1.5im

    julia> (2 + 3im) / 2
    1.0 + 1.5im

    julia> (1 - 3im) / (2 + 2im)
    -0.5 - 1.0im

    julia> 2im^2
    -2 + 0im

    julia> 1 + 3/4im
    1.0 - 0.75im

Note que ``3/4im == 3/(4*im) == -(3/4*im)``, uma vez que um coeficiente
literal se liga mais fortemente com a divisão.

As funções padrões para manipular valores complexos são fornecidos::

    julia> real(1 + 2im)
    1

    julia> imag(1 + 2im)
    2

    julia> conj(1 + 2im)
    1 - 2im

    julia> abs(1 + 2im)
    2.23606797749979

    julia> abs2(1 + 2im)
    5

Como é comum, o valor absoluto de um número complexo é próximo a zero.
A função ``abs2``dá o quadrado do valor absoluto, e é de uso particular
para os números complexos, onde ele evita uma raiz quadrada. A gama 
completa de outras funções matemáticas são igualmente definido por
números complexos ::

    julia> sqrt(im)
    0.7071067811865476 + 0.7071067811865475im

    julia> sqrt(1 + 2im)
    1.272019649514069 + 0.7861513777574233im

    julia> cos(1 + 2im)
    2.0327230070196656 - 3.0518977991517997im

    julia> exp(1 + 2im)
    -1.1312043837568138 + 2.471726672004819im

    julia> sinh(1 + 2im)
    -0.48905625904129374 + 1.4031192506220407im

Note que as funções matemáticas tipicamente retornam valores reais. Quando
se aplica em números complexos os valores são retornados em números complexos
reais. Por exemplo ``sqrt``, por exemplo, se comporta de modo diferente quando aplicado a
``-1``contra ``-1 + 0im`` mesmo que ``-1 == -1 + 0im``::

    julia> sqrt(-1)
    ERROR: DomainError()
     in sqrt at math.jl:111

    julia> sqrt(-1 + 0im)
    0.0 + 1.0im

Se você precisar construir um número complexo usando variáveis, o coeficiente
literal de notação numérica não irá funcionar, embora escrito explicitamente a
operação de multiplicaçõa será::

    julia> a = 1; b = 2; a + b*im
    1 + 2im

Não é recomendado a construção de números complexos a partir de valores de
variáveis como visualizado. Para isto use a função ``complex`` para 
construir um valor complexo diretamente de suas partes reais e imaginárias.
Esta construção é preferida para variáveis de argumentos porque é mais 
eficiente do que multiplicação e adição, também porque certos valores de
``b`` podem produzir resultados inesperados::

    julia> complex(a,b)
    1 + 2im

``Inf`` e ``NaN``propagam números complexos imaginários e reais como
especificado pela aritmética IEEE-754::

    julia> 1 + Inf*im
    complex(1.0,Inf)

    julia> 1 + NaN*im
    complex(1.0,NaN)


.. _man-rational-numbers:

Números Racionais
----------------

Julia tem um tipo de número racional próprio para representar proporções
exatas de números inteiros.
Os números racionais são construídos utilizando o operador ``//``::

    julia> 2//3
    2//3

Se o numerador e o denominador de um número racional tem fatores comuns, eles
são reduzidos a termos mais baixos de tal forma que o denominador não é negativo::

    julia> 6//9
    2//3

    julia> -4//8
    -1//2

    julia> 5//-15
    -1//3

    julia> -4//-12
    1//3

Esta forma de normalização para uma relação de inteiros é única, de modo
a igualdade de valores racionais pode ser testado através da verificação
da igualdade do numerador e denominador. O numerador e o denominador padronizado
de um valor racional pode ser extraído usando as funções ``num`` e ``den``:

    julia> num(2//3)
    2

    julia> den(2//3)
    3

A comparação direta entre o numerador e o denominador não é geralmente
necessário, uma vez que as operações aritméticas e a comparação são
normalizados e definidos para valores racionais::

    julia> 2//3 == 6//9
    true

    julia> 2//3 == 9//27
    false

    julia> 3//7 < 1//2
    true

    julia> 3//4 > 2//3
    true

    julia> 2//4 + 1//6
    2//3

    julia> 5//12 - 1//4
    1//6

    julia> 5//8 * 3//12
    5//32

    julia> 6//5 / 10//7
    21//25

Racionais podem ser facilmente convertidos para números de ponto flutuante ::

    julia> float(3//4)
    0.75

Conversão de racional para ponto flutuante respeita a seguinte identidadde
para quaisquer valores integrais de ``a`` e ``b``, com exceção do caso
``a == 0`` e ``b == 0``::

    julia> isequal(float(a//b), a/b)
    true

A construção de infinitos valores racionais é aceitável::

    julia> 5//0
    Inf

    julia> -3//0
    -Inf

    julia> typeof(ans)
    Rational{Int64}

Porém a construção de um valor NaN racional não é aceitável::

    julia> 0//0
    invalid rational: 0//0

Como de costume, o sistema faz interações com outros tipos 
numéricos::

    julia> 3//5 + 1
    8//5

    julia> 3//5 - 0.5
    0.1

    julia> 2//7 * (1 + 2im)
    2//7 + 4//7im

    julia> 2//7 * (1.5 + 2im)
    0.42857142857142855 + 0.5714285714285714im

    julia> 3//2 / (1 + 2im)
    3//10 - 3//5im

    julia> 1//2 + 2im
    1//2 + 2//1im

    julia> 1 + 2//3im
    1//1 + 2//3im

    julia> 0.5 == 1//2
    true

    julia> 0.33 == 1//3
    false

    julia> 0.33 < 1//3
    true

    julia> 1//3 - 0.33
    0.0033333333333332993

