#{6: operações e funções }

Neste capítulo serão explorados recursos do Less que permitem realizar operações matemáticas e de transformação de dados, operações condicionais e loops.

##operações aritméticas

Less permite realizar operações matemáticas entre valores numéricos e cores, levando em conta as unidades em que são declaradas e fazendo conversão automática quando possível. 

As expressões geram CSS estático, que é diferente do funcionamento da função `calc()` do CSS3, que também é capaz de realizar operações matemáticas, ou das operações em JavaScript que podem operar dinamicamente. Less não tem acesso à estrutura DOM da página que irá receber o estilo. É apenas um processador que *gera* CSS, portanto toda a matemática irá gerar um string estático. Ainda assim é bastante útil para realizar alinhamentos, dimensionamento, manipulação de cores, temporização de animações, distribuição de gradientes, calculo do tamanho de fontes, etc.

As expressões podem ser usadas diretamente na atribuição de valores a propriedades, na definição de variáveis e podem incluir outras variáveis. Para evitar ambigüidades, é recomendável escrever as expressões entre parênteses e de preferência realizá-las em variáveis. Esta recomendação também garantirá o funcionamento em versões futuras de Less.

Os operadores que podem ser usados em expressões matemáticas são os mesmos operadores básicos encontrados nas linguagens de programação: 

* `*` multiplicação
* `/` divisão
* `+` soma
* `-` subtração

O exemplo abaixo ilustra o uso desses operadores:

```
@parte1: 10px;
@parte2: 20px;
@borda: 2px;
@margem: (((@parte1 + @parte2) / (2 * @borda)) - 0.5);

.sec1 {
  margin: @margem @margem;
  color: (#777 + #333);
  width: 100% / 4;
}
```

O resultado do processamento do código acima em CSS é:

```
.sec1 {
  margin: 7px 7px;
  color: #AAAAAA;
  width: 25%;
}
```

As operações também têm a mesma precedência das linguagens de programação: calculados da esquerda para a direita, mas com multiplicação, resto e divisão tendo prioridade sobre adição e subtração. A precedência pode ser alterada com o uso de parênteses. Removendo os parênteses do exemplo acima, o resultado é outro:

```
@margem: (@parte1 + @parte2 / 2 * @borda - 0.5);
```

Resultado:

```
.sec1 {
  margin: 29.5px 29.5px;
}
```

Erros de matemática não são detectados pelo pré-processador Less. A divisão por zero, por exemplo, resulta em `Infinity`. Outros podem gerar CSS incorreto, como por exemplo tamanhos de fonte negativas:

```
.sec3 {
  @tam: (@parte2 - 2 * @parte1);
  font-size: (12pt - @parte2);
  margin: @tam / @parte2 @parte2 / @tam - @parte1;
}
```

Que resulta em:

```
.sec3 {
  font-size: -8pt;
  margin: 0px Infinitypx;
}
```

## operações usando unidades
Operações aritméticas podem e geralmente usam unidades. Se em uma expressão, nenhum número tiver uma unidade, o resultado final será mantido sem unidade:

```
.sec4 {
  font-size: 4 + 2 + 16;
}
```
CSS:

```
.sec4 {
  font-size: 22;
}
```
Se um dos termos possuir uma unidade, essa unidade será usada no resultado final:

```
.sec5 {
  font-size: 4 + 2px + 16;
}
```
```
.sec5 {
  font-size: 22px;
}
```
Se os números de uma expressão tiverem mais de uma unidade, e as unidades forem diferentes, elas poderão ser convertidas automaticamente se forem compatíveis e se as operações forem de soma (+) e subtração (-). A primeira unidade usada na expressão será a unidade do número resultante. A tabela abaixo lista os grupos compatíveis:

Unidades | Tipo
--|--
pc, m, cm, mm, in, pt | Comprimentos
s, ms | Tempo
deg, rad, grad, turn | Ângulos

Unidades do CSS não listadas na tabela acima (`px`, `em`, `rem`, etc.) não podem ser misturadas. Podem ser usadas em expressões contendo números da mesma unidade ou com números sem unidade.

