#{9: +less }
Este capítulo contém tópicos que não foram abordados em módulos anteriores.

##imports
Diferentemente do CSS, diretivas de import podem ser colocadas em qualquer lugar do arquivo less. A geração do CSS irá posicioná-las no início do arquivo corretamente (embora seja uma boa prática sempre mantê-los no início do arquivo).

A sintaxe é a mesma do CSS:

```
@import "documento.css";
```
Less também permite que o arquivo importado seja um documento Less. Neste caso, ele funcionará como se o arquivo importado estivesse anexado antes do arquivo que o importa.

Se a extensão do arquivo for .css, o less irá tratá-lo como um @import comum do CSS, e simplesmente copiar a declaração para o CSS resultante. Se for qualquer outra extensão, o arquivo será incorporado e tratado como sendo less. Se o arquivo for importado sem extensão, o less irá anexar uma extensão .less a ele e buscar incorporar o arquivo.

Pode-se sobrepor o comportamento default do import com seis opções que são passadas entre parênteses entre a diretiva @import e o nome do arquivo, da forma:

```
@import (parametro) "arquivo";
```
Os seguintes parametros podem ser usados:

- **css**: trata o arquivo como CSS (independente de sua extensão)
- **inline**: inclui o conteúdo do arquivo no CSS final, mas não o processa
- **less**: trata o arquivo como LESS (independente de sua extensão)
- **multiple**: aceita que um um arquivo seja importado mais de uma vez
- **once**: permite importa o arquivo uma vez e ignora outras tentativas
- **reference**: inclui e usa o arquivo LESS mas não utiliza o conteúdo na saída

Exemplo:

```
import (reference) 'biblioteca.less';
```

### import e variáveis

Se a importação resultar em uma variável definida mais de uma vez, a última definição, considerando o escopo do atual para o mais externo, é usada. Por exemplo, na pagina `more-3-imports-base.less` foram definidas as seguintes variáveis:

```
@base: #ff0000;
@background: darken(@base, 20%);
@foreground: lighten(@base, 20%);
```

Em uma outra folha de estilos Less, essa biblioteca é importada:

```
@import "more-3-imports-base.less";
@base: blue;

.sec {
  background-color: @background;
  color: @foreground;
}
```

O valor de `@base` foi redefinido, mas `@background` e `@foreground` não foram redefinidas. Elas são calculadas com base no valor *atual* de `@base`, que agora é `#0000ff`. O resultado em CSS é:

```
.sec {
  background-color: #000099;
  color: #6666ff;
}
```

### Bibliotecas úteis
Existem várias bibliotecas de mixins para o Less que podem ser incorpodados em quaisquer projetos Less, evitando a necessidade de se criar mixins óbvios como os que suportam recursos do CSS3 em múltiplos browsers. A maioria consiste de apenas um arquivo `.less` que deve ser importado. O mais simples é o **less elements**. Para usar, baixe-o do site <http://lesselements.com> e importe no seu projeto:

```
@import "elements.less";
```
Com isso pode-se usar qualquer mixin disponível, por exemplo:

```
.secao {
    .opacity(0.9); // usando mixin do less elements
    background-color: blue;
```

Se você precisar de mais mixins, há pelo menos três outras opções mais completas (2014): **Less Hat**, **Clearless** e **Preboot**, além do próprio **Twitter Bootstrap** que inclui mixins reutilizáveis em vários arquivos `.less` que podem ser importados.

## acessibilidade, performance e suporte multi-plataforma
Esta seção aborda algumas funções que ajudam a desenvolver Web sites accessíveis, mais eficientes e independentes de browser.

### contraste
A função contrast recebe uma cor e devolve outra que faça contraste com a outra (default é 43% de diferença). Apenas a cor precisa ser passada como parâmetro, mas também pode-se informar um par de cores como referências de claro/escuro, e a percentagem de contraste desejada.

Função | Recebe | Retorna
--|--|--
`contrast(c)`  `contrast(c,c1,c2)` `contrast(c,c1,c2,p)`| `c` = a cor que servirá de parâmetro para calcular o contraste. `c1, c2` = cores de referência claro/escuro (opcionais) - default é preto/branco. `p` = percentagem de contraste(opcional) - default é 43% | cor contrastante em relação à cor de referência.

O mixin abaixo é usado para aplicar um estilo com cor de fundo e texto contrastante com a cor de fundo:

