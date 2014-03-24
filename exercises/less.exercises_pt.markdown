#Exercícios
### Parte 1 (introdução, aninhamento, variáveis, extend e mixins)   
1. [*Introdução*] Teste a configuração do seu ambiente:
   a. Transforme o documento `teste.css` em um documento `.less`, defina uma variável `@cor` para as cores que se repetem e aninhe os três blocos. Utilize um compilador Less para gerar um documento CSS e teste-o para verificar se ainda funciona.
   b. Altere o arquivo `teste.html` de forma que ele carregue o documento Less (em vez do CSS) para processamento durante a carga. Altere o documento Less (mude o valor da variável `@cor`) e veja se o HTML é alterado.

2. [*Aninhamento*] O arquivo `basics.css` contém diversos blocos de seletores contextuais e pseudo-classes. Utilize-o como base para criar um documento `.less` que reorganize os 17 blocos em um único bloco partindo de `body`, utilizando o aninhamento ao máximo (para contexto e pseudo-elementos).3. [*Variáveis*] Remova a duplicação de dados da folha de estilos `variables.css`. Crie uma folha de estlos Less equivalente que utilize variáveis para:

    a. todas as cores
    b. famílias de fontes
    c. partes de nomes de propriedades que se repetem (`border`, `color`);
    d. parte da URL que se repete nas imagens
    e. nome da propriedade `.sec1` (inclua `sec1` em uma propriedade)4. [*Extend*] Utilize o documento `extend.less` que importa `basics.less` e usando *apenas* pseudo-elementos `:extend` (sem declarar novas propriedades), faça com que a página `extend.html`, que usa `extend.css` "herde" alguns estilos de `basics.less`. Estenda:
   a. `.article` com `article`   b. `.article .contents` com `.contents`
   c. `.article .contents .section` com `section`
   d. `.article .contents .section h1` com `section .h1`
   e. `nav li` com `.nav li`
   f. `nav li a:hover` com `.nav li a:hover`. 
   
   Use aninhamento e faça adaptações se necessário. Verifique se a declaração usada no extend é uma correspondência *exata*.

5. [*Extend*] O documento `mixin-extend.less` utiliza um mixin `.gradient()` para construir um gradiente de cinco cores. O mixin está definido em `gradient-mixin.less` que é importado em `mixin-extend.less`. A página `mixin-extend.html` exibe um `div` branco, cujas dimensões e borda estão definidas em `mixin-extend.less`. Analise o código gerado no CSS e altere o documento `mixin-extend.less` de tal maneira que o gradiente apareça desenhado no `div`.

6. [*Mixins*] O documento `mixin-6.html` contém quatro tabelas idênticas que diferem apenas no `id`. O documento `mixin-6.less` aplica funções que variam a largura e o brilho de uma cor para cada tabela. Observe que há bastante duplicação de código. Crie um mixin para gerar o CSS para as tabelas que receba como argumentos pelo menos uma cor e uma largura. Chame o mixin para cada `id` e verifique se o CSS gerado é o mesmo.

7. [*Mixins*] Analise o documento `mixin-7.less` e identifique trechos duplicados ou trechos que possam ser embutidos em mixins. Crie os mixins indicados utilizando ou não os nomes sugeridos, e substitua os trechos duplicados ou longos por chamadas a esses mixins. Defina argumentos como indicados e use valores default se desejar.
8. [*Operações e funções*] medias
9. [*Operações e funções*]  calculo de margens, padding e colunas
10. [*Operações e funções*]  alinhamento de angulo11. [*Loops e guards*] condicionais12. [*Loops e guards*] loops - geracao de figuras
13. [*Cores*] key frames animacao com spin14. [*+Less*] contrast15. [*+Less*] import16. [*+Less*] script test