Observe o efeito da conversão automática de unidades nas expressões abaixo.

 ```
 .sec6 {
  padding: (1cm + 1mm) (1mm + 1cm) (1pt + 1pc) (1m + 1in);
  duration: (2s + 2ms);
  margin: (1px + 1rem);
  transform: rotate(1deg + 1rad) rotate(1rad + 1deg) rotate(1deg + 1grad) rotate(1deg + 1turn);
}
 ```
 E o CSS resultante:
 
 ```
 .sec6 {
  padding: 1.1cm 11mm 13pt 1.0254m;
  duration: 2.002s;
  margin: 2px;
  transform: rotate(58.29577951deg) rotate(1.01745329rad) rotate(1.9deg) rotate(361deg);
}
 ```
Outras combinações produzirão resultados incorretos e devem ser evitadas. Se forem realizadas operações entre grupos não compatíveis, ou operações de multiplicação ou divisão envolvendo números com unidades diferentes, os valores serão multiplicados *sem considerar as unidades*, e a unidade gerada no final pode ser tanto a primeira (em divisão) como a última (em multiplicação). 

```
.calculos-incorretos {
  duration: (2s * 2ms);
  margin: (1px + 1em) (1em + 1rem) (1mm * 1cm) (1m / 100cm);
  transform: rotate(3turn / 1080deg) rotate(360deg * 3turn);
}
```

CSS:

```
.calculos-incorretos {
  duration: 4ms;
  margin: 2px 2em 1cm 0.01m;
  transform: rotate(0.00277778turn) rotate(1080deg);
}
```

## arredondamento e percentagem
A tabela abaixo relaciona as funções `ceil()` (teto), `floor()` (chão) e `round()` (arredonda) que eliminam a parte decimal de um número de ponto-flutuante segundo diferentes critérios, e `percentage()` que converte o valor em percentagem. 

Função | Recebe | Retorna
--|--|--
ceil | número | Próximo inteiro. `ceil(5.1)` = 6
floor | número | Inteiro anterior. `floor(5.9)` = 5
round | número | Inteiro arredondado. `round(5.4)` = 5, `round(5.5)` = 6.
percentage | número | Número multiplicado por 100 + unidade %

Exemplo:

```
.secao {
  margin: ceil(5.2px) floor(5.7px) round(5.5px) round(5.4px);
  width: percentage(0.5);
}
```
CSS:

```
.secao {
  margin: 6px 5px 6px 5px;
  width: 50%;
}
```

## conversão de unidades
Nem sempre é possível ou recomendado usar transformação automática de unidades. Às vezes é desejável acrescentar uma unidade em um número após a realização de cálculos, ou ainda trocar a unidade sem fazer conversão. Duas funções servem a esse propósito:

Função | Recebe | Retorna
--|--|--
`convert(n, u)` | `n` = número (com ou sem unidade), `u` = unidade compatível | Número convertido na nova unidade: `convert(1in, cm)` gera `2.54cm`
`unit(n, u)` | `n` = número (com ou sem unidade), `u` = unidade | Número recebido qualificado com a unidade passada como parâmetro. `unit(1in, cm)` gera `1cm`

Se as unidades passadas para `convert()` não forem compatíveis ele funcionará igual a `unit()` simplesmente copiando o mesmo valor e trocando a unidade.
A função `convert()` não suporta pixels (`px`), `em` ou `rem`.

Os exemplos abaixo ilustram o uso das funções convert e unit em várias situações e comparadas à conversões automáticas incorretas em expressões:

```
@height: 2cm;
@width: 10px;
@length: 2in;

.secao {
  margin: (@width / @height) unit(@width / @height, cm);
  padding: (@height * @length) (@height * convert(@length, cm));
  font-size: convert(1in, cm);
  duration: unit(floor(100 * 12 / 142), s);
}
```
Resultado em CSS:

```
.secao {
  margin: 5px 5cm;
  padding: 4cm 10.16cm;
  font-size: 2.54cm;
  duration: 8s;
}
```

##funções matemáticas e trigonométricas
Less possui várias funções matemáticas e trigonométricas que são úteis para calcular valores usados em alinhamentos, parâmetros para animações 2D e 3D, transições, conversão de coordenadas, métricas de fontes, filtragem e composição de cores, etc. evitando a necessidade de usar JavaScript quando precisa-se apenas de valores estáticos.

As funções estão relacionadas na tabela abaixo. Os valores passados podem ser números, variáveis que contenham números ou outras funções que retornem números.

A unidade default para ângulos é radianos (`rad`), mas os parâmetros podem ser qualificados com uma unidade. Quanto ângulos são retornados, eles são retornados como radianos. Se os valores retornados forem comprimentos eles serão números (sem unidades).

