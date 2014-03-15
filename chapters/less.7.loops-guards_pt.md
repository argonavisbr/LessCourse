#{7: loops.guards }

Neste capítulo serão explorados recursos do Less que permitem realizar operações condicionais e loops.

##mixins condicionais (mixin guards)
Pode-se criar mixins parametrizados que definem regras de estilo de acordo com operações condicionais realizadas sobre os valores recebidos. São chamados de _mixin guards_. Através deles é possível tomar decisões de forma similar a expressões *if* e *if/else* existentes em linguagens de programação.

A expressão condicional é representada por um *teste* seguindo a palavra-chave `when` ('quando'). Assim pode-se representar expressões do tipo "defina o mixin x *quando* a expressão y for verdadeira" usando a seguinte sintaxe:

```
.mixin(@parametros) when (condição sobre @parametros) {
    regras;
}
```

Veja um exemplo:

```
.page (@fundo) when (@fundo = black) {
   background: @fundo;
   color: white;
}

.page (@fundo) when (@fundo = white) {
  background: @fundo;
  color: black;
}
```
Uso do mixin condicional:

```
.noite {
   .page(rgb(0,0,0));
 }

.dia {
  .page(rgb(255,255,255));
}
```
Resultado em CSS:

```
.noite {
  background: #000000;
  color: white;
}
.dia {
  background: #ffffff;
  color: black;
}
```

Vale lembrar que os mixins condicionais geram *CSS estático*. Em Web sites responsivos muitas vezes o que se deseja é um comportamento dinâmico. Usando media queries é possível controlar o efeito do CSS em diferentes dispositivos, resoluções e dimensões de tela. Portanto, um mixin condicional como:

```
.skin(@width) when (@width >= 500px) {
  column-count:2;
}
.skin(@width) when (@width < 500px) {
  column-count:1;
}
```

que parece ser variável, é na verdade aplicado com um valor estático antes da compilação do Less:

```
.sec {
   .skin(600px);
}
```

e gera o CSS abaixo, invariável.

```
.sec {
  column-count: 2;
}
```

Para um Web site responsivo, o ideal é definir o número de colunas de acordo com a largura de tela do dispositivo em tempo real, que é possível através de media queries do CSS:

```
@media screen and (min-width: 500px) {
  .sec  {
    column-count:2;
  }
}

@media screen and (max-width: 500px) {
  .sec {
    column-count:1;
  }
}
```

### operadores e funções condicionais
Para testar condições pode-se se usar os seguintes operadores: `>`, `>=`, `<`, `<=`, `=`.

```
@max-duration: 10s;
@max-repeats: 2;

.anim (@duration) when (@duration >= @max-duration) {
  animation-duration: @duration;
  animation-iteration-count:1;
}

.anim (@duration) when (@duration < @max-duration) {
  animation-duration: @duration;
  animation-iteration-count:@max-repeats;
}

.symbol {
  .anim(15s);
}
```

Resultado em CSS:

```
.symbol {
  animation-duration: 15s;
  animation-iteration-count: 1;
}
```

Para inverter o resultado de um teste, pode-se usar `when not`, que inverte o resultado da expressão condicional.

No exemplo abaixo, o mixin testa se a variável recebida é ou não um número e chama o mixin adjust que define uma propriedade se o valor retornado *não* for falso (as palavras `true` e `false` são retornadas por funções como `isnumber` mas elas não têm um significado especial em Less):

```
@weight: bold;
.adjust(@test) when not (false) {
   font-weight:@weight;
}

.section {
  font-size: 12pt;
  .adjust(isnumber(@weight));
}
```

Em `.section` o mixin é chamado com o resultado do teste de `isnumber`, que retorna `false` já que `bold` não é número. O resultado em CSS é:

```
.section {
  font-size: 12pt;
  font-weight: bold;
}
```

### detecção de tipos e unidades
Além de `isnumber` existem quatro outras funções usadas para testar o tipo de um valor:

Função | Recebe | Retorna
--|--|--
`iscolor` | string | `true` se valor for uma cor, ou seja, se o string contiver `#rrggbb`, `rgb(%,%,%)`, `rgba(r,g,b,a)`, *nome-de-cor*, etc. `false` caso contrário.
`iskeyword` | string | Quaisquer nomes, sejam de seletores, variáveis, comandos do less, retornam `true`. Retorna `false` se valor for string, cor, número ou url; `true` caso contrário. 
`isnumber` | string | `true` se valor for um número: 12, 12.3, 12%, 12cm, 12/36. `false` se não for.
`isstring` | string | `true` se o valor estiver entre aspas ou apóstrofes. `false` se não for string.
`isurl` | string | `true` se valor for uma url. `false` se não for.

