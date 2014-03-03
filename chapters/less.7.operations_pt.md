#{7: operações }

Retirar todos os exemplos do Less 1.3 que não funcionam mais, testar tudo, e só depois colocar de volta via GitHub.

##operações e diretivas
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

##mixins condicionais (mixin guards)
Pode-se criar mixins parametrizados que definem regras de estilo diferentes de acordo com operações condicionais realizadas sobre os valores recebidos. São chamados de _mixin guards_. Através deles é possível tomar decisões de forma similar a expressões if/else existentes em linguagens de programação.

A expressão condicional é representada por um teste seguindo a palavra-chave `when` que significa 'quando'. Assim pode-se representar expressões do tipo 'defina o mixin x quando a expressão y for verdadeira' usando a seguinte sintaxe
```
mixin when (condição) {
    regras;
}

Por exemplo:

```
```
```
```
```
```
Se usarmos os mixins acima da forma abaixo:
```
```
```
```
Obteremos o seguinte CSS:
```
```
### operadores
Para testar condições pode-se se usar os seguintes operadores: `>`, `>=`, `<`, `<=`, `=`.
```
```
Além dos operadores ainda existem a palavra reservada `true` que pode ser usada na comparação, e `not`, que nega a expressão seguinte:
```
```
```
```
Várias expressões condicionais podem ser combinadas com a vírgula e têm efeito somatório ('ou' lógico), ou seja, se qualquer uma das expressões resultar em verdadeira, a expressão inteira é aceita.
```
```
```
```

O efeito multiplicador ('e' lógico) na combinação de expressões é obtido com a palavra-chave `and`:
```
```
```
```


Existem cinco funções para verificar se o tipo da variável corresponde ao esperado:

- iscolor (true se valor for #rrggbb, rgb(%,%,%), rgb(r,g,b), nome-de-cor, etc.)
- iskeyword (true se valor for uma palavra-chave do Less)
- isnumber (true se valor for um número)
- isstring (true se valor for um texto)
- isurl (true se valor for uma url)

```
```
```
```


A função default é usada como cláusula "else" de um mixin guard. Representa uma condição default que será executada se nenhuma outra condição resultar em true.
```
```
```
```



Para números existem ainda quatro funções que retornam true para determinadas unidades:

- isem (unidade é `em`)
- ispixel (unidade é `px`)
- ispercentage (unidade é `%`)
- isunit (tem unidade - pode-se usar `not` para testar que não tem)

Os exemplos abaixo ilustram algumas expressões complexas e seu resultado em CSS:
```
```
```
```
```
```
```
```
```
```

Expressões condicionais não são exclusividade dos mixins. Elas podem ser aplicadas diretamente nos seletores CSS:

```
```

Pode-se aplicá-las em um conjunto de seletores colocando-os dentro de um bloco que usa `&` como seletor. A expressão condicional será aplicada a cada um dos elementos no bloco abaixo:

```

```

##loops
Loops em LESS são chamadas recursivas de mixins, ou seja, o mixin chama ele mesmo. Para que o loop não seja infinito, é necessário que a cada iteração uma condição seja testada e que o valor testado mude para que em algum momento ela seja falsa, e o loop tenha fim.

O loop abaixo repete um número de vezes definida quando o mixin for chamado. A cada repetição, o contador é comparado com zero, e se for maior que zero, seu valor é diminuído de um e o corpo do mixin é executado. Quando o valor for zero, o mixin termina.
```
```
No bloco abaixo o mixin é chamado para executar quatro vezes:
```
```
E o resultado do CSS é:
```
```
Neste exemplo, um loop é usado para gerar colunas em uma página:
```
```
```
```
Este exemplo ilustra o uso de loops para gerar uma seqüência para animação:
```
```
```
```
```
```

##agregação de valores
Less permite que valores de propriedades definidas em um mixin sejam agregadas aos valores existentes de propriedades dos seletores onde o mixin é usado, em uma lista separada por vírgulas. Isto é útil para propriedades que aceitam valores separados por vírgulas como 'font-family' e outros.

Para usar, é necessário incluir um sinal `+` tanto da propriedade declarada no mixin, quanto na propriedade declarada no seletor afetado:

```
```
```
```
```
```


##exercícios
1. x
2. x
3. x
4. x
5. x
6. x