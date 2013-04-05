.. _man-introduction:

**************
 Introdução    
**************

Computação científica tem requerido, tradicionalmente, alta performace embora
grandes nomes da área tenham passado a utilizar linguagens dinâmicas lentas
para o trabalho diário. Acreditamos que existam várias boas razões para
preferir utilizar linguagens dinâmicas em suas aplicações, e não esperamos
desmerecer seu uso. Felizmente, as modernas técnicas para criação de linguagens
e de compilação torna possível eliminar, quase totalmente, o problema de
desempenho de linguagens dinâmicas e prover um ambiente produtivo para
experimentação e eficiente para produção de aplicativos que precisam de alto
desempenho. A linguagem de programação Julia preenche esse buraco: é uma
linguagem dinâmica, apropriada para computação numérica e científica, com um
desempenho comparável a linguagens estáticas tradicionalmente utilizadas.

As características de Julia são tipagem opcional, *multiple dispatch*, e bom
desempenho, alcançado utilizando inferência de tipos e compilação
*just-in-time* (JIT) [#JIT-en]_, [#JIT-pt]_,
implementada utilizando
*LLVM* [#LLVM-en]_, [#LLVM-pt]_. Ela é
multi-paradigma, combinando características de programação imperativa,
funcional e orientada a objetos. A sintaxe de Julia é similar a do `GNU Octave
<http://en.wikipedia.org/wiki/GNU_Octave>`_ ou `MATLAB(R)
<http://en.wikipedia.org/wiki/Matlab>`_ e consequentemente os programadores que
que já utilizam estas linguagens devem sentir-se imediatamente confortáveis com
Julia. Enquanto MATLAB(R) é um bem eficiente para experimentações e explorações
de álgebra linear numérica, possui limitações para tarefas computacionais fora
deste campo relativamente pequeno. Julia mantem a facilidade e expressividade
do MATLAB(R) para computação numérica de alto nível, mas ultrapassa as
limitações comparadas a uma linguagem de programação de propósito geral. Para
alcançar isso, Julia é construída com heranças das linguagens de programação
matemática, mas também herda muito de outras linguagens dinâmicas populares,
incluindo
`Lisp <http://en.wikipedia.org/wiki/Lisp_(programming_language)>`_,
`Perl <http://en.wikipedia.org/wiki/Perl_(programming_language)>`_,
`Python <http://en.wikipedia.org/wiki/Python_(programming_language)>`_,
`Lua <http://en.wikipedia.org/wiki/Lua_(programming_language)>`_, and
`Ruby <http://en.wikipedia.org/wiki/Ruby_(programming_language)>`_.

As características mas significativas de Julia em relação a linguagens
dinâmicas típicas são:

-  O núcleo da linguagem impõe muito pouco; a biblioteca padrão é escrita
   utilizando a própria linguagem Julia, incluindo operadores primitivos como
   operações aritméticas de inteiros
-  Uma grande variedades de tipos para construir e descrever objetos, que pode
   também, opcionalmente, ser utilizado para fazer declarações de tipos
-  A habilidade de definir o comportamento de funções com base na combinação de
   vários tipos de argumentos via *multiple dispatch* [#MD-en]_, [#MD-pt]_
-  Geração automática de código eficiente e especializado para diferentes tipos
   de argumentos
-  Bom desempenho, aproximando-se de linguagens estáticas e compiladas como C

Embora alguns por vezes digam que linguagens dinâmicas não são tipadas,
elas definitivamente são: todo objeto, seja primitivo ou definido pelo usuário,
possui um tipo. A ausência na declaração do tipo na maioria das linguagens
dinâmicas, entretanto, significa que não podemos instruir o compilador sobre o
tipo dos valores, e comumente não podemos falar sobre tipos. Em linguagens
estáticas, em oposição, enquanto podemos - e usualmente precisamos -
especificar tipos para o compilador, tipos existem apenas em tempo de
compilação e não podem ser manipulados ou expressos em tempo de execução. Em
Julia, tipos são objetos em tempo de execução, e podem também ser utilizados
para convenientemente informar o compilador.

Embora o programador casual não precise explicitamente utilizar tipos ou
*multiple dispatch*, estas são características principais de Julia: funções são
definidas para diferentes combinações de tipos de argumentos, e utilizadas de
acordo com as especificações mais semelhantes. Este modelo ser para programação
matemáticas, onde não é natural o primeiro argumento "possuir" uma operação
como é tradicional em linguagens orientadas a objetos. Operadores são apenas
funções com uma função especial - para estender a adição para um novo tipo
definido pelo usuário, você define um novo método para a função ``+``. Codes já
existentes são aplicados para novos tipos sem problemas.

Parcialmente por causa da inferência de tipos em tempo de execução (aumentado
pela opcionalidade da declaração de tipo), e parcialmente por causa do grande
foco em desempenho existente no início do projeto, a eficiência computacional
de Julia é maior que a de outras linguagens dinâmicas, e até rivaliza com
linguagens estáticas e compiladas. Para problemas numéricos de larga escala,
velocidade sempre foi, continua sendo, e provavelmente sempre será crucial: a
quantidade de dados sendo processada tem seguido a Lei de Moore na década
passada.

Julia anseia criar uma combinação sem precedente de facilidade de uso, força e
eficiência em uma única linguagem. Em adição ao dito acima, algumas das
vantagens de Julia em comparação com outros sistemas são:

-  Livre e open source (`Licença MIT
   <https://github.com/JuliaLang/julia/blob/master/LICENSE>`_)
-  Tipos definidos pelo usuário são rápidos e compactos como tipos nativos
-  Ausência da necessidade de vetorizar códigos por desempenho; códigos não
   vetorizados são rápidos
-  Projetado para computação paralela e distribuída
-  *Lightweight "green" threading coroutines* [#COR-en]_, [#COR-pt]_
-  Sistemas de tipos não obstrutivos mas poderoso
-  Conversão e promoção de tipos numéricos e outros de forma elegante e
   extensível
-  Suporte eficiente para
   `Unicode <http://en.wikipedia.org/wiki/Unicode>`_, incluindo mas não
   limitado ao `UTF-8 <http://en.wikipedia.org/wiki/UTF-8>`_
-  Chamadas de funções em C de forma direta (sem necessidade de *wrappers* ou
   *API* especial)
-  Capacidade semelhante a de uma poderosa *shell* para gerenciar outros
   processos
-  Macros de forma parecida a Lisp e outras facilidades de metaprogramação

.. rubric:: Notas de rodapé

.. [#JIT-en] http://en.wikipedia.org/wiki/Just-in-time_compilation
.. [#JIT-pt] http://pt.wikipedia.org/wiki/JIT
.. [#LLVM-en] http://en.wikipedia.org/wiki/Low_Level_Virtual_Machine
.. [#LLVM-pt] http://pt.wikipedia.org/wiki/Low_Level_Virtual_Machine
.. [#MD-en] http://en.wikipedia.org/wiki/Multiple_dispatch
.. [#MD-pt] http://pt.wikipedia.org/wiki/Despacho_m%C3%BAltiplo
.. [#COR-en] http://en.wikipedia.org/wiki/Coroutine
.. [#COR-pt] http://pt.wikipedia.org/wiki/Corotina

