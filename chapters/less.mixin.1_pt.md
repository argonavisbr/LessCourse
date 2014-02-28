#{6: mixin }
##mixins?
Variáveis permitem armazenar valores. Mixins permitem armazenar conjuntos inteiros de regras, que podem ser reusadas em outros blocos. A declaração do conjunto de regras de um mixin não difere em nada de uma declaração normal usando seletores de classe ou id, mas esses seletores não precisam existir no HTML. Além disso, mixins podem ter parâmetros.

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
Expresso desta forma, o CSS gerado é praticamente o mesmo (talvez mude a forma de representação de cores, dependendo do processador). Mas o que faz esse bloco um mixin é a forma como ele é usado. Agora podemos aplicá-lo em vários outros blocos, e evitar repetir um monte de declarações:

```
.section {
   .text-content-set;
   h1 {
     color: #00008b;
   }
}

#footer {
  .text-content-set;
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

Mixins podem também usar ids como seletores (não faz diferença). 

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

##exercícios
1. Veja a folha de estilos exercicio1.css. Escreva uma folha de estilos LESS que obtenha os mesmos resultados com o máximo de reuso, usando recursos que vimos até agora (variáveis, extend e mixins).
2. xx
3. xx
4. ..
5. dd

