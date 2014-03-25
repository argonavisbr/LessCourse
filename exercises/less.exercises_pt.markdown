#Exercícios
### Parte 1 (introdução, aninhamento, variáveis, extend e mixins)   
1. [*Introdução*] Teste a configuração do seu ambiente:
   a. Transforme o documento `teste.css` em um documento `.less`, defina uma variável `@cor` para as cores que se repetem e aninhe os três blocos. Utilize um compilador Less para gerar um documento CSS e teste-o para verificar se ainda funciona.
   b. Altere o arquivo `teste.html` de forma que ele carregue o documento Less (em vez do CSS) para processamento durante a carga. Altere o documento Less (mude o valor da variável `@cor`) e veja se o HTML é alterado.

2. [*Aninhamento*] O arquivo `basics.css` contém diversos blocos de seletores contextuais e pseudo-classes. Utilize-o como base para criar um documento `.less` que reorganize os 17 blocos em um único bloco partindo de `body`, utilizando o aninhamento ao máximo (para contexto e pseudo-elementos).

3. [*Variáveis*] Remova a duplicação de dados da folha de estilos `variables.css`. Crie uma folha de estlos Less equivalente que utilize variáveis para:

    a. todas as cores
    b. famílias de fontes
    c. partes de nomes de propriedades que se repetem (`border`, `color`);
    d. parte da URL que se repete nas imagens
    e. nome da propriedade `.sec1` (inclua `sec1` em uma propriedade)

4. [*Extend*] Utilize o documento `extend.less` que importa `basics.less` e usando *apenas* pseudo-elementos `:extend` (sem declarar novas propriedades), faça com que a página `extend.html`, que usa `extend.css` "herde" alguns estilos de `basics.less`. Estenda:
   a. `.article` com `article`
   b. `.article .contents` com `.contents`
   c. `.article .contents .section` com `section`
   d. `.article .contents .section h1` com `section .h1`
   e. `nav li` com `.nav li`
   f. `nav li a:hover` com `.nav li a:hover`. 
   
   Use aninhamento e faça adaptações se necessário. Verifique se a declaração usada no extend é uma correspondência *exata*.

5. [*Extend*] O documento `mixin-extend.less` utiliza um mixin `.gradient()` para construir um gradiente de cinco cores. O mixin está definido em `gradient-mixin.less` que é importado em `mixin-extend.less`. A página `mixin-extend.html` exibe um `div` branco, cujas dimensões e borda estão definidas em `mixin-extend.less`. Analise o código gerado no CSS e altere o documento `mixin-extend.less` de tal maneira que o gradiente apareça desenhado no `div`.

6. [*Mixins*] O documento `mixin-6.html` contém quatro tabelas idênticas que diferem apenas no `id`. O documento `mixin-6.less` aplica funções que variam a largura e o brilho de uma cor para cada tabela. Observe que há bastante duplicação de código. Crie um mixin para gerar o CSS para as tabelas que receba como argumentos pelo menos uma cor e uma largura. Chame o mixin para cada `id` e verifique se o CSS gerado é o mesmo.

7. [*Mixins*] Analise o documento `mixin-7.less` e identifique trechos duplicados ou trechos que possam ser embutidos em mixins. Crie os mixins indicados utilizando ou não os nomes sugeridos, e substitua os trechos duplicados ou longos por chamadas a esses mixins. Defina argumentos como indicados e use valores default se desejar.

8. [*Operações e funções*] Analise o documento `operations-8.less`. Veja o resultado em `operations-8.less`. Troque os valores fixos por valores obtidos a partir das duas variáveis declaradas no documento. Siga as instruções nos comentários para:
    a. Determinar os valores de `@conversion` em duas células `td` através de conversão do valor de `@dimension` no seletor `#unit`.
    b. Determinar os valores de `@result` em três células `td` através do uso de funções de arredondamento do valor `@number` no seletor `#round`.
    c. Obter o valor de `@angulo` usando apenas o valor de `@number` (converta o número de radianos para graus); depois obter os valores `@result` através de funções trigonométricas sobre `@angulo` ou `@number`.
    
9. [*Operações e funções*]  Abra o documento `operations-9.html`. Veja o HTML e o resultado no browser. O documento `operations-9.less` reposiciona os objetos usando valores fixos. Altere-o para que os valores sejam calculados em função das três variáveis `@width` (largura de um quadrado menor), `@height` (altura) e `@gap` (margem). Use os comentários como guias.

10. [*Operações e funções*]  O documento `operations-10.less` possui várias variáveis e valores fixos. Siga as instruções nos comentários para que os valores sejam derivados ou extraídos das variáveis e dados fornecidos:
    a. extraia a menor percentagem `@minp` da lista `@p`, a maior dimensão `maxh` da lista `@h` e a última cor `@lastc` da lista de cores `@c`.
    b. construa um string de query iniciado em `?`, separando pares nome=valor por `&` com caracteres especiais codificados no formato *url-encoding*. Use o exemplo fornecido como guia e quaisquer funções ou operadores do Less para obter o resultado. Depois que montar o query-string, inclua-o no final da `@url`.
    c. extraia a cor (formato `#rrggbb`) do *query-string*  montado no exercício anterior. Será necessário converter o caractere `%23` em `#`, eliminar o início do string e converter o resultado em uma cor, que será usada em duas outras propriedades.
    
11. [*Loops e guards*] Utilize o documento `conditional.less` e escreva um mixin condicional para gerar colunas de acordo com a largura maxima da pagina e de cada coluna. Multiplique o numero de colunas pela largura de cada coluna e some os espacos entre as colunas. Se este valor for menor que a largura total, gere as propriedades CSS para construir o numero de colunas indicado. Se o valor for maior, calcule o maior numero de colunas que pode ser inserido. Veja exemplos nos comentarios.

12. [*Loops e guards*] loops - geracao de figuras
13. [*Cores*] key frames animacao com spin
14. [*+Less*] contrast
15. [*+Less*] import
16. [*+Less*] script test
