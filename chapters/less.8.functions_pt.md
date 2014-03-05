#{8: funções }
Este é um capítulo novo! Tipo referência!

##funções?

##conversão
### color
Converte uma representação de cor expressa como string em uma cor. Parece inútil, já que CSS interpreta uma representação de cor em string como uma cor sem que seja necessário passar para uma função como `color`. Mas às vezes é necessário passar uma cor como argumento de um mixin ou função que pode incorretamente interpretá-la como texto ou número. Nesse caso, passar o valor como argumento da função color garante que ele será tratado como uma cor.

Parâmetros: um valor do tipo string representando uma cor
Retorna: um valor do tipo color

### convert
Permite converter um número qualificado com uma unidade em outra unidade, por exemplo, cm em in. É uma função de conversão. As unidades devem ser compatíveis: comprimentos (m, cm, mm, in, pt, pc), tempo (s, ms) e ângulo (rad, deg, grad, turn). convert não faz conversões de pixels (px), em ou rem.

convert recebe dois parâmetros. O primeiro parâmetro é o número qualificado com uma unidade (ex: 12cm) e o segundo a unidade no qual se deseja ter o valor convertido.
```
```
```
```
### unit
Diferentemente de convert, unit() não converte, apenas troca a unidade. 5cm se transforma em 5in, ou 5px. Um número sem unidade pode receber uma ou um número com unidade pode tê-la removida.
```
```
```
```

### data-uri
Converte uma URL em um recurso embutido.
```
```
```
```

##string
### escape
realiza conversão de URL-encoding para caracteres especiais que precisem ser incluídos em uma URL, por exemplo, converter espaço em %20.
### e
converte strings em formato reconhecido pelo less (similar a color, só que mais abrangente)
###% (mover para outro lugar!)
A função % permite  a criação de strings formatados no estilo da função printf do C. Ela recebe uma string contendo placeholders indicando o tipo, seguido pelas variáveis, valores ou strings que devem ser inseridos nos lugares onde estão os placeholders.
(explorar as possibilidades com isto - o que pode e o que nao pode)






(exemplos de uso úteis em cada caso)
##cores

###rgb (CSS)
###rgba (CSS)
###argb
###hsl (CSS)
###hsla (CSS)
###hsv
###hsva
###hue, saturation e lightness
###hsvhue, hsvsaturation, hsvvalue
###red, green, blue
###alpha
###luma
### saturate, desaturate, greyscale
### lighten, darken
###fadein, fadeout, fade
###spin, mix
###contrast
##filtros
###multiply, screen, overlay
###softlight, hardlight
###difference, exclusion, average, negation

##exercícios
1. x
2. s
3. d
4. e
5. d
6. 