Função | Recebe | Retorna
--|--|--
`sin(a)` | ângulo | seno do ângulo
`asin(c)` | comprimento | ângulo (inverso do seno )
`cos(a)` | ângulo | cosseno do ângulo
`acos(c)` | comprimento | ângulo (inverso do cosseno)
`tan(a)` | ângulo | tangente do ângulo
`atan(c)` | comprimento | ângulo (inverso da tangente)
`pi()` | | 3.14159265
`pow(n,p)` | `n` = número, `p` = potência | número elevado à potência
`sqrt(n)` | número | raiz quadrada
`mod(n,d)` | `n` = nominador, `d` = denominador | módulo (resto)
`abs(n)` | número | valor absoluto

Veja alguns exemplos.

Ângulos e arcos:

```
#senos {
  transform: translateY(sin(45deg)) rotate(convert(asin(0.5px), deg));
}
#co-senos {
  transform: translateX(cos(45deg)) rotate(convert(acos(0.5px), deg));
}
#tangentes {
  transform: skewX(tan(45deg)) rotate(convert(atan(1px), deg));
}
```
Resultado em CSS:

```
#senos {
  transform: translateY(0.70710678) rotate(30deg);
}
#co-senos {
  transform: translateX(0.70710678) rotate(60deg);
}
#tangentes {
  transform: skewX(1) rotate(45deg);
}
```

Um mixin que calcula áreas e circunferências:

```
.circle-mixin(@radius, @x, @y) {
  @area: pi() * pow(@radius, 2);
  @circunference: 2 * pi() * @radius;
}

#trigonometria {
  .circle-mixin(5, 0, 0);
  transform: rotate(acos(0.5px)*180deg/pi());
  content:  'area: @{area} circunference: @{circunference}';
}
```
CSS:

```
#trigonometria {
  transform: rotate(60deg);
  content: 'area: 78.53981633974483 circunference: 31.41592653589793';
}
```

Cálculo de potência, raiz quadrada, restos, números absolutos:

```
#quadrados {
  margin: pow(5px,2);
  padding:sqrt(25cm);
}

#modulos {
  length: mod(11px,3);
  width: abs(-10cm);
  @component: pow(2,7); // 50% 
  color: rgb(@component,@component,@component);
}
```
CSS:

```
#quadrados {
  margin: 25px;
  padding: 5cm;
}
#modulos {
  length: 2px;
  width: 10cm;
  color: #808080;
}
```

##funções que operam sobre coleções
Less possui algumas funções que operam sobre listas. Uma lista é um conjunto de valores separada por vírgulas ou espaços. Muitas propriedades do CSS e seletores podem ser tratados como listas, e elas podem ser usadas em loops.

Função | Recebe | Retorna
--|--|--
`length(a)` | `a` = lista de valores | o número de elementos da lista
`extract(a, p)` | `a` = lista de valores,<br/>`p` = posição | o valor da lista contido na posição informada
`min(c)` | `c` = lista de números separada por vírgulas | o menor número da lista
`max(c)` | `c` = lista de números separada por vírgulas | o maior número da lista

O exemplo abaixo ilustra o uso de várias funções listadas acima:

```
@comp: 255 127 64;
@tamanhos: 36pt 24pt 18pt 16pt 12pt 10pt;
@fontes: 'Garamond', 'Times New Roman', Times, serif;
@steps: 0% 20% 40% 60% 80% 100%;

.paragrafo {
  font-size: min(@tamanhos);
  duration: unit(length(@steps), s);
  font-family: @fontes;
}

.titulo {
  font-size: max(@tamanhos);
  color: rgb(extract(@comp,1), extract(@comp, 2), extract(@comp, 3));
  font-family: extract(@fontes, 1), extract(@fontes, length(@fontes));
}
```

Resultado em CSS:

```
.paragrafo {
  font-size: 10pt;
  duration: 6s;
  font-family: 'Garamond', 'Times New Roman', Times, serif;
}
.titulo {
  font-size: 36pt;
  color: #ff7f40;
  font-family: 'Garamond', serif;
}
```

##transformação e formatação de texto
Less oferece algumas funções que auxiliam na construção de strings de texto formatado e conversão de texto em números, cores, urls, etc. Através deles é possível suprimir várias limitações do Less e gerar CSS literalmente. Suponha que temos as seguintes variáveis:

```
@image-name: 'pattern 5.png';
@docroot: 'http://www.test.com/app';
```

