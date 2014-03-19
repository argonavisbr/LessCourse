#{5: mixin }
##mixins
Variáveis permitem armazenar valores. Mixins permitem armazenar conjuntos inteiros de regras, que podem ser reusadas em outros blocos. 

A declaração de um mixin não difere em nada de uma declaração de um seletor CSS de classe ou id. A diferença é que o mixin foi criado para ser usado apenas na folha de estilos. Além disso, mixins podem ter parâmetros.

O mixin abaixo agrupa várias regras usadas em diferentes seções de um documento:

```
.text-content-set {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid rgb(225,225,225);
  background-color: rgb(128,128,128);
  color: rgb(240,240,240);
}
```

Expresso desta forma, o CSS gerado é praticamente o mesmo (talvez mude a forma de representação de cores, dependendo do processador). O que faz esse bloco um mixin é a *forma* como ele é usado. Agora podemos aplicá-lo em vários outros blocos, e evitar repetir um monte de declarações:

```
.section {
   .text-content-set;
   h1 {
     color: #00008b;
   }
}

#footer {
  .text-content-set;  // este é o mixin
  background-color: white;
}
```

O resultado será:

```
.text-content-set {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section h1 {
  color: #00008b;
}
#footer {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
  background-color: white;
}

```

Como não pretendemos que o mixin seja usado na página, podemos ocultá-lo na geração final do CSS declarando-o com parênteses:

```
.text-content-set() { ... }
```

Assim apenas os elementos que usam o mixin fazem parte do CSS final:

```
.section {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section h1 {
  color: #00008b;
}
#footer {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
  background-color: white;
}
```
É uma boa prática sempre usar parênteses na declaração de mixins, mesmo que eles não tenham parâmetros. 

Mixins podem também usar seletores de id (`#nome`) em vez de seletores de classe (`.nome`). Não faz nenhuma diferença, mas a convenção é usar seletores de classes para mixins, e seletores de id para namespaces (que veremos mais adiante).

##mixins com blocos aninhados
Mixins podem conter blocos aninhados, por exemplo:

```
.mixin1() {
  .abc {
    color: black;
  }
  .xyz {
    color: gray;
  }
}
```
Isto vai criar incluir .abc e .xyz no contexto de qualquer seletor que usar o mixin:

```
.container {
  .mixin1();
}
```
Que resulta no CSS:

```
.container .abc {
  color: black;
}
.container .xyz {
  color: gray;
}
```

Pode-se criar outros tipos de relacionamento entre seletores, mas não é permitido usar `&` antes ou depois da *chamada*  a um mixin:

```
.container {
  &.mixin1;  // ILEGAL!
  .mixin1 &;  // ILEGAL!
}
```

No entanto, é possível usar o símbolo & dentro de um mixin, permitindo que o seletor do bloco externo seja usado como parâmetro dentro do mixin:

```
.mixin2() {
  &.abc { 
    color: blue;
  }
  &.xyz {
    color: navy;
  }
}
```
Desta vez a chamada:

```
.component {
  .mixin2();
}
```
irá passar o(s) seletor(es) do bloco onde o mixin foi chamado como argumento no lugar do &, adicionando um seletor de classe em cada um:

```
.component.abc {
  color: blue;
}
.component.xyz {
  color: navy;
}
```

##!important
A palavra-chave `!important` é usada em CSS para sobrepor as regras de precedência do cascade e forçar a aplicação de uma declaração de estilo em um seletor. Se for aplicada em um mixin, todas as declarações de estilo contidas no mixin irão ser marcadas como `!important`.

Considere o mixin abaixo:

```
.mask-circle() {
  -webkit-mask-image: url(circle.svg);
  -webkit-mask-origin: padding;
  -webkit-mask-position: 450px 150px;
  -webkit-transform: scale(2);
}
```
Se ele for usado desta forma:

```
.photo {
  border: none;
  .mask-circle() !important;
}
```
Produzirá este resultado em CSS:

```
.photo {
  border: none;
  -webkit-mask-image: url(circle.svg) !important;
  -webkit-mask-origin: padding !important;
  -webkit-mask-position: 450px 150px !important;
  -webkit-transform: scale(2) !important;
}
```

##mixins com parâmetros
Mixins podem receber parâmetros na forma de variáveis com escopo limitado ao bloco. As variáveis têm seus valores definidos quando o mixin é usado. Este tipo de mixin é muito bom para reusar definições com valores diferentes.

No exemplo abaixo, um mixin para redimensionamento que recebe três parâmetros:

