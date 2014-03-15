#{3: variáveis }

Variáveis permitem armazenar valores que serão reusados muitas vezes. É muito comum que certos valores como cores, fontes, dimensões, etc. sejam repetidas várias vezes em uma folha de estilo CSS. As variáveis do Less permitem eliminar essa repetição. É possível ainda realizar operações com variáveis, passá-las como argumentos para funções, etc.

Variáveis devem ter a seguinte sintaxe:

```
@nome: valores;
```

O `nome` pode ser qualquer nome que não seja uma palavra-chave (por exemplo, não pode ser `import`). Os `valores` podem quaisquer valores ou listas de valores válidas em CSS ou mesmo expressões sobre valores ou variáveis. 

## variáveis globais

Uma variável pode ser global ou ter o escopo limitado a um bloco. Variávels globais devem ser declaradas fora de blocos, em uma linha própria. Veja alguns exemplos:

```
@tipologia: "Times New Roman", "Times", serif;
@largura: 15em;
@cor-base: rgb(255, 255, 255);
@altura: @largura * 2;
```

Uma vez declarada, uma variável global pode ser usada no lugar de valores de propriedades CSS em qualquer lugar do arquivo:

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
  font-family: "Times New Roman", "Times", serif;
  height: 30em;
  width: 15em;
  color: #ffffff;
}
```

As variáveis globais podem ser declaradas em qualquer parte do documento Less, mesmo depois dos blocos que as utilizam. A ordem em que são declaradas é irrelevante. Mas é uma boa prática adotar uma ordem seqüencial e mantê-las no início do documento para facilitar a legibilidade e manutenção do código.

## escopo e variáveis locais
As chaves que delimitam blocos limitam o escopo das variáveis. Se uma variável é definida dentro de um bloco ela é *local* ao bloco. O valor dela vale para todo o bloco e para os blocos que estejam aninhados em um nível mais baixo, mas o valor defnido para uma variável declarada dentro de um bloco não é visível em blocos externos ou no contexto global.

No exemplo abaixo há variáveis em três niveis de escopo. `@global` pode ser usada em qualquer lugar do documento. `@local-1` foi definida dentro do contexto do bloco `header` então só pode ser usada dentro desse bloco e por blocos aninhados (como o `div`), mas `@global` pode ser usada. Finalmente em `div` há uma variável `@local-2` definida, que não existe fora desse bloco.

```
@global: 'Global';

header {
  @local-1: 'Local ao header';
  Content: @global, @local-1;

  div {
    @local-2: 'Local ao div';
    Content:@global, @local-1, @local-2;
  }
  // aqui não existe @local-2
}
// aqui não existe local-1 ou local-2

footer {
  Content: @global;
}
```
O CSS resultante desse documento Less é:

```
header {
  Content: 'Global', 'Local ao header';
}
header div {
  Content: 'Global', 'Local ao header', 'Local ao div';
}
footer {
  Content: 'Global';
}
```

O processador Less exibe uma mensagem de erro e não gera o CSS se uma variável for usada fora do contexto onde ela é declarada, mas é possível *declarar* variáveis em um contexto global com um valor qualquer, e *redefinir* essa variável em um contexto local declarando-a novamente (mesmo nome) com outro valor. Nesse caso, o novo *valor*  da variável será válido apenas nesse contexto. Quando o bloco onde o valor foi redefinido terminar, a variável volta para o seu valor antigo:

```
@global: 'Global';

article {
   @global: "Local ao article";
   Content: @global;
   section {
     Content: @global;
   }
}

aside {
  Content: @global;
}
```

No exemplo acima, a variável  `@global` foi redefinida no bloco `article`, e o novo valor vale não apenas para todas as vezes que a variável for usada dentro do bloco, mas também em qualquer bloco aninhado, como section. O bloco `aside` está fora do bloco portanto vê o valor original de `@global`, pois a redefinição tem escopo limitado pelo bloco.

```
article {
  Content: "Local ao article";
}
article section {
  Content: "Local ao article";
}
aside {
  Content: 'Global';
}
```

Assim como ocorre no documento com variáveis globais, a *ordem* das declarações dentro de cada bloco é irrelevante. Você pode declarar uma variável no final de um bloco, que o valor dela valerá para o bloco inteiro e todos os sub-blocos, mesmo que tenham sido declarados antes. Somente o *aninhamento* dos blocos limita o escopo de variáveis. A ordem dentro do mesmo bloco não afeta o escopo.

O exemplo anterior poderia ter sido escrito da forma abaixo e geraria o mesmo CSS:

```
article {
   Content: @global;
   section {
     Content: @global;
   }
  @global: "Local ao article";
}

aside {
  Content: @global;
}

@global: 'Global';
```

É importante entender esse funcionamento para não ter surpresas. É igualmente importante evitar escrever documentos Less assim, pois eles ficam menos legíveis para quem precisa mantê-los.

## variáveis interpoladas

Variáveis não servem apenas para guardar valores de propriedades. Podem ser usadas para conter outros textos, tais como nomes de propriedades, seletores, URLs, etc. Neste caso a *declaração* continua igual, mas para *usar* a variável é preciso usar uma sintaxe diferente com o nome da variável (sem o `@`) entre chaves da forma `@{variavel}`.

O exemplo abaixo declara quatro variáveis que são usadas de diversas maneiras diferentes usando interpolação:

- Como seletor: variável `@classe`;
- Como nome de uma propriedade CSS: variável `@transparencia`;
- Como parte do nome de uma propriedade: variável `@prop`;
- Como parte do string usado em uma URL: variável `@diretorio`;

```
@transparencia: opacity;
@classe: coluna;
@diretorio: "../images";
@prop: color;

.@{classe} {
  @{transparencia}: 0.5;
  border-@{prop}: red;
  background: url('@{diretorio}/paisagem.png');
}
```

Observe que o uso de variáveis interpoladas em URLs deve ser dentro das aspas (ou apóstrofes). O resultado em CSS do Less acima é:

```
.coluna {
  opacity: 0.5;
  border-color: red;
  background: url('../images/paisagem.png');
}
```

Uma variável também pode conter o nome de outra variável simplesmente incluíndo mais um `@` antes do nome (que ficará `@@nome`):

```
@the-end: "this is the end";
@end: "the-end";

.my-friend {
  Content: @@end;
}
```
O resultado será:
```
.my-friend {
  Content: "this is the end";
}
```

Não se pode incluir mais que dois `@` seguidos.

##exercícios
1. X
2. X
3. X
4. X
5. X
6. X