E queremos usá-las para construir o valor de uma propriedade `background` da forma:

```
background: url(http://www.test.com/app/images/pattern%205.png);
```
Poderíamos tentar concatenar as variáveis assim:

```
.sec8 {
  background: url(@docroot '/images/' @image-name);
}
```

Mas o processador Less reclama e não permite. Precisamos usar interpolação de variáveis e colocá-las dentro do string:

```
.sec8 {
  background: url('@{docroot}/images/@{image-name}');
}
```
Com isto obtemos o resultado:

```
.sec8 {
  background: url('http://www.test.com/app/images/pattern 5.png');
}
```
que não é exatamente o que queremos. Precisamos converter o espaço no nome do arquivo em `%20` e remover as aspas.

A remoção das aspas pode ser feita com a função `e()` ou pelo operador `~`. São sinônimos. Tanto faz usar um como outro. Esta função ou operador faz algo parecido com o `eval()` do JavaScript, convertendo o string recebido em uma representação nativa do Less. Na prática, ele vai remover as aspas no resultado final gerado no JavaScript: A conversão pode ser feita da forma abaixo:

```
.sec8 {
  background: e('url(@{docroot}/images/@{image-name})');
}
```
ou
```
.sec8 {
  background: url(~'@{docroot}/images/@{image-name}');
}
```

O operador `~` é mais versátil já que a função `e()` não é aceita em qualquer lugar. O resultado para qualquer uma das alternativas acima é a mesma:

```
.sec8 {
  background: url(http://www.test.com/app/images/pattern 5.png);
}
```

Ainda falta converter o espaço do nome da imagem no formato *url-encoded*: `%20`. Isto pode ser feito com a função `escape`, que requer um string:

```
.sec8 {
  @image-escaped: escape(@image-name);
  background: url(~'@{docroot}/images/@{image-escaped}');
}
```
O resultado final é:

```
.sec8 {
  background: url(http://www.test.com/app/images/pattern%205.png);
}
```

Há outras formas de obter o mesmo resultado. Na tabela abaixo estão as funções e operadores de manipulação de string do Less:

Função/operador | O que recebe | O que faz
--|--|--
`e('s')` ou `~'s'` | `'s'`&nbsp;=&nbsp;string | Converte string em representação nativa do Less. Ex: `~"12px"` ou `~'12px'` ou `e("12px")` resulta em `abc` em CSS. Sem usar o operador, o resultado em CSS mantém as aspas.
`escape('s')` | `'s'`&nbsp;=&nbsp;string | Codifica string em formato url-encoded, que atribui códigos unicode no formato `%hh` para caracteres que são reservados em URLs, onde `h` é um dígito hexadecimal entre `0` e `f`. URL-encoding *não* substitui: `,`, `/`, `?`, `@`, `&`, `+`, `'`, `~`, `!`, `$`. Exemplo: `escape('São Paulo')` = `S%C3%A3o%20Paulo`
`%('s', args...)` | `'s'` = string contendo parâmetros de substituição, `args...` = valores de substituição `%a`, `%A`, `%s`, `%S`, `%d`, `%D` | Permite  a criação de strings formatados com parâmetros de substituição (uma versão ultra-simplificada do `printf` do C). Os parâmetros de substituição maiúsculos fazem `escape()` automático. `%a`/`%A` e `%d`/`%D` são equivalentes (Less 1.7) mas `%s`/`%S` retornam `undefined` se o valor for uma cor. Exemplo: `%(~'url(%a) url(%A)', a b, a b)` gera `url(a b) url(a%20b)`
replace(s1, p, s2, f) | `s1` = string,<br/>`p` = regexp, `s2`&nbsp;=&nbsp;substituição, `f` = flags | Substitui um padrão de expressão regular (`p`) encontrado em um string (`s1`) por outro (`s2`) com flags opcionais (`g` = global, `i` = ignore case). Exemplo: `replace(~"w.a.b", "a\.b", "x.k")` gera ` w.x.k`.
`color('s')`|`s` = string contendo a representação de uma cor | Retorna uma cor (sem as aspas). Exemplo: `color('#ff0000')` = `#ff0000`, `color('red')` = `#ff0000`. É possível obter o mesmo resultado usando `e()` ou `~`: `~'#ff0000'` = `#ff0000`.

Mais exemplos usando `%()`:

```
@estilo: italic;
@peso: 900;
@mintam: 10;
@maxtam: 36;
@fam: Arial, Helvetica, sans-serif;

@image-name: 'pattern 5.png';
@docroot: 'http://www.test.com/app';

.sec7 {
  font: %(~"%d %d %dpt/%dpt %d", @estilo, @peso, @mintam, @maxtam, @fam);
  background: %(~"url(%d/images?name=%D)", @docroot, @image-name);
}

```
CSS:

```
.sec7 {
  font: italic 900 10pt/36pt Arial, Helvetica, sans-serif;
  background: url('http://www.test.com/app'/images?name='pattern%205.png');
}
```

Mais exemplos usando `replace()`:

```
@old-url: ~"url(http://abc.com/logo-ABC.COM.jpg)";
.sec9 {
  background: replace(@old-url, "abc\.com", "xyz.com.br", "gi");
}
```
CSS:

```
.sec9 {
  background: url(http://xyz.com.br/logo-xyz.com.br.jpg);
}
```

## sintaxes problemáticas
Vimos que o uso de expressões aritméticas usando unidades incompatíveis (ou expressões de divisão e multiplicação com unidades diferentes) geram resultados incorretos e devem, portanto, ser evitados. Outros problemas surgem na ambigüidade devido ao significado duplo da barra `/` e do espaço. Esses problemas podem ser resolvidos com o uso de parênteses.

O trecho de código abaixo inclui várias propriedades que necessitam preservar a barra no CSS. O Less identifica essas propriedades e não realiza a operação, copiando os valores intactos para o CSS:

```
@media screen and (aspect-ratio: 16/9) and (min-height: 960px / 2 ) {
  .section {
    margin-top: 16/10;
    font: 12pt/36pt;
    font-size: 12pt/36pt;
  }
}
```
O compilador Less gera o CSS abaixo para o código acima:

```
@media screen and (aspect-ratio: 16/9) and (min-height: 960px / 2) {
  .section {
    margin-top: 1.6;
    font: 12pt/36pt;
    font-size: 0.33333333pt;
  }
}
```

Observe que o `aspect-ratio` não foi tratado como uma divisão, corretamente, mas o `min-height` também não, embora o valor aceito por ele seja número.

Dentro do bloco `.section` a propriedade margin-top teve o valor 16/10 transformado em 1.6, `font-size` dividiu `12pt` por `36pt` mas `font`, que aceita no CSS o valor `12pt/36pt` não realizou a divisão.

E se você realmente quisesse realizar a operação de divisão em `aspect-ratio` e `font`? E também a subtração de `min-height`? Neste caso, basta envolver a expressão entre parênteses, que o Less irá entender que ela deve ser tratada como expressão matemática:

```
@media screen and (aspect-ratio: (16/9)) and (min-height: (960px / 2) ) {
  .section {
    margin-top: 16/10;
    font: (12pt/36pt); 
    font-size: 12pt/36pt;
  }
}
```
E agora o resultado do CSS é:

```
@media screen and (aspect-ratio: 1.77777778) and (min-height: 480px) {
  .section {
    margin-top: 1.6;
    font: 0.33333333pt;
    font-size: 0.33333333pt;
  }
}
```

Quanto ao significado do espaço ele também pode ser resolvido mas não com parênteses. É preciso usar uma notação que não seja ambígua. Se um valor de propriedade aceita quatro números separados por espaços, o Less só irá tratar os valores como expressão matemática se não houver outra forma de interpretar os dados. Veja o resultado dos valores de `margin` e `padding` abaixo:

```
header, footer {
  padding: -5 -10 +5 -20;
  margin: -5 - 10 +5- 20;
}
```
Os valores de `padding` podem ser interpretados como quatro valores, então Less irá tratá-los assim. Já os valores de `margin` não podem ser tratados como valores, pois um `-` não vale como valor, e `+5-` é ilegal, mas ainda é possível encontrar dois valores. O resultado é:

```
header, footer {
  padding: -5 -10 5 -20;
  margin: -15 -15;
}
```

Se a intenção for a realização da expressão aritmética, o sinal negativo não pode estar junto do número se houver um espaço antes. Tem que ficar claro que é uma expressão:

```
header, footer {
  padding: -5 - 10 + 5 - 20;
  margin: -5-10+5-20;
}
```

Resultado em CSS:

```
header, footer {
  padding: -30;
  margin: -30;
}
```

##exercícios
1. x
2. x
3. x
4. x
5. x
6. x