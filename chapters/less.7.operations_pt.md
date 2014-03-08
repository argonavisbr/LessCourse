#{7: operações e funções }

Neste capítulo serão explorados recursos do Less que permitem realizar operações matemáticas e de transformação de dados, operações condicionais e loops.

##matemática

Less permite realizar operações matemáticas entre valores numéricos e cores, levando em conta as unidades em que são declaradas e fazendo conversão automática quando possível. 

As expressões geram CSS estático, que é diferente do funcionamento da função `calc()` do CSS3, que também é capaz de realizar operações matemáticas, ou das operações em JavaScript que podem operar dinamicamente. Less não tem acesso à estrutura DOM da página que irá receber o estilo. É apenas um processador que *gera* CSS, portanto toda a matemática irá gerar um string estático. Ainda assim é bastante útil para realizar alinhamentos, dimensionamento, manipulação de cores, temporização de animações, distribuição de gradientes, calculo do tamanho de fontes, etc.

As expressões podem ser usadas diretamente na atribuição de valores a propriedades, na definição de variáveis e podem incluir outras variáveis. Para evitar ambigüidades, é recomendável escrever todas as expressões entre parênteses e usar sempre variáveis. Esta recomendação também garantirá o funcionamento em versões futuras de Less.

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
}
```

O resultado do processamento do código acima em CSS é:
```
.sec1 {
  margin: 7px 7px;
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

## ambigüidades e parênteses
Expressões matemática *podem* ser escritas diretamente nas propriedades, mas é sempre recomendável usar variáveis.
[    INCLUIR DE FONTES: http://meri-stuff.blogspot.com.br/2013/05/less-css-expressions-and-traps.html , ]

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
px, m, cm, mm, in, pt, px | Comprimentos
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

### funções de arredondamento e percentagem
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

###funções de conversão de unidades
Nem sempre é possível ou recomendado usar transformação automática de unidades. Às vezes é desejável acrescentar uma unidade em um número após a realização de cálculos, ou ainda trocar a unidade sem fazer conversão. Duas funções servem a esse propósito:

Função | Recebe | Retorna
--|--|--
`convert(n, u)` | `n` = número (com ou sem unidade), `u` = unidade compatível | Número convertido na nova unidade: `convert(1in, cm)` gera `2.54cm`
`unit(n, u)` | `n` = número (com ou sem unidade), `u` = unidade | Número recebido qualificado com a unidade passada como parâmetro. `unit(1in, cm)` gera `1cm`

Se as unidades passadas para `convert()` não forem compatíveis ele funcionará igual a `unit()` simplesmente copiando o mesmo valor e trocando a unidade.
A função `convert()` não suporta pixels (`px`), `em` ou `rem`.

Exemplos:
```
```
CSS:

```
```

### funções matemáticas e trigonométricas
Less possui várias funções matemáticas e trigonométricas que são úteis para calcular valores usados em alinhamentos, animações 2D e 3D, transições, conversão de coordenadas, métricas de fontes, etc. evitando a necessidade de usar JavaScript para valores que muitas vezes permanecem estáticos.

As funções estão relacionadas na tabela abaixo. Os valores passados podem ser números, variáveis que contenham números ou outras funções que retornem números.

Função | Recebe | Retorna
--|--|--
sin | ângulo em radianos | seno
asin | ângulo em radianos | seno<sup>-1</sup>
cos | ângulo em radianos | cosseno
acos | ângulo em radianos | cosseno<sup>-1</sup> 
tan | ângulo em radianos | tangente
atan | ângulo em radianos | tangente<sup>-1</sup>
pi | | 3.141516...
pow | | potência
sqrt | número | raiz quadrada
mod | módulo | módulo
abs | número | valor absoluto

Veja alguns exemplos usando convert e funções trigonométricas:

```
```
Resultado em CSS:

```
```

### funções que operam sobre coleções
Less possui algumas funções que operam sobre listas. Uma lista é um conjunto de valores separada por vírgulas ou espaços. Muitas propriedades do CSS e seletores podem ser tratados como listas, e elas podem ser usadas em loops.

As funções estão listadas abaixo:

Função | Recebe | Retorna
--|--|--
min | lista de números | o menor número da lista
max | lista de números | o maior número da lista
length | lista de valores | o número de elementos
extract | lista de valores | o valor em uma posição específica da lista

Alguns exemplos:

```
```

CSS:

```
```




##transformação de texto
### conversao data-uri
Converte uma URL em um recurso embutido.
```
```
```
```
### escape
realiza conversão de URL-encoding para caracteres especiais que precisem ser incluídos em uma URL, por exemplo, converter espaço em %20.
### e
converte strings em formato reconhecido pelo less (similar a color, só que mais abrangente)

###% (mover para outro lugar!)
A função % permite  a criação de strings formatados no estilo da função printf do C. Ela recebe uma string contendo placeholders indicando o tipo, seguido pelas variáveis, valores ou strings que devem ser inseridos nos lugares onde estão os placeholders.
(explorar as possibilidades com isto - o que pode e o que nao pode)

##exercícios
1. x
2. x
3. x
4. x
5. x
6. x