```
.scale-2D-2(@amount) {
  -webkit-transform-origin: top left;
  -webkit-transform: scale(@amount);
  -moz-transform-origin: top left;
  -moz-transform: scale(@amount);
  -ms-transform-origin: top left;
  -ms-transform: scale(@amount);
  -o-transform-origin: top left;
  -o-transform: scale(@amount);
  transform-origin: top left;
  transform: scale(@amount);
}
```
O mixin acima pode ser usado da forma:

```
.image2 {
  .scale-2D-2(.75);
}
```
e irá gerar o CSS abaixo:

```
.image2 {
  -webkit-transform-origin: top left;
  -webkit-transform: scale(0.75);
  -moz-transform-origin: top left;
  -moz-transform: scale(0.75);
  -ms-transform-origin: top left;
  -ms-transform: scale(0.75);
  -o-transform-origin: top left;
  -o-transform: scale(0.75);
  transform-origin: top left;
  transform: scale(0.75);
}
```

Um mixin pode ter múltiplos parâmetros separados por *ponto-e-vírgula* (vírgulas também podem ser usadas mas podem causar problemas em alguns casos). Os parâmetros são selecionados pela sua posição.

```
.pular(@duration; @count) {
  -webkit-animation-name: roteiro-pulo;
  -webkit-animation-duration: @duration;
  -webkit-animation-iteration-count: @count;
}
```
Uso do mixin:

```
.anim-pulando {
  .pular(4s; 3);
}
```
Resultado:

```
.anim-pulando {
  -webkit-animation-name: roteiro-pulo;
  -webkit-animation-duration: 4s;
  -webkit-animation-iteration-count: 3;
}
```

Pode-se definir o mixin com um valor *default* para cada variável, que será usado caso os parâmetros não sejam definidos no uso.

No exemplo abaixo o segundo parâmetro do mixin `.pulo-vertical`, `@timing-function` possui um valor *default* `ease-out ` que será usado se o segundo parâmetro não estiver presente.

```
.pulo-vertical(@height; @timing-function: ease-out) {
  -webkit-transform: translateY(@height);
  -webkit-animation-timing-function: @timing-function;
}
```
O mixin é chamado três vezes. Na primeira e última o segundo parâmetro é omitido e o valor *default* é usado.

```
@-webkit-keyframes roteiro-pulo {
  0%   { .pulo-vertical(100px);}
  50%  { .pulo-vertical(50px; ease-in);}
  100% { .pulo-vertical(100px);}
}
```
Resultado em CSS:

```
@-webkit-keyframes roteiro-pulo {
  0% {
    -webkit-transform: translateY(100px);
    -webkit-animation-timing-function: ease-out;
  }
  50% {
    -webkit-transform: translateY(50px);
    -webkit-animation-timing-function: ease-in;
  }
  100% {
    -webkit-transform: translateY(100px);
    -webkit-animation-timing-function: ease-out;
  }
```

Mixins também podem ter parâmetros identificados por nomes. Os nomes sobrepõem a ordem dos elementos. Isto permite que mixins chamados com menos parâmetros, já que os parâmetros são identificados.

O mixin abaixo possui três parâmetros com valores *default*:

```
.reflexo(@begin:left top; @end: left bottom; @final-color:white) {
  -webkit-box-reflect:
       below
       10px
      -webkit-gradient(linear, @begin, @end,
         from(transparent),
         color-stop(0.5, transparent),
         to(@final-color));
```
Pode-se chamar o mixin sem parâmetros ou com menos de três parâmetros, desde que os parâmetros sejam os primeiros. Para chamar o mixin com apenas o último parâmetro, é preciso identificá-lo:

```
.image3 {
  .reflexo(@final-color: black;);
}
```
Desta forma a ordem dos parâmetros não importa. O resultado em CSS será:

```
.image3 {
  -webkit-box-reflect: 
         below 
         10px 
         -webkit-gradient(linear, left top, left bottom, 
                from(transparent), 
                color-stop(0.5, transparent), 
                to(#000000));
}
```
É uma boa prática usar mixins com parâmetros identificados, já que são mais legíveis.

##sintaxes alternativas (e problemáticas)
O Less também permite que se use *vírgula* para separar os parâmetros, mas ele se confunde quando os argumentos passados também contém vírgulas. 

Considere o mixin abaixo:

```
.font-mix(@family, @size: 10pt) {
  font-family: @family;
  font-size: @size;
}
```
Se ele for chamado desta forma:

