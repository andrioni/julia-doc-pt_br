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
exemplo, ``f``, da seção anterior isto é o valor da expressão ``x + y``.
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

Sob o nome ``f``, a função suporta a forma infixa.

.. _man-anonymous-functions:

Funções Anónimas
----------------

Funções em Julia são objetos de primeira classe: podem ser atribuídos a
variáveis, chamadas usando a sintaxe padrão para chamada de função a partir da
variável que foram atribuídas. Podem ser usadas como argumentos, e podem ser
retornadas como valores. Também pode ser criadas anonimamente, sem ter um
nome::

    julia> x -> x^2 + 2x - 1
    #<function>

Isto cria uma função sem nome que possue um argumento e que retorna o valor do
polinômio *x* \ ^2 + 2 \ *x* - 1.  O uso principal para funções anónimas é
serem passadas para funções que recebem outras funções como argumentos. Um
exemplo clássico é a função do ``map``, que aplica uma função a cada valor de
um vetor e retorna um novo vetor que contem os valores resultantes::

    julia> map(round, [1.2, 3.5, 1.7])
    3-element Float64 Array:
     1.0
     4.0
     2.0

Não existe problema se uma função, já nomeada, que efetua a transformação
desejada já existe para ser passada como o primeiro argumento da função
``map``. Entretanto, frequentemente, não existe a função desejada pronta para
uso.  Nestas situações, a função anónima permite a criação de um objeto função
para um único uso sem precisar atribuir um nome::

    julia> map(x -> x^2 + 2x - 1, [1,3,-1])
    3-element Int64 Array:
     2
     14
     -2

Uma função anónima que aceita mais de um argumentos pode ser escrita usando a
sintaxe ``(x,y,z)->2x+y-z``. Uma função anónima sem argumento é escrita como
``()->3``. A ideia de uma função sem argumentos pode parecer estranha, mas é
útil para "atrasar" algum cálculo.  Neste uso, um bloco de código é envolvido
em uma função sem argumento, que é posteriormente invocada chamando ``f()``.

Retornando mais de um valor
---------------------------

Em Julia, uma tupla deve ser retornada para simular o retorno de mais de um
valor. Contudo, os tuplas podem ser criadas e destruidas sem precisar de
parênteses, fornecendo a ilusão de que mais de um valor esta sendo retornado,
ao invés de uma única tuple.  Por exemplo, a função a seguir retorna um par de
valores::

    function foo(a,b)
      a+b, a*b
    end

Se você chama essa função em uma sessão interativa sem atribuir o valor de
retorno em nenhum lugar, você verá a tupla sendo retornada::

    julia> foo(2,3)
    (5,6)

Um uso típico de funções que retornam mais de um valor, contudo, extrai cada
valor em uma variável.  Julia suporta a "destruição" simplificada de tuplas que
facilitam isto::

    julia> x, y = foo(2,3);

    julia> x
    5

    julia> y
    6

Você também pode retornar mais de um valores através do uso explícito da
expressão``return``::

    function foo(a,b)
      return a+b, a*b
    end

Isto tem exatamente mesmo efeito que a definição anterior de ``foo``.

Funções com Número Variado de Argumentos
----------------------------------------

