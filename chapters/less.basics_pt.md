#{3: fundamentos }

Nesta seção ilustraremos algumas características da sintaxe do Less e comparações com CSS. Alguns tópicos serão detalhados em módulos futuros.

Less é uma extensão do CSS. Sempre pode-se usar CSS puro em uma página Less.

##aninhamento
[Incluir aqui capítulos 2 e 3 da apostila antiga]

Less inclui a opção de aninhamento de regras de estilo. É um recurso opcional mas ajuda a diminuir bastante a repetição de código em CSS. Por exemplo, em vez de expressar seletores contextuais desta forma:
```
```
Pode-se escrever desta outra forma. 
```
```
```
```
O resultado reflete melhor a estrutura do HTML e também fica mais compacto.

O aninhamento não se limita a seletores contextuais. É possível usá-lo para aplicar pseudo-classes e pseudo-elementos usando o símbolo `&` que representa o seletor pai em Less.
```
```
```
```
A ordem dos seletores também pode ser alterada com o símbolo `&`. 
```
```
[MUDAR PARA VARIAVEIS]
As chaves também limitam o escopo de variáveis. Se uma variável é definida dentro de um bloco, o valor dela vale para todo o bloco e para os blocos que estejam em nível mais baixo.
```
```
```
```

A ordem das declarações é irrelevante. Você pode declarar uma variável no final de um bloco, que o valor dela valerá para o bloco inteiro e todos os sub-blocos, mesmo que tenham sido declarados antes. As variáveis podem ser declaradas em qualquer lugar do arquivo, mesmo no final. Somente blocos limitam o escopo.
```
```
```
```

[MUDAR PARA NS]
Um namespace em less é declarado com um seletor de ID.
```
```
```
```

##comentários
Less suporta comentários de bloco CSS:
```
```
e também comentários de linha, usando duas barras:
```
```
Apenas os comentários de bloco são preservados no CSS:
```
```


##comparações com css
[remover esta seção ou atualizar cap 10 da apostila antiga e colocar aqui]
##exercícios
1. x
2. y
3. z
4. a
5. b
6. c

