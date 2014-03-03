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

O Terminal é uma aplicação que está localizada na pasta Utilities, que fica dentro da pasta Applications. Abra o terminal. Ele mostra um cursor piscando no final de uma linha que contém o nome de sua máquina, e o diretório em que você está no momento (o diretório raiz da sua conta). Digite ls

A lista de arquivos e diretórios é a lista dos arquivos e diretórios da sua conta. Veja que entre eles há um chamado Desktop. Digite ls Desktop (com 'D' maiúsculo):

Agora a listagem contém o conteúdo do seu Desktop.

O Mac não vem com uma ferramenta nativa para instalar pacotes. É preciso instalar uma. As mais populares são Fink, MacPorts e HomeBrew. Talvez a mais fácil de usar seja HomeBrew. 

-- como instalar o home brew

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

Agora abra o CSS e veja o resultado.

##pré-processador estático

O programa `lessc` que executamos é um pré-processador estático. Ele gera o arquivo CSS que iremos usar. Observe que o CSS gerado é bem maior e têm mais dados repetidos (cores) que o arquivo LESS. 

Você não precisa usar sempre linha de comando. Várias ferramentas de Web Design têm suporte a LESS ou permitem a instalação de plug-ins que geram automaticamente o CSS a cada alteração de um documento LESS. 

Uma ferramenta que faz a conversão sem precisar da linha de comando é o Less.app (para Apple). http://incident57.com/less/

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

## watchers

## versões do Less

## ferramentas

## outros recursos do lessc
