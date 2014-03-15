#{12 instalação }
Para instalar Less é preciso ter o Node Package Manager (NPM) instalado. Se o NPM não estiver instalado no seu sistema, 

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
