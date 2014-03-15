#{2: fundamentos }

Nesta seção ilustraremos algumas características da sintaxe do Less e comparações com CSS. Alguns tópicos que serão abordados aqui em um primeiro momento serão detalhados em módulos futuros.

## Less é CSS, CSS não é Less

Os recursos do Less são todos opcionais. Less é uma extensão do CSS. Sempre pode-se usar CSS puro em uma página Less. Tudo que é CSS também é Less.

Por outro lado, CSS não é Less. Less é usado para gerar um CSS novo, que poderá ficar maior e mais repetitivo que o documento original Less.

Pode-se criar uma folha de estilos Less partindo-se de uma folha de estilos CSS existente. A forma mais simples é mudar a extensão do arquivo para `.less`. Quando o arquivo for lido pelo processador Less será gerado um CSS funcionalmente idêntico. Ele talvez substitua algumas representações por outras equivalentes (por exemplo, `rgb(255,0,0)` será substituído por `#ff0000`), mas o arquivo gerado será equivalente.

A partir de um documento Less contendo CSS puro, pode-se fazer alterações gradualmente, aplicando recursos do Less como aninhamento de declarações, variáveis, funções, etc. Este processo é chamado de refatoração: ele altera a forma de se expressar regras de estilo sem alterar o resultado. A melhor maneira de fazer refatoração é usando um editor que gere automaticamente o CSS, como os que foram apresentados no capítulo anterior.

##aninhamento de declarações
Less permite que declarações CSS sejam aninhadas. É uma forma alternativa de expressar relacionamentos contextuais entre seletores. Por exemplo, as duas declarações a seguir em CSS:

```
.artigo {
  border: solid red 1px;
  font-size: 12pt;
}
.artigo .secao1 {
  background-color: black;
  color: white;
}
.artigo .secao2 {
  background-color: white;
  color: black;
}
```
Podem ser combinadas em uma única declaração aninhada em Less:

```
.artigo {
  border: solid red 1px;
  font-size: 12pt;
  .secao1 {
    background-color: black;
    color: white;
  }
  .secao2 {
    background-color: white;
    color: black;
  }
}
```
Não há limites para o nível de aninhamento:

```
.livro {
  font-size: 10pt;
  .capitulo {
    border: solid gray 1px;
    .secao {
      background-color: #eee;
      .titulo {
        font-size: 120%;
      }
      .corpo {
        text-align: justify;
      }
    }
  }
}
```
A declaração aninhada acima resulta em cinco declarações:

```
.livro {
  font-size: 10pt;
}
.livro .capitulo {
  border: solid gray 1px;
}
.livro .capitulo .secao {
  background-color: #eee;
}
.livro .capitulo .secao .titulo {
  font-size: 120%;
}
.livro .capitulo .secao .corpo {
  text-align: justify;
}
```

O(s) seletor(es) declarados em um bloco externo são copiados antes de cada seletor de um bloco interno. Por exemplo, considere o bloco abaixo:

```
.t1 p {
  .t2, .t3 li {
    color: blue;
  }
}
```
Os seletores `.t2` e `.t3 li` serão ambos reescritos em CSS como seletores contextuais descendentes de `.t1 p`:

```
.t1 p .t2,
.t1 p .t3 li {
  color: blue;
}
```

Qualquer tipo de contexto hierárquico pode ser representado, como filho (child selector) `>`, irmão adjacente (adjacent sibling selector) (`+`), etc. como na regra abaixo:

```
.v1 {
   + .v2 {
   > .v3 {
      font: 12pt;
    }
  }
}
```

que gera este CSS:

```
.v1 + .v2 > .v3 {
  font: 12pt;
}
```

O uso de declarações aninhadas é opcional. Geralmente elas aumentam a legibilidade e facilitam a manutenção mantendo juntas as regras aplicadas em  seletores que têm relacionamentos hierárquicos que refletem a estrutura do HTML. Mas são opcionais e não devem ser usadas se uma declaração comum expressar a aplicação de um estilo de forma mais clara e concisa. Por exemplo, se um relacionamento contextual longo aparece uma única vez em CSS. Por exemplo:

```
div .notes table .footnotes p {color: red}
```

é bem mais conciso que:

```
div {
  .notes {
    table {
      .footnotes {
        p {
          color: red
        }
      }
    }
  }
}
```

Mas se houver necessidade de aplicar regras a seletores intermediários da hierarquia, o uso de seletores aninhados pode ser mais vantajoso. O importante é avaliar os benefícios de usar ou não declarações aninhadas em Less.

##comentários
Less é CSS, logo suporta comentários de bloco CSS, no formato `/* ... */`:

```
/* Este bloco de texto e as duas últimas propriedades
    do ruleset abaixo serão ignoradas pelo browser */
.header {
      color: cyan;
  /*  background: navy;
      font-size: 16pt;   */
} 
    
```

Mas também comentários de linha, usando duas barras `//`. Este tipo de comentário permite omitir trechos de código da geração do CSS final, e não contribui para aumentar o arquivo CSS. O comentário inicia em `//`, afeta apenas a linha em que foi definido, a partir desse ponto, e só termina no fim da linha. Não é possível terminá-lo antes.

