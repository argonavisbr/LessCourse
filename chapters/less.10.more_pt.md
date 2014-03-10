#{9: +less }

##imports
Diretivas de import podem ser colocadas em qualquer lugar do arquivo less. A geração do CSS irá posicioná-las no início do arquivo corretamente.
```
```
```
```
Se a extensão do arquivo for .css, o less irá tratá-lo como um @import comum do CSS, e manter no arquivo. Se for qualquer outra extensão, o arquivo será incorporado e tratado como sendo less. Se o arquivo for importado sem extensão, o less irá anexar uma extensão .less a ele e buscar incorporar o arquivo.

Pode-se sobrepor o comportamento default do import com seis opções que são passadas entre parênteses entre a diretiva @import e o nome do arquivo, da forma:

```
@import (parametro) "arquivo";
```
Os seguintes parametros podem ser usados:

- css: trata o arquivo como CSS (independente de sua extensão)
- inline: inclui o conteúdo do arquivo no CSS final, mas não o processa
- less: trata o arquivo como LESS (independente de sua extensão)
- multiple: aceita que um um arquivo seja importado mais de uma vez
- once: permite importa o arquivo uma vez e ignora outras tentativas
- reference: inclui e usa o arquivo LESS mas não utiliza o conteúdo na saída

### reference
### multiple
### once
### inline
### less
### css

### conversao data-uri
Converte uma URL em um recurso embutido.

```
```

```
```



### conversão de cor para formato Microsoft

A função `argb` converte uma cor em uma representação de cor proprietária compatível com aplicações Microsoft e Internet Explorer. Nesta representação o fator alfa é um número hexadecimal entre 00 e FF e é representado antes dos pares hexadecimais dos componentes RGB. O formato resultante é #aarrggbb.

Função | Recebe | Retorna
--|--|--
`argb(a,r,g,b)` | `a` = número entre 0 e 1 ou percentagem, `r,g,b` = números (0-255) ou percentagem | cor em hexadecimal no formato `#aarrggbb`. Ex: `argb( rgba(255,0,0,0.5) )` gera `#80ff0000`.

### contraste
Função | Recebe | Retorna
--|--|--
`contrast(c,c1,c2,p)` | c |

## import e variáveis

Variáveis não precisam ser declaradas antes do uso. Se houver mais de uma definição, a última definição, considerando o escopo do atual para o mais externo, é usada.

```
@base-color: green;
@dark-color: darken(@base-color, 10%);

// use of library
@import "library.less";
@base-color: red;
```

`@base-color` é `red`, e `@dark-color` é vermelho escuro.

##bibliotecas de mixins com namespaces

Um namespace em less é declarado com um mixin usando seletor de ID. Namespaces são úteis para agrupar outros mixins e evitar conflitos de nome, principalmente em aplicações que importam folhas de estilo de terceiros.

```
```
Para usar deve-se usar o nome do mixin externo (namespace) antes do mixin interno.

```
```
Existe uma sintaxe alternativa usando `>` que é opcional:

```
```

Incluir recomendações sobre namespaces, boas práticas.

##less com javascript
less.Parser - como substituir variáveis, como acessar o pre-processador, como usar com Node, etc.

##exercícios

##projeto