```
.section1 {
  .font-mix('Times', 12pt);
}
```
O compilador Less irá gerar o CSS abaixo, como esperado:

```
.section1 {
  font-family: 'Times';
  font-size: 12pt;
}
```

Como o segundo argumento possui um valor *default*, é possível omiti-lo caso o valor de `10pt` for desejado:

```
.section2 {
  .font-mix('Times');
}
```
E o resultado também será o esperado:

```
.section2 {
  font-family: 'Times';
  font-size: 10pt;
}
```
Mas, e se o mixin for chamado com uma lista de fontes?

```
.section3 {
  .font-mix('Times', sans-serif);
}
```
O compilador Less não vai reclamar, no entanto vai gerar o CSS incorreto:

```
.section3 {
  font-family: 'Times';
  font-size: sans-serif;
}
```
Se forem passados três ou mais parâmetros, o compilador não gera o CSS e exibe uma mensagem de erro, mas se o número de parâmetros for compatível, não acontece erro e o CSS é gerado incorretamente.

A solução é usar *ponto-e-vírgula*  para separar e terminar os parâmetros nas chamadas. A chamada do mixin em `.section3` acima pode ser corrigida adicionando um *ponto-e-vírgula* no final:

```
.section3 {
  .font-mix('Times', sans-serif;);
}
```

Usando *ponto-e-vírgula*, agora pode-se passar mais parâmetros de fonte sem causar erro no compilador e obtendo o resultado esperado:

```
.section4 {
  .font-mix('Times', 'Times New Roman', sans-serif; 12pt;);
}
```

##mixins com número variável de argumentos
O número de parâmetros aceitos no uso de um mixin depende de como ele foi declarado. A tabela abaixo ilustra algumas situações. `@arg`, `@arg1` e `@argn` representam variáveis genéricas e *...* (em itálico) representa zero ou mais argumentos. Já `@arguments`, `...` e `@variavel... `são variáveis especiais do Less que serão abordadas nesta seção.

Sintaxe usada na declaraçao do mixin | No. args | Variáveis criadas
:-|
`.mixin` ou `.mixin()` | 0 | Nenhuma
`.mixin(@arg)` | 1 | `@arg` e `@arguments`
`.mixin(@arg1;`  *...*  `; @argn)` | n | @`arg1`, ..., `@argn` e `@arguments`
`.mixin(@arg:` *valor*`)` | 0..1 | `@arg` e `@arguments`
`.mixin(@arg1:` *valor*; *...* `;@argn:` *valor*`;)` | 0..n | `@arg1`, ..., `@argn` e `@arguments`
`.mixin(...)` | 0..* | `@arguments`
`.mixin(@arg; ...)` | 1..* | `@arg` e `@arguments`
`.mixin(@arg1;`  *...*  `; @argn; ...)` | n..* | `@arg1`, ..., `@argn` e `@arguments`
`.mixin(@arg:` *valor*`; ... )` | 0..* | `@arg` e `@arguments`
`.mixin(@arg; @restante...)` | 1..* | `@arg | @restante` = `@arguments`

Se o número de argumentos passados para um mixin for incompatível com o número esperado de argumentos (segundo a tabela acima), o compilador Less não irá gerar o CSS e exibirá uma mensagem de erro. Mas não há verificação de tipo de dados, portanto se os argumentos forem enviados na ordem errada, eles serão atribuídos a variáveis incorretas e não irão gerar o CSS esperado. Por essa razão é uma boa prática usar argumentos com nome, e valores *default* nos mixins com muitos argumentos.

### @arguments

`@arguments` é uma variável que existe em todos os mixins que aceitam argumentos, e contém *todos* os argumentos recebidos. Dentro do mixin pode-se tanto usar as variáveis individuais como todas juntas na ordem em que foram recebidas. As variáveis de `@arguments` podem ser usadas em loops ou diretamente nas propriedades. No CSS resultante elas serão separadas por espaços.

```
.borda (@estilo: solid; @cor: black; @espessura: 1px) {
  border: @arguments;
  background: lighter(@cor, 50%)
}
```
As duas classes abaixo usam este mixin. A primeira não passa parâmetros e usa os valores *default*. A segunda altera apenas um parâmetro:

```
.sec1 {
  .borda();
}

.sec2 {
  .borda(@cor: red);
}
```
Este é o resultado em CSS:

```
.sec1 {
  border: solid #000000 1px;
  background: #808080;
}
.sec2 {
  border: solid #ff0000 1px;
  background: #ffffff;
}
```
Este outro mixin pre-define vários argumentos para uma margem, inicializados com um valor default (`2px`):

