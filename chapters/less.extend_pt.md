#{5: extend }
##extend?

Extend é uma pseudo-classe exclusiva do Less que mescla as regras do seletor no qual é aplicado com outros que combinam com nomes passados como parâmetro. 

##sintaxe

A sintaxe geral é

```
seletor:extend(seletores)
```

Ele pode ser usado em seletores comuns, ou dentro de um bloco usando o seletor universal ```&```:

```
s1, s2 {
   &:extend(s3, s4)
}
```

O código acima é o mesmo que

```
s1:extend(s3, s4), s2:extend(s3, s4) {}
```

ou 

```
s1:extend(s3) {}
s1:extend(s4) {}
s2:extend(s3) {}
s2:extend(s4) {}
```

Se s3 e s4 forem:

```
s3 {color: blue}
s4 {font-size: 12pt}
```

O CSS resultante será

```
s3, s1, s2 {
  color: #0000ff;
}
s4, s1, s2 {
  font-size: 12pt;
}

```

##em seletor

Um seletor pode ter outras pseudo-classes além de extend, mas extend tem que ser a última, porém pode haver mais de um. Extend não é capaz de pesquisar variáveis, mas ele pode ser anexado a um seletor representado por uma variável interpolada

Isto é legal:

```
pre:nth-child(odd):extend(div span) {}
a:hover:extend(div):extend(section) {}
a:hover:extend(div, section) {}
@var: .secao;
@{var}:extend(div) {}
```

Isto não é legal

```
a:extend(div):hover {}
```

Isto não produz resultados

```
@var: .secao;
div:extend(@{var}) {}
```

##em ruleset

Se um conjunto de regras possui múltiplos seletores, um ou mais podem ter :extend. Se todos têm :extend, pode-se usar &:extend dentro das chaves para obter o mesmo resultado, por exemplo:

```
s1:extend(h1), s2:extend(h1) { color: blue}
```

é a mesma coisa que

```
s1, s2 {
   &:extend(h1);
   color: blue;
}
```

## extend em seletores aninhados

Em seletores aninhados, o extend trata os seletores de acordo com o resultado da transformação em CSS. Um bloco aninhado em Less da forma:

```
table {
  td { ... }
}
```

que corresponde ao CSS

```
table td { border: solid; }
```

pode ser alvo da extensão de um seletor em Less como

```
.classe:extend(table td) { color: green; }
```

que resulta no CSS

```
.classe {
  color: green;
}
table td,
.classe {
  border: solid;
}
```

O seletor Less universal(?) também pode ser usado

```
table {
  td & { ... }
}
```

que corresponde ao CSS

```
td table { ... }
```

pode ser alvo da extensão de um seletor em Less como

```
.classe:extend(td table) {}
```

## exact matching
(apostila antiga)


##exercícios
(apostila antiga)
