#{7: operações }

Neste capítulo serão explorados recursos do Less que permitem realizar operações matemática, operações de transformação de dados, operações condicionais e loops.

##operações matemáticas

tabela

##funções matemáticas

tabela

### arredondamento: ceil, floor, round
###percentage
Converte um número em percentagem.
### funções trigonométricas: sin, asin, cos, acos, tan, atan, pi
Exemplos usando convert e funções trigonométricas
### min e max
Retornam o maior e menor valor de uma lista de valores
### pow e sqrt
### mod e abs


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

## detecção de tipos
Existem cinco funções para verificar se o tipo da variável corresponde ao esperado:

###isnumber
###isstring
###iskeyword
###isurl


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

## detecção de unidades

Para números existem ainda quatro funções que retornam true para determinadas unidades:

###ispixel
###isem
###ispercentage
###isunit

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

##funções de listas
###length
Retorna o número de elementos em uma lista separada por vírgulas ou por espaços. Combinado com loops e a função extract pode realizar um for
### extract
Retorna o valor em uma posição específica da lista

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



##exercícios
1. x
2. x
3. x
4. x
5. x
6. x