```
.margem(@top: 2px; @right: 2px; @bottom: 2px; @right: 2px) {
  margin: @arguments;
}
```
Ele pode ser chamado sem argumentos para usar os valores *default*, ou pode escolher qualquer um ou mais parâmetros para redefinir. O seletor `.sec3` abaixo mudou apenas o terceiro argumento para zero:

```
.sec3 {
  .margem(@bottom: 0);
}
```
O mixin garante que a substuição em CSS ocorre no lugar correto:

```
.sec3 {
  margin: 2px 2px 0 2px;
}
```

###... (três pontos)
Mixins que recebem um número variável de argumentos podem ser declarados com reticências (três pontos) dentro dos parênteses:

```
.padding-e-margem(...) {
  margin:@arguments;
  padding: @arguments;
}
```
A chamada pode conter de zero a muitos argumentos:

```
.sec4 {
  .padding-e-margem(1;2);
}
```
O CSS resultante contém os argumentos na ordem em que foram enviados:

```
.sec4 {
  margin: 1 2;
  padding: 1 2;
}
```
Um problema desse mixin é que ele aceita que não sejam passados argumentos. Se ele for chamado desta forma:

```
.sec4 {
  .padding-e-margem; 
}
```
O resultado será um CSS incorreto:

```
.sec4 {
  margin: ;
  padding: ;
}
```

Para evitar isto, pode-se declarar um ou mais parâmetros e usar `...` como último argumento. A variável `@arguments` irá conter todos os argumentos, inclusive os que têm variáveis declaradas.

O mixin abaixo garante que haverá ao menos um argumento válido:

```
.transmix(@transform: scale(1); ...) {
  -webkit-transform: @arguments;
  -moz-transform: @arguments;
  -o-transform: @arguments;
  -ms-transform: @arguments;
  transform: @arguments;
}
```
Se for chamado sem argumentos:

```
.sec5 {
  .transmix;
}
```

irá produzir:

```
.sec5 {
  -webkit-transform: scale(1);
  -moz-transform: scale(1);
  -o-transform: scale(1);
  -ms-transform: scale(1);
  transform: scale(1);
}
```

Mas se forem passados argumentos, eles terão precedência:

```
.sec6 {
  .transmix(scale(0.5); translate(3); rotateX(25deg));
}
```
Resultado:

```
.sec6 {
  -webkit-transform: scale(0.5) translate(3) rotateX(25deg);
  -moz-transform: scale(0.5) translate(3) rotateX(25deg);
  -o-transform: scale(0.5) translate(3) rotateX(25deg);
  -ms-transform: scale(0.5) translate(3) rotateX(25deg);
  transform: scale(0.5) translate(3) rotateX(25deg);
}
```

O número mínimo de argumentos é definido pelas variáveis explícitas antes dos três pontos. Cada uma receberá o valor passado na posição correspondente ou pelo seu nome.

### @variavel...
`@arguments` sempre contém todos os argumentos. Mas declarando uma variável seguida de `...`, por exemplo `@args...` em vez de `...` como último argumento, é possível ter uma lista contendo apenas os argumentos que não foram declarados em variáveis. Esta lista está disponível dentro do mixin na variável declarada (sem as reticências), por exemplo `@args`.

O mixin abaixo usa o primeiro argumento para definir a propriedade `border`. Os argumentos restantes são usados para definir `margin`:

```
.borda-com-margem(@borda; @outros-args...) {
  .border: @borda;
  margin: @outros-args;
}
```
Abaixo ela está sendo chamada com cinco argumentos:

```
.sec7 {
  .borda-com-margem(solid black 1px; 5; 10; 15; 5;);
}
```
E o resultado em CSS será:

```
.sec7 {
  border: solid #000000 1px;
  margin: 5 10 15 5;
}
```

##sobrecarga de mixins 
Mixins podem ter nomes iguais e serem selecionados com base na quantidade de argumentos declarados.

```
.mancha(@alpha: 1){    // 0 ou 1 argumento
  background: black;
  color: white;
}
.mancha(@gray; @alpha: 1) {    // 1 ou 2 argumentos
  @yarg: 255 - @gray;
  background: rgb(@gray,@gray,@gray);
  color: rgb(@yarg, @yarg, @yarg);
}
.mancha(@red; @green; @blue; @alpha: 1) {    // 3 ou 4 argumentos
  @der   : 255 - @red;
  @neerg : 255 - @green;
  @eulb  : 255 - @blue;
  background: rgb(@red, @green, @blue);
  color: rgb(@der, @neerg, @eulb);
}
```

