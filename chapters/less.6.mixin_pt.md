#{6: mixin }
##mixins?
Variáveis permitem armazenar valores. Mixins permitem armazenar conjuntos inteiros de regras, que podem ser reusadas em outros blocos. 

A declaração de um mixin não difere em nada de uma declaração de um seletor CSS de classe ou id. A diferença é que o mixin foi criado para ser usado apenas na folha de estilos. Além disso, mixins podem ter parâmetros.

O mixin abaixo agrupa várias regras usadas em diferentes seções de um documento:

```
.text-content-set {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid rgb(225,225,225);
  background-color: rgb(128,128,128);
  color: rgb(240,240,240);
}
```

Expresso desta forma, o CSS gerado é praticamente o mesmo (talvez mude a forma de representação de cores, dependendo do processador). O que faz esse bloco um mixin é a *forma* como ele é usado. Agora podemos aplicá-lo em vários outros blocos, e evitar repetir um monte de declarações:

```
.section {
   .text-content-set;
   h1 {
     color: #00008b;
   }
}

#footer {
  .text-content-set;  // este é o mixin
  background-color: white;
}
```

O resultado será:

```
.text-content-set {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section h1 {
  color: #00008b;
}
#footer {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
  background-color: white;
}

```

Como não pretendemos que o mixin seja usado na página, podemos ocultá-lo na geração final do CSS declarando-o com parênteses:

```
.text-content-set() { ... }
```

Assim apenas os elementos que usam o mixin fazem parte do CSS final:

```
.section {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
}
.section h1 {
  color: #00008b;
}
#footer {
  padding: 2px;
  margin: 0 2px 0 2px;
  border: solid #e1e1e1;
  background-color: #808080;
  color: #f0f0f0;
  background-color: white;
}
```
É uma boa prática sempre usar parênteses na declaração de mixins, mesmo que eles não tenham parâmetros. 

Mixins podem também usar seletores de id (`#nome`) em vez de seletores de classe (`.nome`). Não faz nenhuma diferença, mas a convenção é usar seletores de classes para mixins, e seletores de id para namespaces (que veremos mais adiante).

##mixins com blocos aninhados
Mixins podem conter blocos aninhados, por exemplo:
```
```
```
```

Usando o símbolo &, que representa o seletor pai, um mixin pode ser usado para afetar o contexto de seletores aninhados:
```
```
```
```


##namespacing
Um namespace em less é declarado com um mixin usando seletor de ID. Namespaces são úteis para agrupar outros mixins e evitar conflitos de nome, principalmente em aplicações que importam folhas de estilo de terceiros.
```
```
Para usar deve-se usar o nome do mixin externo (namespace) antes do mixin interno. 
```
```
Existe uma sintaxe alternativa usando `>` que é opcional:
```
```
Recomendações sobre namespaces, boas práticas.

##!important
A palavra-chave !important é usada em CSS para sobrepor as regras de precedência do cascade e forçar a aplicação de uma declaração de estilo em um seletor. Se for aplicada em um mixin, todas as declarações de estilo contidas no mixin irão ser marcadas como !important.

```
```

```
```

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

## quando usar extend em vez de mixin
combining styles
reducing size

##exercícios
1. Escreva um mixin para aplicar uma aparência de alto relevo em botões. O...
2. Escreva um mixin que receba duas cores, e uma lista variável de percentagens. O mixin deve então gerar um gradiente de uma cor para a outra, usando as percentagens como indicativos de stops para cada cor.
3. Veja a folha de estilos exercicio1.css. Escreva uma folha de estilos LESS que obtenha os mesmos resultados com o máximo de reuso, usando recursos que vimos até agora (variáveis, extend e mixins).





