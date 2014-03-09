#{7: loops.guards }

Neste capítulo serão explorados recursos do Less que permitem realizar operações condicionais e loops.


##mixins condicionais (mixin guards)
Pode-se criar mixins parametrizados que definem regras de estilo diferentes de acordo com operações condicionais realizadas sobre os valores recebidos. São chamados de _mixin guards_. Através deles é possível tomar decisões de forma similar a expressões if/else existentes em linguagens de programação.

A expressão condicional é representada por um teste seguindo a palavra-chave `when` que significa 'quando'. Assim pode-se representar expressões do tipo 'defina o mixin x quando a expressão y for verdadeira' usando a seguinte sintaxe

```
mixin when (condição) {
    regras;
}
```

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

### operadores e funções condicionais
Para testar condições pode-se se usar os seguintes operadores: `>`, `>=`, `<`, `<=`, `=`.

```
```

Além dos operadores ainda existem a palavra reservada `true` que pode ser usada na comparação, e `not`, que nega a expressão seguinte:

```
```
```
```



### detecção de tipos e unidades
Existem cinco funções para verificar se o tipo da variável corresponde ao esperado:

Função | Recebe | Retorna
--|--|--
iscolor | string | true se valor for uma cor, ou seja, se o string contiver #rrggbb, rgb(%,%,%), rgb(r,g,b), nome-de-cor, etc.
iskeyword | string | true se valor for uma palavra-chave do Less, por exemplo ...
isnumber | string | true se valor for um número, 12, 12.3
isstring | string | true se valor for um texto
isurl | string | true se valor for uma url

Para números existem ainda quatro funções que retornam true para determinadas unidades:

Função | Recebe | Retorna
--|--|--
ispixel | número | unidade é `em`
isem número | unidade é `px`
ispercentage | número | unidade é `%`
isunit | número | tem unidade - pode-se usar `not` para testar que não tem

### combinação de expressões condicionais

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

```
```
```
```


A função default é usada como cláusula "else" de um mixin guard. Representa uma condição default que será executada se nenhuma outra condição resultar em true.
```
```
```
```




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

##mixins recursivos (loops)

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



##exercícios
1. x
2. x
3. x
4. x
5. x
6. x