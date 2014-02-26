#{8: operações }
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

##loops
##merge
##when
##exercícios