Alguns exemplos usando as funções acima:

```
.istest {
  content: '1. string'  isstring('12345');
  content: '2. string'  isstring(12345);
  content: '3. keyword' iskeyword(flowerblue);
  content: '4. keyword' iskeyword(cornflowerblue);
  content: '5. number'  isnumber(12.77);
  content: '6. number'  isnumber(~'123');
  content: '7. color'   iscolor(cornflowerblue);
  content: '8. color'   iscolor(~'rgba(100%,100%,100%,1)');
  content: '9. url'     isurl(url(image.jpg));
  content: '10.url'     isurl(~'/image/jpg');
}
```

CSS resultante:

```
.istest {
  content: '1. string'  true;
  content: '2. string'  false;
  content: '3. keyword' true;
  content: '4. keyword' false;
  content: '5. number'  true;
  content: '6. number'  false;
  content: '7. color'   true;
  content: '8. color'   false;
  content: '9. url'     true;
  content: '10.url'     false;
}
```

Para *números* existem ainda quatro funções que retornam `true` ou  `false` de acordo com as unidades que os qualificam:

Função | Recebe | Retorna
--|--|--
`ispixel(n)` | número | `true` se unidade é `px`; `false` caso contrário.
`isem(n)` | número | `true` se unidade é `em`; `false` caso contrário.
`ispercentage(n)` | número | `true` se unidade é `%`; `false` caso contrário.
`isunit(n,u)` | n = número, u = unidade | `true` se o número está na unidade especificada; `false` caso contrário.

Exemplos:

```
@pixvalue: 1px * 30 / 2;
@cmvalue: 10cm + 10pt + .01m;
.istest2 {
  content: '1. pixel'  ispixel(@pixvalue);
  content: '2. pixel'  ispixel(convert(@pixvalue, 'pt'));
  content: '3. em' isem(12em + 12pt);
  content: '4. em' isem(12pt + 12em);
  content: '5. percentage'  ispercentage(100%);
  content: '6. percentage'  ispercentage(0.5);
  content: '7. unit'  isunit(@cmvalue, 'cm');
  content: '8. unit'  isunit(10ms, 's');
}
```

CSS:

```
.istest2 {
  content: '1. pixel' true;
  content: '2. pixel' false;
  content: '3. em' true;
  content: '4. em' false;
  content: '5. percentage' true;
  content: '6. percentage' false;
  content: '7. unit' true;
  content: '8. unit' false;
}
```

### combinação de expressões condicionais

Várias expressões condicionais podem ser combinadas com a vírgula e têm efeito somatório *OR* (*'ou' lógico*), ou seja, se qualquer uma das expressões resultar em verdadeira, a expressão inteira é aceita.

```
skin(@cor, @tambase) when (lightness(@cor) > 50%), (@tambase > 20) {
  font-size: convert(@tambase, "pt");
  color: darken(@cor, 80%);
  background: @cor;
}

.sec1 {
   font-family: serif;
  .skin(#d0d0d0, 2); // true OU false = true
}
.sec2 {
  font-family: serif;
  .skin(#101010, 21); //  false OU true = true
}
.sec3 {
  font-family: serif;
  .skin(#101010, 19);  // false OU false = false
}
```
Resultado em CSS:
```
.sec1 {
  font-family: serif;
  font-size: 2;
  color: #040404;
  background: #d0d0d0;
}
.sec2 {
  font-family: serif;
  font-size: 21;
  color: #000000;
  background: #101010;
}
.sec3 {
  font-family: serif;
}
```

O efeito multiplicador *AND* (*'e' lógico*) na combinação de expressões é obtido com a palavra-chave `and`:

```
.mix(@um, @dois) when (iscolor(@um)) and (ispixel(@dois)) {
  margin: @dois;
  color: @um;
}

.sec4 {
  font-family: monospace;
  .mix(red, 2px); // true E true = true
}
.sec5 {
  font-family: monospace;
  .mix(red, 2pt); // true E false = false
}
```
Resultado em CSS:

```
.sec4 {
  font-family: monospace;
  margin: 2px;
  color: #ff0000;
}
.sec5 {
  font-family: monospace;
}
```

