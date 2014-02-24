#{4: variáveis }
##variáveis?

Variáveis permitem armazenar valores que serão reusados muitas vezes. É muito comum que certos valores como cores, fontes, dimensões, etc. sejam repetidas várias vezes em uma folha de estilo CSS. As variáveis do Less permitem eliminar essa repetição. É possível ainda realizar operações com variáveis, passá-las como argumentos para funções, etc.

A declaração de uma variável tem a seguinte sintaxe:

```
@variavel: valor;
```

Exemplos:

```
@tipologia: "Times New Roman", serif;
@largura: 15em;
@altura: @largura * 2;
```

Uma variável pode ser usada no lugar de valores de propriedades CSS:

```
h1 {
   font-family: @tipologia;
   height: @altura;
   width: @largura;
}
```

O CSS gerado será

```
h1 {
  font-family: "Times New Roman", serif;
  height: 30em;
  width: 15em;
}
```

## variáveis interpoladas

Variáveis não servem apenas para guardar valores. Podem ser usadas para conter outros textos, tais como nomes de propriedades, seletores, URLs, etc. Neste caso a declaração não muda, mas para usar a variável é preciso colocar seu nome entre chaves:

```
@transparencia: opacity;
@classe: coluna
@diretorio: "../images";
@prop: color;

.@{coluna} { 
    @{transparencia}: 0.5; 
    background: url(@{diretorio}/paisagem.png");
    border-@{prop}: red;
    background-@{prop}: black;
} 
```

Variáveis contendo nomes de variáveis

```
@the-end: "this is the end";
@end: "the-end";

content: @@end;
```

Variáveis não precisam ser declaradas antes do uso. Se houver mais de uma definição, a última definição, considerando o escopo do atual para o mais externo, é usada.

```
@base-color: green;
@dark-color: darken(@base-color, 10%);

// use of library
@import "library.less";
@base-color: red;
```

`@base-color` é `red`, e `@dark-color` é vermelho escuro.

##seletores
##urls
##propriedades
##import
##exercícios