Os mixins serão selecionados de acordo com a correspondência do número de argumentos. Se houver ambiguidade, o compilador não irá gerar o CSS e exibirá uma mensagem de erro.

```
.sec3 {
    .mancha;
}

.sec4 {
    .mancha(32;0.5)
}

.sec5 {
    .mancha(128;64;32)
}
```
Resultado em CSS:

```
.sec3 {
  background: black;
  color: white;
}
.sec4 {
  background: #202020;
  color: #dfdfdf;
}
.sec5 {
  background: #804020;
  color: #7fbfdf;
}
```

Pode-se também selecionar mixins de mesmo nome com parâmetros fixos. Os mixins `.caixa()` abaixo têm o primeiro parâmetro fixo que é usado para distingui-los. O segundo parâmetro é variável e pode ser omitido. O último mixin `.caixa()` recebe dois parâmetros quaisquer, e o segundo é opcional. Qualquer chamada a mixins `.caixa()` com um ou dois argumentos irá selecioná-los. Se a chamada contiver no primeiro argumento algum dos parâmetros fixos, este também será selecionado e as propriedades serão misturadas (sem nenhuma garantia que não haverá repetição).

```
.caixa-base(@espessura: 1px; @cor:black) {
  margin: @espessura;
  padding: @espessura;
  border: solid @cor @espessura;
}

.caixa(grande; @cor:black) {
  .caixa-base(4px; @cor);
}

.caixa(media; @cor:black) {
  .caixa-base(2px; @cor);
}

.caixa(pequena; @cor:black) {
  .caixa-base(1px; @cor);
}

.caixa(zero; @cor:black) {
  .caixa-base(0; @cor);
}

.caixa(@todas; @todas: xyz) {
  position: absolute;
  top: 0; left: 0;
}
```

Pode-se então selecionar o mixin usando o nome do argumento fixo como parâmetro. A chamada abaixo irá executar também o mixin `.caixa(@todas, @todas: xyz)` que combina com qualquer chamada de um ou dois argumentos. 

```
.sec2 {
  .caixa(pequena;red);
}
```
O resultado será o CSS abaixo:

```
.sec2 {
  margin: 1px;
  padding: 1px;
  border: solid #ff0000 1px;
  position: absolute;
  top: 0;
  left: 0;
}
```
Se os mixins ficarem em um documento global importado por outros documentos locais, o parâmetro fixo pode ser armazenado em uma variável e ser definido em um único lugar no documento:

```
@import url(mixins-11-matching.less);
@tipo-caixa: grande; // definição do tamanho default para esta folha de estilos

.sec1 {
  .caixa(@tipo-caixa);
}
```
Resultado em CSS:

```
.sec1 {
  margin: 4px;
  padding: 4px;
  border: solid #000000 4px;
  position: absolute;
  top: 0;
  left: 0;
}
```

##mixins para CSS multi-browser
Mixins são ótimos para encapsular propriedades dependentes de browser. Essas propriedades têm um prefixo proprietário e geralmente recebem os mesmos dados. É possível encapsular em um mixin as propriedades relativas a diversos fabricantes diferentes, e chamar a propriedade pelo nome do mixin. 

O mixin abaixo gera a propriedade do CSS3 transform com um número mínimo de argumentos para quatro browsers diferentes:

```
.transform(@transform:scale(1); ...) {
  -webkit-transform: @arguments;
  -moz-transform: @arguments;
  -o-transform: @arguments;
  -ms-transform: @arguments;
  transform: @arguments;
}
```
Exemplo de uso:

```
.secao {
  .transform(
    rotate(45deg);
    scale(0.5);
    translate(100px,100px);
    skewY(10deg);
  )
}
```
Resultado da geração do CSS:

```
.secao {
  -webkit-transform: rotate(45deg) scale(0.5) translate(100px, 100px) skewY(10deg);
  -moz-transform: rotate(45deg) scale(0.5) translate(100px, 100px) skewY(10deg);
  -o-transform: rotate(45deg) scale(0.5) translate(100px, 100px) skewY(10deg);
  -ms-transform: rotate(45deg) scale(0.5) translate(100px, 100px) skewY(10deg);
  transform: rotate(45deg) scale(0.5) translate(100px, 100px) skewY(10deg);
}
```

