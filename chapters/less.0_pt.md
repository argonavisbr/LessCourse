#{less}
Um tutorial por Helder da Rocha.

##apresentação
Pré-processadores CSS como Less e SASS/SCSS são hoje ferramentas essenciais para o desenvolvimento Web. Eles estendem o CSS com recursos que tornam o desenvolvimento mais eficiente, mais fácil de manter e reutilizar. Less é um dos mais populares pré-processadores CSS. É usado em bibliotecas de temas, aplicações e em frameworks populares de design responsivo como o Twitter Bootstrap.

Este curso explora a tecnologia Less em detalhes, oferecendo uma introdução abrangente e cobrindo tópicos mais avançados através de exemplos. Você deve conhecer HTML e CSS, uma vez que Less é uma extensão do CSS e também é importante entender o CSS que é gerado quando se usa Less. Não é preciso conhecer JavaScript, mas se você souber usar a linguagem poderá explorar alguns recursos não-essenciais de Less que requerem JavaScript. O uso de Less com JavaScript é abordado em um dos tópicos do último capítulo.

Para acompanhar este tutorial você deve ter um computador com um editor de textos e um compilador Less instalado. Algumas opções de editores e compiladores serão apresentadas no primeiro capítulo.

##conteúdo
1. introdução
    - o que é less
     - por que usar
     - compilação
    - ambiente de desenvolvimento
    - compilação em tempo real
    - versões e alternativas
2. fundamentos
     - less é css, css não é less
     - aninhamento
     - comentários
     - o seletor pai
     - aninhamento com pseudo-seletores
     - concatenação com &
3. variáveis
     - variáveis globais
     - escopo e variáveis locais
     - variáveis interpoladas
4. extensão
     - :extend
     - extensão múltipla
     - extensão com seletores aninhados
     - sobreposição de propriedades
     - pseudo-elementos e variáveis
     - correspondência exata
     - correspondência parcial
     - escopo
     - quando usar :extend
5. mixins
     - mixins
     - mixins com blocos aninhados
     - !important
     - mixins com parâmetros
     - sintaxes alternativas (e problemáticas)
     - mixins com número variável de argumentos
     - sobrecarga de mixins
     - mixins para CSS multi-browser
     - mixins que retornam valores
     - mixins com :extend
     - agregação de valores
6. operações e funções
     - operações aritméticas
     - operações usando unidades
     - arredondamento e percentagem
     - conversão de unidades
     - funções matemáticas e trigonométricas
     - funções que operam sobre coleções
     - transformação e formatação de texto
     - sintaxes problemáticas
7. loops e guards
     - mixins condicionais (mixin guards)
         - operadores e funções condicionais
         - detecção de tipos e unidades
         - combinação de expressões condicionais
     - mixins recursivos (loops)
8. cores
     - definição de cores
         - funções do CSS
         - funções HSV
     - extração de componentes de cores
         - componentes rgba
         - componentes hsl e hsv
         - luma
     - operações sobre cores individuais
         - saturação
         - luminância
         - matiz
         - transparência
     - operações de combinação de cores
         - simétricas
         - assimétricas
9. outros
     - imports e namespaces
         - import e variáveis
         - bibliotecas de mixins com namespaces
         - bibliotecas úteis: less elements
     - acessibilidade, performance e suporte multi-plataforma
         - contraste
         - conversão de cores para formatos Microsoft
         - conversão data-uri
     - less com javascript
         - javascript embutido em less
         - scripting no browser
         - scripting em node.js
     - algumas opções do compilador
         - linha de comando
         - browser
10. fontes de referência
     - especificações e documentação oficial
     - artigos e tutoriais
     - ferramentas, bibliotecas, utilitários

##arquivos
Todos os códigos usados como exemplos foram testados e compilados usando a versão 1.7 do Less. As fontes `.less` e os resultados `.css` estão todos disponíveis na pasta `examples`, em sub-pastas correspondentes a cada capítulo. Alguns exemplos utilizam arquivos HTML e imagens que estão disponíveis nas pastas `html` e `images`, respectivamente.

Todo o código utilizado neste curso pode ser baixado de:
<http://github.com/argonavisbr/LessCourse> em formato ZIP ou utilizando GIT *clone*.

Este tutorial é compatível e foi testado com Less versão 1.7 (fevereiro 2014). Todo o código CSS incluído na apostila foi gerado a partir do código exibido.

##distribuição e termos de uso
Esta apostila foi desenvolvida especialmente para a DRC.  Trata-se de uma versão derivada e adaptada de um tutorial continuamente desenvolvido pelo autor que é mantido no repositório <http://github.com/argonavisbr/LessCourse>. É distribuído de acordo com a licença Creative Commons Share-Alike, de acordo com os termos descritos no repositório GIT.

## sobre o autor
Helder da Rocha é programador, escritor, professor, ator, músico e artista plástico. Trabalha com tecnologias Web e Java desde 1994. É autor de um livro sobre HTML e CSS publicado em 1996, e sobre JavaScript publicado em 1998, e de diversos tutoriais disponibilizados online sobre Java, XML e tecnologias relacionadas.