Frequentemente, é conveniente poder escrever funções que tomam um número
arbitrário de argumentos. Tais funções são tradicional conhecidas como funções
*varargs*, que um acrônimo para "variable number of arguments" (ou "número
variável de argumentos", em tradução literal). Você pode definir uma função
*varargs* utilizando depois do último argumento uma elipse (``...``)::

    bar(a,b,x...) = (a,b,x)

As variávies ``a`` e  ``b`` são atribuidas aos primeiros dois argumento como é
o costume, e a variável  ``x`` é atribuida para coleção de zero ou mais valores
passados para ``bar`` depois dos seus primeiros dois argumentos:: 

    julia> bar(1,2)
    (1,2,())

    julia> bar(1,2,3)
    (1,2,(3,))

    julia> bar(1,2,3,4)
    (1,2,(3,4))

    julia> bar(1,2,3,4,5,6)
    (1,2,(3,4,5,6))

Em todos estes casos, ``x`` corresponde a uma tupla dos valores passado a
``bar``. 

Por outros lado, é frequentemente necessário "dividir" os valores presentes em
uma coleção iterável em argumentos individuais para uma chamda de função. Para
fazer isso, usa-se de forma análoga ``...`` mas na chamada da função::

    julia> x = (3,4)
    (3,4)

    julia> bar(1,2,x...)
    (1,2,(3,4))

Neste caso uma tupla de valores é dividido na chamada de uma função *varargs*
precisamente onde o número de argumentos variável vai. Isso não precisar
necessidade ser o caso::

    julia> x = (2,3,4)
    (2,3,4)

    julia> bar(1,x...)
    (1,2,(3,4))

    julia> x = (1,2,3,4)
    (1,2,3,4)

    julia> bar(x...)
    (1,2,(3,4))

Além disso, não é necessário dividir uma tupla para passá-la para uma função::

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

Além disso, a função não precisa ser *varargs* para que os argumentos sejam
divididos (embora é frequentemente)::

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

Como você pode ver, se o objeto a ser dividido na chamada da função resultar em
um número de argumentos diferente do esperado, a função irá falhar, de forma
semelhante se um muitos argumentos tivessem sido passados de forma explícita.

Argumentos opcionais
--------------------

Em muitos casos, os argumentos de uma função possuem valores padrões que não
precisam ser passados explicitamentes em toda chamada de função.  Por exemplo,
a função ``parseint(num,base)`` interpreta interpreta uma *string* como um
número em alguma base.  O valor padrão para o argumento ``base`` é ``10``. Este
comportamento pode ser expresso como::

    function parseint(num, base=10)
        ###
    end

Com esta definição, a função pode ser chamada com um ou dois argumentos, e
``10`` é passado automaticamente quando um segundo argumento não é
especificado::

    julia> parseint("12",10)
    12

    julia> parseint("12",3)
    5

    julia> parseint("12")
    12

Argumentos opcionais são na verdade apenas uma sintaxe conveniente para
escrever mais de uma definição para um método com números diferentes de argumentos
(veja :ref:`man-methods`).

Argumento nomeado
-----------------

Algumas funções precisam de um grande número de argumentos, ou têm um grande
número de comportamentos. Recordar como chamar tais funções pode ser difícil.
Argumentos nomeados, ou *keyword arguments*, podem facilitar o uso destas
funções complexas e estendida ao permitindo que os argumentos sejam
identificados por nome em vez de apenas pela da posição.

Por exemplo, considere uma função ``plot`` que traça uma linha. Esta função
deve ter muitas opções, para controlar o estilo, largura, cor, ... da linha.
Se ela aceitar argumentos nomeados, um possível a chamada pode parecer com
``plot(x, y, width=2)``, onde escolhemos especificar somente a largura da
linha. Observe que isto serve para duas finalidades. A chamada da função é mais
fácil de ler, desde que podemos etiquetar os argumentos com seu significado. E
também, torna-se possível passar qualquer subconjunto de argumentos em qualquer
ordem.

As funções com argumentos nomeados são definidas usando um ponto-e-vírgula na
declaração::

    function plot(x, y; style="solid", width=1, color="black")
        ###
    end

Argumentos nomeados adicionais podem ser informados utilizando ``...``, como
nas funções *vargargs*::

    function f(x; args...)
        ###
    end

Dentro de ``f``, ``args`` será uma coleção de tuplas do tipo ``(chave,valor)``,
onde cada ``chave`` é um símbolo. Tais coleções podem ser passadas como
argumentos nomeados usando um ponto-e-vírgula na chamada da função, ``f(x;
k...…)``. Dicionários podem ser usados para esta finalidade.


Sintaxe de bloco para argumentos de função
-------------------------------------------

Passar funções como argumentos a outras funções é uma técnica poderosa,
mas a sintaxe para isso não é sempre conveniente. Tais chamadas são
especialmente difíceis de escrever quando o argumento da função exige mais de uma linhas.
Por um exemplo, considere a chamada da função ``map`` passando uma função em diversos casos::

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

Julia possue uma palavra reservado ``do`` para reescrevendo este código de
forma mais clara::

    map([A, B, C]) do x
        if x < 0 && iseven(x)
            return 0
        elseif x == 0
            return 1
        else
            return x
        end
    end

A sintaxe ``do x`` cria uma função anónima com o argumento ``x`` e passa essa
função como o primeiro argumento de ``mapa``. Esta sintaxe facilita usar
funções para estender a línguagem, pois as chamadas parecem com blocos de
código convencional. Há muitos usos diferentes da função ``mapa``, como
gerenciar o estado do sistema. Por exemplo, a biblioteca padrão fornece uma
função ``cd`` para rodar código em um diretório especificado, e retornar ao
diretório anterior quando o código terminar ou abortar. Existe também uma
função ``open`` que roda código garantindo que o arquivo aberto será
eventualmente fechado.  Podemos combinar estas funções para escrever com
segurança um arquivo em um determinado diretório::

    cd("data") do
        open("outfile", "w") do f
            write(f, data)
        end
    end

O argumento da função ``cd`` não recebe nenhum argumento; é apenas um bloco de
código.  O argumento da função ``open`` recebe informações de como lidar com o
arquivo aberto.

Leitura adicional
-----------------

Devemos mencionar aqui que esta não é uma imagem completa sobre definições de
funções.  Julia tem um sofisticado sistema de tipos e permite mais de uma
declarações baseada no tipo de argumentos.  Nenhuns dos exemplos dados aqui
fornecem qualquer tipo de anotações sobre seus argumentos, significando que são
aplicáveis a todos os tipos de argumentos. O sistema de tipos é descrito em
:ref:`man-types` e a definição de funções em termos de métodos escolhidos com
base no tipo dos argumentos em tempo de execução é descrito em :ref:
`man-methods`.
