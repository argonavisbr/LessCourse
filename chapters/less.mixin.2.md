#{7: +mixin }
##mixins com parâmetros
Mixins podem receber parâmetros na forma de variáveis com escopo limitado ao bloco. As variáveis têm seus valores definidos quando o mixin é usado. Este tipo de mixin é ótimo para lidar com propriedades dependentes de fabricante, como muitas propriedades do CSS3.

```
```

Pode-se definir o mixin com um valor default para cada variável, que será usado caso os parâmetros não sejam definidos no uso.

```
```

Um mixin pode ter múltiplos parâmetros separados por ponto-e-vírgula. Os parâmetros são selecionados pela sua posição.
```
```

```
```

Mixins também podem ter parâmetros com nomes. Isto permite que sejam chamados com menos parâmetros, já que os parâmetros são identificados.
```
```

```
```

##sintaxes alternativas (e não recomendadas)
O Less também permite que se use vírgula para separar os parâmetros, o que pode gerar problemas já que algumas propriedades do CSS têm valores separados por vírgulas. Se o parâmetro passado a um mixin consistir de uma lista de valores separadas por vírgula, é importante incluir um ponto e vírgula no final para evitar que a lista seja interpretada como vários parâmetros.

Uma boa prática é utilizar sempre ponto-e-vírgula separando os argumentos, e sempre incluir um ponto-e-vírgula também no final.

Também é possível definir mixins com o mesmo nome que se distinguem apenas pelo número de parâmetros. Também é uma boa prática evitar mixins desse tipo.

É também uma boa prática usar mixins com parâmetros identificados, já que são mais legíveis. 

##@arguments
Há duas variáveis especiais que podem ser usadas em mixins. @arguments contém todos os argumentos recebidos. É util para aplicar os mesmos valores para diferentes propriedades:
```
```
que gera o seguinte CSS
```
```
Mixins que recebem um número variável de argumentos podem ser declarados com reticências (três pontos) entre parênteses.
```
```
Pode-se declarar alguns parâmetros e deixar os finais com número variável
```
```
Se houver uma variável antes das reticências ela receberá todos os argumentos que forem passados.
```
```

##seleção de mixins através de parâmetros
Pode-se declarar mixins contendo um ou mais parâmetros fixos que serão usados para selecioná-los. Por exemplo:
```
```

```
```

```
```

```
```

##mixins para CSS multi-browser
Mixins são ótimos para encapsular propriedades dependentes de browser. Essas propriedades têm um prefixo proprietário e geralmente recebem os mesmos dados. É possível encapsular em um mixin as propriedades relativas a diversos fabricantes diferentes, e chamar a popriedade pelo nome do mixin. Seguem alguns exemplos usando propriedades comuns do CSS3:
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

##mixins que retornam valores
Variáveis podem ser definidas dentro de mixins. Quando o mixin é usado, os valores dessas variáveis serão usados como valor de 'retorno' e podem ser lidas no bloco destino.
```
```
```
```
As variáveis não precisam ter um valor fixo. Os mixins podem transformar os valores recebidos como parâmetros e retornar outros, funcionando como se fossem funções:
```
```

```
```

## exercícios
1. Escreva um mixin para aplicar uma aparência de alto relevo em botões. O...
2. Escreva um mixin que receba duas cores, e uma lista variável de percentagens. O mixin deve então gerar um gradiente de uma cor para a outra, usando as percentagens como indicativos de stops para cada cor.