```
// No CSS, o bloco a seguir terá apenas uma declaração de estilo
// Também não terá esta linha, nem a anterior
/* Mas esta linha estará presente no CSS, embora seja ignorada pelo browser */
.header {
  color: cyan;
  // background: navy;
}
```
Apenas os comentários de bloco são preservados no CSS:
```
/* Mas esta linha estará presente no CSS, embora seja ignorada pelo browser */
.header {
  color: cyan;
}
```

##o símbolo '&' (seletor pai)
Vimos que o aninhamento cria seletores contextuais posicionando os seletores de um bloco interno como descendentes dos seletores do bloco externo. O símbolo `&` que representa o acumulado de seletores do bloco externo. 

Considere a regra abaixo:
```
.t1 {
  & .t2 {
    & .t3 {
      font: 12pt;
    }
  }
}
```
Ela irá gerar o seguinte CSS:

```
.t1 .t2 .t3 {
  font: 12pt;
}
```

Que é o mesmo gerado se o `&` não estivesse presente.

```
.t1 {
  .t2 {
    .t3 {
      font: 12pt;
    }
  }
}
```

Mas podemos usar o `&` para posicionar os seletores acumulados nos blocos externos em outro lugar. Na regra abaixo invertemos a ordem de contexto:

```
.s1 {
  .s2 & {
     .s3 & {
      font: 12pt;
    }
  }
}
```

E obtemos o seguinte resultado:

```
.s3 .s2 .s1 {
  font: 12pt;
}
```

Se o símbolo `&` é usado uma vez, ele substitui a posição dos seletores herdados, mas se ele aparecer mais de uma vez, ele repete esses seletores, por exemplo:

```
.r1 {
  & & .r2 {
    font: 12pt;
  }
}
```

Que tem como resultado:

```
.r1 .r1 .r2 .r1 {
  font: 12pt;
}
```

## aninhamento de declarações com pseudo-seletores

O aninhamento não se limita a seletores contextuais. É possível usar o símbolo `&` em classes, pseudo-classes e pseudo-elementos.

O exemplo abaixo aplica aplica dois pseudo-elementos em `.tag`

```
.tag {
  &:before {Content: '&lt;'}
  &:after {Content: '/&gt;'}
}
```
Resultado em CSS:

```
.tag:before {
  Content: '&lt;';
}
.tag:after {
  Content: '/&gt;';
}
```
A regra abaixo aplica uma pseudo-classe no seletor `div p` e uma classe no seletor `div`:

```
div {
  p {
    &:first-child {color: red;}
  }
  &.last {color: blue;}
}
```

Produzindo este resultado em CSS:

```
div p:first-child {
  color: red;
}
div.last {
  color: blue;
}
```

#criando seletores com nomes semelhantes

```
.secao {
  &-1 {}
  &-2 {}
```

#mais concatenação com &

```
&+&
& &
&&
&, &1, &2, &3
```

```
a, b, c, d, e {
  & > & > & {}
}
```

##exercícios
Os arquivos usados nos exercícios estão disponíveis para download e podem ser encontrados nos subdiretórios da pasta `exercicios` correspondente ao capitulo.

1. Aplicando os recursos do Less que vimos até o momento, altere o CSS abaixo aplicando aninhamento e o símbolo '&' onde for possível.

```
${file: stylesheets/basics/basics_e1.less}
```

2. Expresse a seguinte regra de estilo na forma de uma *única* declaração Less aninhada: "Todos os elementos `p` dentro de `td` em uma seção `.rodape` devem ter fonte de tamanho 8px e cor #ccc; elementos `td` filhos de rodapé devem ter margens e paddings laterais iguais a zero, e os últimos `td` devem ter uma borda de 1px preta e sólida na parte de baixo, enquanto que o primeiros `td` devem ter uma linha igual na parte de cima". Dica: use pseudo-seletores :first-item e :last-item.
3. Considere o documento Less abaixo:

```
${file: stylesheets/basics/basics_e3.less}
```
Quantas declarações CSS serão geradas? Marque uma opção:

a. 1 
b. 2
c. 3
d. 4
e. 5

4. Considere o documento Less abaixo:

```
${file: stylesheets/basics/basics_e4.less}
```
Que palavras abaixo estarão presentes no CSS gerado? Marque todas as opções verdadeiras:
a. cor de fundo
b. comentário
c. linha
d. fonte
e. última

5. Considere as declarações CSS abaixo:

```
${file: stylesheets/basics/basics_e5.css}
```
Quais declarações ou grupo de declarações Less abaixo pode ter gerado o CSS acima?

a. `${file: stylesheets/basics/basics_e5_a.less}`
b. `${file: stylesheets/basics/basics_e5_b.less}`
c. `${file: stylesheets/basics/basics_e5_c.less}`
d. `${file: stylesheets/basics/basics_e5_d.less}`
e. `${file: stylesheets/basics/basics_e5_e.less}`

6. Considere o documento Less abaixo:

```
${file: stylesheets/basics/basics_e6.less}
```
Que declarações CSS abaixo não poderiam ter sido geradas a partir do processamento do documento acima?

a. `${file: stylesheets/basics/basics_e6_a.css}`
b. `${file: stylesheets/basics/basics_e6_b.css}`
c. `${file: stylesheets/basics/basics_e6_c.css}`
d. `${file: stylesheets/basics/basics_e6_d.css}`
e. `${file: stylesheets/basics/basics_e6_e.css}`