A função `default()` é usada como cláusula "else" de um mixin guard. Representa uma condição *default* que será executada se nenhum outro mixin condicional de mesmo nome/parâmetros resultar em `true`. 

```
.mix(@um, @dois) when (iscolor(@um)) and (ispixel(@dois)) {
  margin: @dois;
  color: @um;
}
.mix(@um, @dois) when(default()) { // caso nenhum mixin seja selecionado
  margin: 0;
  color: black;
}
```
Aplicando agora o mixin `.mix` nos seletores abaixo:

```
.sec4 {
  font-family: monospace;
  .mix(red, 2px); // true E true = true
}
.sec5 {
  font-family: monospace;
  .mix(red, 2pt); // true E false = false
}
```
Obtém-se o CSS:

```
.sec4 {
  font-family: monospace;
  margin: 2px;
  color: #ff0000;
}
.sec5 {
  font-family: monospace;
  margin: 0;
  color: black;
}
```

Expressões condicionais não são exclusividade dos mixins. Elas podem ser aplicadas diretamente nos seletores CSS. 

```
@debug: true;

div when (@debug) {
   border: solid red 1px;
}
```

Quando @debug for true bordas vermelhas serão colocadas em volta de cada `div`:

```
div {
  border: solid red 1px;
}
```

Esse tipo de expressão só pode ser aplicada em um único seletor de cada vez. Para aplicar a vários seletores, deve-se usar o `&`:

```
& when (@debug) {
  section, footer, aside, div {
    border: solid red 1px;
  }
  p:before {
     content:'[P]';
  }
  p:after {
    content:'[/P]';
  }
}
```
Resultado em CSS:

```
section, footer, aside, div {
  border: solid red 1px;
}
p:before {
  content: '[P]';
}
p:after {
  content: '[/P]';
}
```

##mixins recursivos (loops)
Loops em LESS são chamadas recursivas de mixins, ou seja, o mixin chama ele mesmo. Para que o loop não seja infinito, é necessário que a cada iteração uma condição seja testada e que o valor testado mude para que em algum momento a condição seja falsa, e o loop tenha fim.

O loop abaixo repete um número de vezes definida quando o mixin for chamado. A cada repetição, o contador é comparado com zero, e se for maior que zero, seu valor é diminuído de um e o corpo do mixin é executado. Quando o valor for zero, o mixin termina.

```
.repetir(@vezes) when (@vezes > 0) {
  .repetir(@vezes - 1);
  margin: (@vezes);
}
```
No bloco abaixo o mixin é chamado para executar quatro vezes:

```
.sec2 {
  .repetir(4);
}
```
E o resultado do CSS é:

```
.sec2 {
  margin: 1;
  margin: 2;
  margin: 3;
  margin: 4;
}
```

Loops também podem ser usados para gerar blocos inteiros de seletores. O mixin  abaixo permite gerar até 5 seções, cada uma com uma cor de fundo mais escura.

```
.criar-secoes(@vezes) when (@vezes > 0) and (@vezes <= 5) {
  .secao-@{vezes} {
     color: (darken(#ff0000, 10% * @vezes));
  }
  .criar-secoes( (@vezes - 1) );
}

.criar-secoes(5);
```
Ele é chamado logo em seguida para repetir 5 vezes e gera o seguinte CSS:

```
.secao-5 {
  color: #000000;
}
.secao-4 {
  color: #330000;
}
.secao-3 {
  color: #660000;
}
.secao-2 {
  color: #990000;
}
.secao-1 {
  color: #cc0000;
}
```

Este exemplo ilustra o uso de loops para gerar uma seqüência para animação:

```
@positions: 0px 50px 100px 50px 0px;

.make-keyframe(@step, @count) when (@count > 0) {
  .make-keyframe(@step, (@count - 1) );
  @pos: @step * (@count - 1);
  @{pos} {top: extract(@positions, @count);}
}
```
Uso do mixin:

```
@keyframes anim
{
  .make-keyframe(25%, length(@positions));
}
```
Resultado em CSS:

```
@keyframes anim {
  0% {
    top: 0px;
  }
  25% {
    top: 50px;
  }
  50% {
    top: 100px;
  }
  75% {
    top: 50px;
  }
  100% {
    top: 0px;
  }
}
```

##exercícios
1. x
2. x
3. x
4. x
5. x
6. x