#{2: instalação }
##o que é less?

Less é um pré-processador CSS. Ele gera CSS. É também uma extensão do CSS: um documento CSS é um documento Less válido. Less acrescenta vários recursos que tornam mais eficientes a criação e manutenção de folhas de estilo do CSS. Documentos Less são bem mais curtos que documentos CSS e têm muito menos repetição de código. Alguns dos recursos que Less acrescenta ao CSS incluem funções, variáveis, mixins, condicionais e recursos comuns em linguagens de programação. Less também facilita a criação de folhas de estilo independentes de browser e plataforma.

##como obter e instalar

Less roda em JavaScript. Ainda é muito ineficiente usar Less no browser, portanto a melhor maneira é usar Less em desenvolvimento para gerar os arquivos CSS, e carregar os CSS normalmente. 

Fora do browser, Less roda como uma aplicação Node.js. Node.js é uma plataforma baseada na máquina JavaScript do Google, a V8. Aplicações Node.js rodam em máquinas Apple, Linux, Windows. Portanto, antes de instalar o Less localmente é preciso instalar o Node.js, e depois o NPM (Node Package Manager). Se você desenvolve em uma ferramenta como o WebStorm (PHPStorm), NetBeans, Aptana Studio, é possível instalar suporte a Node.js e o Less como plug-in se ele já não estiver instalado. A instalação do Node.js e NPM é bem simples no Linux e no Mac, e um pouco mais complicada no Windows. Independente da plataforma, você precisará usar o terminal ou console.

### Instalação do Node e NPM Linux

Se você usa Linux provavelmente está familiarizado com o terminal. O Node.js e NPM podem ser instalados através de um gerenciador de pacotes.

Se o seu sistema for Ubuntu (12/13), use

```
sudo apt-get install nodejs npm
```

Em Fedora e CentOS use:

```
sudo yum install nodejs rpm
```

### Instalação do Node e NPM no MacOS

A instalação MacOS é simples como a do Linux, mas o típico usuário Mac não costuma usar o terminal. Iremos necessitar do terminal para instalar o Node, NPM e Less, e posteriormente para usar o Less para gerar CSS (a menos que você use uma ferramenta de desenvolvimento). 

O `Terminal` é uma aplicação que está localizada na pasta `Utilities`, que fica dentro da pasta `Applications`. Abra o `Terminal`. Ele mostra um cursor piscando no final de uma linha que contém o nome de sua máquina, e o diretório em que você está no momento (o diretório raiz da sua conta). Digite `ls`:

```
Last login: Tue Mar 11 09:59:45 on console
Vaders-MacBook-Pro:~ darthvader$ ls
Pictures                 Public
Adlm				     Snapshots
Applications		     WebstormProjects
Code					 Desktop				
Documents				 Downloads		
Dropbox					 Library			
MoviesMusic
Vaders-MacBook-Pro:~ darthvader$ 
```

A lista de arquivos e diretórios é a lista dos arquivos e diretórios da sua conta. Veja que entre eles há um chamado Desktop. Digite `cd Desktop` (com `D` maiúsculo). Você não precisa digitar o nome inteiro. Se digitar algumas letras tecle `<TAB>` e  nome vai se autocompletar:

```
Vaders-MacBook-Pro:Desktop darthvader$ cd Desktop
```

Agora digite `ls` de novo. O terminal irá listar o conteúdo da sua pasta `Desktop`:

```
Vaders-MacBook-Pro:Desktop darthvader$ ls
2013 
_1674193519_n.jpg
_1375778272_o.jpg
...
```

Agora a listagem contém o conteúdo do seu Desktop. Para voltar ao seu diretório 'home' digite simplesmente `cd`.

O Mac não vem com uma ferramenta nativa para instalar pacotes. É preciso instalar uma. As mais populares são Fink, MacPorts e HomeBrew. Veja se você tem alguma dessas ferramentas instaladas da seguinte forma:

macports
fink
brew