##mixins que retornam valores
Variáveis definidas no corpo de mixins têm seu escopo propagado no bloco que faz a chamada, ou seja, os valores dessas variáveis podem ser lidas no bloco destino.

Por exemplo, o mixin abaixo define sete variáveis. Quatro como parâmetros e quatro no corpo do mixin. 

```
.mancha(@red; @green; @blue; @alpha: 1) { // 3 ou 4 argumentos
  @der   : 255 - @red;
  @neerg : 255 - @green;
  @eulb  : 255 - @blue;
  @opacity: @alpha;
  background: rgb(@red, @green, @blue);
  color: rgb(@der, @neerg, @eulb);
}
```
As quatro variáveis que foram declaradas no corpo são visíveis no bloco que chama o mixin. O seletor abaixo utiliza `@opacity` e `@neerg`:

```
.sec8 {
  .mancha(255,128,64);
  opacity: @opacity;
  border-color: @neerg;
}
```
Resultado em CSS:

```
.sec8 {
  background: #ff8040;
  color: #007fbf;
  opacity: 1;
  border-color: 127;
}
```

As variáveis dentro de mixins podem ser usadas para transformar os valores recebidos. Assim o mixin se comporta como uma função. 

Dois mixins para conversão de coordenadas cartesianas e polares:

```
.polar(@x, @y) {
  @r: sqrt(@x*@x + @y*@y);
  @theta: convert(atan(@y / @x), deg);
}

.cartesian(@r, @theta) {
  @x: @r * cos(@theta);
  @y: @r * sin(@theta);
}
```

Usando os mixins para fazer conversões:

```
.sec6:before {
  .polar(3, 4);
  content: 'r: @{r} theta: @{theta}';
}

.sec7:before {
  .cartesian(5, convert(53.13010235deg, rad));
  content: 'x: @{x} y: @{y}';
}
```
Resultado em CSS:

```
.sec6:before {
  content: 'r: 5 theta: 53.13010235415598deg';
}
.sec7:before {
  content: 'x: 3.0000000002901417 y: 3.9999999997823936';
}
```
Estes mixins utilizam funções matemáticas e de conversão que fazem parte do Less e serão abordadas mais adiante.

## mixins com extend
`:extend` pode ser usado dentro de mixins, permitindo que o mixin estenda os seletores do bloco onde é declarado com propriedades de outros seletores. Por exemplo:

```
.label:before > input:hover {
  content: 'hovering';
}

.list-style() {
  &:extend(.label:before > input:hover);
}
```

A chamada do mixin estende os seletores do bloco onde é usado com as propriedades dos seletores correspondentes:

```
.menu-item, .nav-item {
  .list-style();
}
```

Resultado em CSS:

```
.label:before > input:hover, .menu-item, .nav-item {
  content: 'hovering';
}
```

##agregação de valores
Less permite que valores de propriedades definidas em um mixin sejam *agregadas* aos valores existentes de propriedades dos seletores onde o mixin é usado, em uma lista separada por vírgulas. Isto é útil para propriedades que aceitam valores separados por vírgulas como `font-family` e outros.

Para usar, é necessário incluir um sinal `+` tanto na propriedade declarada no mixin, quanto na propriedade declarada no seletor afetado. 

Por exemplo, o mixin abaixo define uma transição multi-plataforma sobre a propriedade `transform` do CSS3:

```
.transform-transition(@arg) {    @duration: @arg;    -webkit-transition-property+: -webkit-transform, -moz-transform,  transform;    -webkit-transition-duration+: @duration, @duration, @duration;}
```

Aqui ele é usado como base para adicionar mais um atributo à transição:

```
.secao1 {   .transform-transition(2s);   -webkit-transition-property+: opacity;   -webkit-transition-duration+: @duration;} 
```

O resultado em CSS acrescenta os atributos adicionados no fim da lista separada por vírgulas:

```
.secao1 {  -webkit-transition-property: -webkit-transform, -moz-transform,  transform, opacity;  -webkit-transition-duration: 2s, 2s, 2s, 2s;} 
```

##exercícios
1. Escreva um mixin para aplicar uma aparência de alto relevo em botões. O...
2. Escreva um mixin que receba duas cores, e uma lista variável de percentagens. O mixin deve então gerar um gradiente de uma cor para a outra, usando as percentagens como indicativos de stops para cada cor.
3. Veja a folha de estilos exercicio1.css. Escreva uma folha de estilos LESS que obtenha os mesmos resultados com o máximo de reuso, usando recursos que vimos até agora (variáveis, extend e mixins).