```
.labeled-box(@color, @label) {
  background-color: @color;
  &:before {
    content: @label;
    color: contrast(@color);
  }
}
```

Pode ser usado para garantir que o texto seja sempre visível:

```
.sec-1 {
  .labeled-box(red, '#ff0000')
}

.sec-2 {
  .labeled-box(yellow, '#ffff00')
}

.sec-3 {
  .labeled-box(blue, '#0000ff')
}
```

CSS gerado:

```
.sec-1 {
  background-color: #ff0000;
}
.sec-1:before {
  content: '#ff0000';
  color: #ffffff;
}
.sec-2 {
  background-color: #ffff00;
}
.sec-2:before {
  content: '#ffff00';
  color: #000000;
}
.sec-3 {
  background-color: #0000ff;
}
.sec-3:before {
  content: '#0000ff';
  color: #ffffff;
}
```


### conversão de cor para formato Microsoft
A função `argb` converte uma cor em uma representação de cor proprietária compatível com aplicações *Microsoft* e *Internet Explorer*. Pode ser usada em um mixin que gera declarações CSS independentes de browser. Nesta representação o fator alfa é um número hexadecimal entre 00 e FF e é representado antes dos pares hexadecimais dos componentes RGB. O formato resultante é `#aarrggbb`.

Função | Recebe | Retorna
--|--|--
`argb(a,r,g,b)` | `a` = número entre 0 e 1 ou percentagem, `r,g,b` = números (0-255) ou percentagem | cor em hexadecimal no formato `#aarrggbb`. Ex: `argb( rgba(255,0,0,0.5) )` gera `#80ff0000`.

### conversao data-uri
Esta função converte uma URL de imagem em um recurso embutido no CSS (`url(data)`). A imagem deve existir em um caminho relativo à folha de estilo, e será localizada pelo processador Less que irá gerar uma representação da mesma em Base 64 e embutir os bytes diretamente na folha CSS gerada.

```
@uri: data-uri('../../images/less-logo.png');
.sec10 {
  image: @uri;
}
```
Resultado em CSS:

```
.sec10 {
  image: image: url("data:image/png;base64,iVBORw0KGgoAAAANSUh ... rkJggg==");
}
```
Isto pode ser útil em imagens que são utilizadas muitas vezes pois é mais eficiente que fazer download da imagem em outra requisição,

##bibliotecas de mixins com namespaces

Um namespace em less é declarado com um mixin usando seletor de ID. Namespaces são úteis para agrupar outros mixins e evitar conflitos de nome, principalmente em aplicações que importam folhas de estilo de terceiros.

Abaixo estão dois mixins de mesmo nome, porém em namespaces diferentes:

```
#circular {
  .mask() {
    -webkit-mask-image: url(circle.svg);
  }

}

#rectangular {
  .mask() {
    -webkit-mask-image: url(rectangle.svg);
  }
}
```

Para usar deve-se usar o nome do mixin externo (namespace) anexado ao nome do mixin interno (pode-se também usar a notação `#externo > .interno`).

```
.photo1 {
   border: none;
   #circular.mask();
 }

.photo2 {
  border: none;
  #rectangular.mask();
}
```

Resultado em CSS:

```
.photo1 {
  border: none;
  -webkit-mask-image: url(circle.svg);
}
.photo2 {
  border: none;
  -webkit-mask-image: url(rectangle.svg);
}
```

## uso de less no browser
Less pode ser carregado no browser através do módulo less.js que pode ser baixado do site <http://lesscss.org> ou vinculando com o `less.js` disponível na nuvem:

```
 <script src="//cdnjs.cloudflare.com/ajax/libs/less.js/1.7.0/less.min.js">
 </script>
```
Quaisquer folhas de estilo Less em uso na página que precisem ser processadas pelo script, devem ser carregadas na página *antes* dessa chamada:

```
<link rel="stylesheet/less" type="text/css" href="estilo1.less" />
<link rel="stylesheet/less" type="text/css" href="estilo2.less" />
```

### desenvolvimento e depuração no browser
Para trabalhar com Less em tempo de desenvolvimento, e ver os resultados no browser sempre que uma alteração for feita na folha de estilos, ligue o modo `watch`  chamando `less.watch()` *depois* de carregar o `less.js`:

```
 <script>less.watch()</script>
```
Uma extensão para o FireBug, o [FireLess](https://addons.mozilla.org/en-us/firefox/addon/fireless/) permite obter informações detalhadas de erros, números de linha onde os erros ocorreram, etc. quando se trabalha com Less.

##execução de javascript via less
Existe um recurso não documentado que permite escapar blocos no Less e executar trechos de JavaScript. Não é possível executar trechos grandes nem funções externas (a menos que se esteja usando Less no browser ou em uma aplicação Node.js). Como é um recurso que não está mais documentado no site (desde Less 1.6), é possível que sofra modificações ou deixe de ser suportado no futuro.

Para executar JavaScript dentro de uma folha de estilos Less, chame o JavaScript dentro de crases:

```
@texto: "Digite aqui!";
p.editor {
  &:before {
    content: `@{texto}.toUpperCase()`;
  }
}
```

As variáveis podem ser passadas de forma interpolada, como em strings. O método `toUpperCase()` em JavaScript põe a string em caixa alta. O resultado em CSS é:

```
p.editor:before {
  content: "DIGITE AQUI!";
}
```

É possível fazer coisas bem mais sofisticadas, e contornar diversas limitações do Less, pagando o preço do aumento da complexidade. 

Este outro exemplo é um mixin que gera propriedades e valores:

```
@coords: "TOP LEFT BOTTOM RIGHT";
.center(@position) {
  margin: auto !important;
  position: ~`'@{position};\n  ' + @{coords}.toLowerCase().split(' ').join(': 0;\n  ') + ': 0'`;
}
```

Uso do mixin:

```
.section {
  .center(absolute);
}
```
Resultado em CSS:

```
.section {
  margin: auto !important;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}
```

### acesso ao DOM do browser via JavaScript

JavaScript em Less *não* garante acesso ao DOM, apenas ao *core* Javascript. Pode-se transformar strings, arrays, gerar datas e fazer contas matemáticas, mas não é possível ler variáveis do modelo de objetos:

```
@variavel: `document.images.length`;  // INEFICIENTE E PODE NÃO FUNCIONAR!
```

É fácil compreender isto quando se executa o pré-processador Less no servidor ou em linha de comando, já que não existe página e Less precisa gerar um CSS estático. Mas no browser também poderá não funcionar. Não há garantia que os objetos do DOM estejam disponíveis quando a página Less gerar o CSS. 

Uma maneira de contornar essa limitação é reprocessar toda a folha de estilos *após* a carga, o que pode ser feito com  `less.watch()` e configurando as opções do browser para que o próximo reprocessamento demore muito:

```
 <script>
  less.poll = 86400000; 
  less.watch(); // reprocessa uma vez a cada 86400 segundos.
 </script>
```

Se o objetivo for usar informações dinâmicas do browser para reconfigurar a folha de estilos, pode-se fazer o contrário: diminuir ao máximo o tempo entre os reprocessamentos. Assim, se o usuário redimensionar uma janela, por exemplo, o Less poderá usar esse dado para reposicionar os objetos. Por exemplo, o código abaixo (que só funciona no browser, pois depende do DOM) usa a altura e largura da janela obtida via JavaScript do DOM para posicionar um DIV no centro da janela:

```
// Variaveis DOM  - funciona apenas no browser!
@win-height:`$(window).height()`;
@win-width:`$(window).width()`;

@div-width: (@win-width / 2px);
@div-height: (@win-height / 2px);
@div-left-position: (@win-width - @div-width) / 2px;
@div-top-position: (@win-height - @div-height) / 2px;

.box {
  background: red;
  height: @div-height;
  width: @div-width;
  position: absolute;
  top: @div-top-position;
  left: @div-left-position;
}
```

Usando:

```
 <script>less.poll = 10; less.watch()</script>
```
no browser, o Less será reprocessado a cada dez milissegundos, fazendo com que as posições mudem dinamicamente.

##interagindo com o parser
Pode-se ter acesso ao parser do Less e configurá-lo, ler componentes de uma folha de estilos e alterar variáveis usando os recursos de scripting do Less, acessíveis tanto no servidor via Node.js, quanto no browser. Poucos recursos são documentados, portanto, devem ser usados com reserva. 

###substituição de variáveis no browser
Variáveis globais podem ser modificadas via JavaScript. A alteração não altera nem recarrega a folha de estilo mas força a recompilação. As variáveis alteradas irão valer para todo o escopo da folha de estilos na página. 

Para modificar use um script carregado *após* o `less.js` definindo um objeto `less.modifyVars` contendo a lista das variáveis a serem redefinidas:

```
 <script>
 less.modifyVars({
  '@cor-default': 'rgba(128,128,128,0.9)',
  '@tam-fonte-default': '12pt',
  '@margens': '2px'
 });
 </script>
```

Variáveis globais novas (que não existem na folha de estilos) podem ser inseridas de forma similar usando `less.globalVars`.

### funções customizadas
Pode-se também criar funções em JavaScript que serão usadas dentro de folhas de estilo Less carregadas no browser. Essas funções devem ser definidas em `less.functions` e dependem do uso de tipos não-documentados para funcionar. Podem ser declaradas como parte do objeto `less` *antes* de carregar o `less.js`:

```
 <script>
       less = {
            functions: {
                media: function(a, b) {
                    return new less.tree.Dimension((a.value + b.value) / 2);
                }
            }
        }; 
 </script>
```
Ou *depois* de carregar o `less.js` desta forma:

```
 <script>
        less.tree.functions.media = function(a, b) {
            return new less.tree.Dimension((a.value + b.value) / 2);
        };
 </script>
```

Uma vez definida, pode ser chamada dentro da folha de estilos Less:

```
.centralizar(@left, @right) {
  @a: unit(@left);
  @b: unit(@right);
  margin-left: unit(media(@a, @b), px);
}
.section {
  .centralizar(25px; 35px;);
}
```
CSS gerado:

```
.section {
    margin-left: 30px;
}
```

### leitura de variáveis
O parser do Less pode ser usado para carregar uma folha de estilos e ter acesso ao seu conteúdo. A folha de estilos pode ser uma string gerada na hora por JavaScript, ou mesmo um documento externo que não seja carregado na folha como Less.

O script JQuery abaixo carrega uma folha de estilos Less localizada no servidor e imprime uma lista de suas variáveis e valores:

```
 <script>
    $( document ).ready(function() {
        var parser = new(less.Parser);
        $.get( "../stylesheets/more/more-6-scripts.less", function( data ) {
            parser.parse(data, function (e, tree) {
                var variables = tree.variables();
                console.log(variables)
                for (i in variables) {
                    var name = variables[i].name;
                    var value = variables[i].value.toCSS(); // Falha se variavel contiver expressoes!
                    $("#varlist").append("<li>"+name + ":" + value+"</li>");
                }
            });
        });
    });
 </script>
  ```
  
Vários desses recursos do Less ainda são pouco documentados e podem mudar, portanto evite usá-los em produção. O processamento Less no browser também é pouco performático, sendo ideal gerar as folhas de estilo CSS offline. Processamento Less online deve apenas ser usado em desenvolvimento ou em sites com movimento muito baixo.

### uso em aplicações node.js
Em aplicações JavaScript lado-servidor como aplicações node.js, o Less pode ser instalado localmente em um projeto usando o Node Package Manager `npm`:

```
npm install less
```

E depois usar `require` nos scripts que usarem o Less:

```
var less = require('less');
```

A partir daí pode-se ter acesso aos recursos do Less através do objeto `less` de forma semelhante ao uso no browser:

```
var fs = require('fs');
var less = require('less');
var parser = new(less.Parser);
            
fs.readFile('stylesheets/styles.less', 'utf8', function(err, data) {
    if(!err) {
        parser.parse(data, function (e, tree) {
           var variables = tree.variables();
           tree.toCSS();
           ...
        });
    }
 });
 ```

## opções de linha de comando
Duas opções tornam o compilador Less mais rigoroso durante a compilação. As opções são passadas em linha de comando separadas por espaços da forma:

```
lessc opções arquivo.less
```

A opção `-su=on` (strict units) provoca erros de compilação em cálculos com unidades incompatíveis, multiplicação e divisão com unidades. Esses erros são ignorados por default.

A opção `-sm=on` (strict math) só executa expressões matemáticas que estiverem dentro de parênteses. Isto pode tornar-se default na versão 2.0 de Less. Com `-sm=on` o bloco Less abaixo:

```
.sec {
   font-size: 12pt + 10pt;
   margin: (12cm + 10cm);
}
```
será gerado em CSS como:

```
.sec {
   font-size: 12pt + 10pt;
   margin: 22cm;
}
```

##exercícios
1. x
2. x
3. x
4. x
5. x