Se tiver, você pode instalar o NPM usando uma das linhas de comando abaixo. O Fink e o MacPorts poderão pedir senha de administrador.

macports
fink
brew

### instalando o homebrew
Se você não tem uma ferramenta de gerenciamento de pacotes instalada, talvez a mais fácil de usar seja HomeBrew. Com ela podemos instalar o Less facilmente sem precisar de privilégios de administrador.

Mas para instalar o HomeBrew é preciso ter esses privilégios. Então primeiro instale o HomeBrew:

instalação do home brew

Depois que o HomeBrew estiver instalado, abra o terminal, mude para o seu diretório home (`cd`) e rode:

```
brew install node npm
```

Com isto o gerenciador de pacotes do Node será instalado. Agora vá para a seção seguinte para instalar o Less pelo NPM.

### Instalação do Node e NPM no Windows

--- falta

### Instalação do Less em linha de comando

Agora que o NPM está instalado, pode-se instalar o pré-processador Less usando:

```
npm install -g less
```

Para testar a instalação, crie um arquivo teste.less contendo:

```
@cor-principal: #FF8800;

#secao {
   color: @cor-principal;
   border-color: @cor-principal;
   background: lighten(@cor-principal, 50%);
   a:hover {
   	  color: darken(@cor-principal, 30%);
   }
}
```

Grave o arquivo em sua área de trabalho.

Através do terminal, mude até o diretório correspondente à pasta da sua área de trabalho. Verifique que o arquivo teste.less está nessa pasta:

```
computador$ ls
teste.less
```

Agora execute

```
lessc teste.less
```

O arquivo CSS gerado é listado na tela. Para converter o resultado para um arquivo faça:

```
lessc teste.less > teste.css
```

Agora abra o CSS e veja o resultado. Deve ser semelhante ao CSS abaixo:

```
#secao {
  color: #ff8800;
  border-color: #ff8800;
  background: #ffffff;
}
#secao a:hover {
  color: #663600;
}
```

##pré-processador estático

O programa `lessc` que executamos é um pré-processador estático. Ele gera o arquivo CSS que iremos usar. Observe que o CSS gerado é bem maior e têm mais dados repetidos (cores) que o arquivo LESS. 

Você não precisa usar sempre linha de comando. Várias ferramentas de Web Design têm suporte a LESS ou permitem a instalação de plug-ins que geram automaticamente o CSS a cada alteração de um documento LESS. 

Uma ferramenta que faz a conversão sem precisar da linha de comando é o Less.app (para Apple). <http://incident57.com/less/>

##pré-processador dinâmico

Você também pode usar folhas de estilo Less diretamente no HTML, para processamento no browser. Essa alternativa é muito prática durante o desenvolvimento, mas é lenta e ineficiente para uso em produção. 

É necessário usar o script less.js, que pode ser baixado de https://github.com/less ou usado através de um CDN.

Usando `less.js` local:

```
 <script src="less.js" type="text/javascript"></script>
```

Através de CDN:

```
 <script src="//cdnjs.cloudflare.com/ajax/libs/less.js/1.6.3/less.min.js"></script>
```

Depois deve-se incluir a folha de estilos Less usando 

```
 <link rel="stylesheet/less" type="text/css" href="teste.less" />
```

É importante que as folhas de estilo Less sejam carregadas **antes** do script. Elas serão compiladas individualmente, portanto não é possível compartilhar variáveis e outros recursos do Less entre elas. Mas o CSS resultante continuará seguindo as mesmas regras de cascade, na ordem em que os estilos Less foram declarados.

## versões do Less
Este curso foi preparado e testado com a versão 1.7 do Less. O Less não tem uma especificação formal e toda a documentação está disponível no site e no GitHub do projeto. Muitos detalhes da linguagem não estão documentados. Se você estiver usando uma versão anterior do Less, vários exemplos poderão não funcionar. Se estiver usando uma versão mais nova, alguns comandos poderão ser diferentes ou até mais simples.

## ferramentas
Existem várias ferramentas que ajudam a trabalhar com Less.

## exercícios
