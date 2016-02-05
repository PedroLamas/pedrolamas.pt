---
layout: post
status: publish
published: true
title: Windows Mobile Widgets - Parte 1
wordpress_id: 823
wordpress_url: http://www.pedrolamas.com/?p=823
categories:
- Widgets
tags:
- Mobilidade
- Programação
- Windows Mobile
- Windows Mobile 6.5
- Windows Mobile Widgets
---
![Hello World Widget Thumbnail](/wp-content/uploads/2009/07/Hello-World-Widget-Thumbnail.jpg "Hello World Widget Thumbnail")Este é o primeiro de uma série de artigos que tenho em vista, onde pretendo demonstrar como podem utilizar a nova forma de desenvolvimento para o Windows Mobile 6.5: **Widgets**!

Utilizando o suspeito do costume, para dar a conhecer os conceitos base associados ao desenvolvimento numa qualquer tecnologia/linguagem, apresento agora o programa *"Hello World"*, passo a passo! :)

Para esta demonstração vamos precisar do [Windows Mobile 6.5 Developer Tool Kit](/2009/06/04/windows-mobile-6-5-developer-tool-kit/) (disponível [aqui](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=20686a1d-97a8-4f80-bc6a-ae010e085a6e)); depois de instalado no computador, os emuladores de WM 6.5 podem ser encontrados directamente no menu iniciar e depois pesquisando em *Programas \\ Windows Mobile 6 SDK \\ Standalone Emulator Images \\ US English* (sendo que esta última aparecerá conforme a linguagem do Dev. Toolkit que instalaram).

![Windows Mobile 6.5 Emulator Images](/wp-content/uploads/2009/07/Windows-Mobile-6.5-Emulator-Images.jpg "Windows Mobile 6.5 Emulator Images")

Abrindo o emulador "WM 6.5 Professional" e mal este acabe de arrancar, devem ver uma imagem como a seguinte, onde se mostra o *Today* do WM 6.5:

![Windows Mobile 6.5 Professional Today](/wp-content/uploads/2009/07/Windows-Mobile-6.5-Today.jpg "Windows Mobile 6.5 Professional Today")

Feito isto, estamos em condições de começar a desenvolver o nosso programa!

Na sua essência, um Widget nada mais é uma ou mais páginas de Internet, no qual são aplicadas as tecnologias regularmente associadas como HTML, CSS e Flash (sendo que futuramente é de se esperar também o suporte para Silverlight) para a interface, e Javascript para a programação da interacção, e algumas técnicas como por exemplo o AJAX.

Assim, a nossa página *"Hello World"* pode ser resumida simplesmente ao seguinte código:

```html
<html>
<head>
<title>Hello World</title>
</head>
<body>
<p>Hello World</p>
</body>
</html>
```

Copiando o código acima para o bloco de notas do computador, e guardando o resultado como ficheiro de HTML com o nome ***"default.htm"*** (com aspas, de forma a que ao guardar, o Bloco de Notas não adicione a extensão *".txt"* ao nome do ficheiro, acabando com algo como *"default.htm.txt"* o que não é o pretendido), temos a página de arranque do nosso Widget.

De seguida temos que criar o ficheiro de manifesto da aplicação! Este ficheiro tem sempre o nome de "config.xml" e permite ao gestor de Widgets saber o que fazer com este especificamente, definindo assim elementos como a origem, instalação, necessidades especiais, etc.. O formato do ficheiro obedece ao [draft para desenho de Widgets da W3C, de 22 Dezembro de 2008](http://www.w3.org/TR/2008/WD-widgets-20081222/).

Para o nosso exemplo, vamos considerar o seguinte manifesto:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<widget version="1.0" xmlns="http://www.w3.org/ns/widgets" id="">
<name>Hello World</name>
<content src="default.htm" type="text/html" />
<access network="false" />
<icon src="icon.png" />
<description>Hello World Demo Application!</description>
</widget>
```

Mais uma vez, recorremos ao Bloco de Notas para copiar o texto acima para um novo ficheiro, que deverá ser guardado como ***"config.xml"*** (atenção às aspas, que devem ser mantidas no Bloco de Notas!) na mesma pasta do anterior *"default.htm"*.

Analisando o manifesto anterior, podemos que foi indicado o nome de *"Hello World"* para o nosso Widget, a descrição *"Hello World Demo Application"*, o ficheiro de arranque *"default.htm"*, que não vai necessitar do acesso à rede (internet ou local), e ainda que tem como ícone o ficheiro ***"icon.png"***; para ícone podem usar qualquer imagem no formato .ico, .png e .jpg, com um dos seguintes formatos:

-   36x36@96
-   45x45@96
-   64x64@96
-   60x60@128
-   60x60@192
-   90x90@192

No entanto, devo referir que no caso do Windows Mobile 6.5 Standard, este apenas aceita ficheiros .ico, pelo que se pretendermos desenvolver Widgets para as duas plataformas, devemos sempre ter pelo menos um ficheiro .ico, aparecendo a referência para o .ico em último lugar. Por exemplo:

```xml
<icon src="icon.png" />
<icon src="icon.ico" />
```

Terminado este passo, devemos ter neste momento 3 ficheiros: *"default.htm"*, *"config.xml"* e *"icon.png"*.

![Hello World files](/wp-content/uploads/2009/07/Hello-World-files.jpg "Hello World files")

Temos agora de empacotar os três ficheiros, simplesmente seleccionando os mesmos e clicando com o botão direito do rato na selecção, para depois clicar em *Enviar para \\ Pasta comprimida*. Ao fazer isto vai aparecer um novo ficheiro .zip, que contém os 3 outros ficheiros devidamente *"zippados"* (compactados em formato zip). Para que o ficheiro possa ser usado como um Widget, a extensão deve ser alterada de .zip para .wgt

E pronto, temos o nosso Widget prontinho para teste e distribuir! :)

![Hello Word Widget file](/wp-content/uploads/2009/07/Hello-Word-Widget-file.jpg "Hello Word Widget file")

Nesta altura falta-nos apenas testar o nosso Widget e para isso temos o emulador que abrimos anteriormente; para copiar o ficheiro para o emulador, a maneira mais fácil é clicando em *File \\ Configure* e depois no campo *Shared Folder* escolhera pasta onde se encontra o nosso Widget (a pasta da imagem acima). Depois, no emulador propriamente dito ir a *Start*, abrir o *File Manager*, navegar até *\\Storage Card*, e clicar sobre o ficheiro .wgt para instalar o Widget.

![Widget file on File Explorer](/wp-content/uploads/2009/07/Widget-file-on-File-Explorer.jpg "Widget file on File Explorer")

No final da instalação, é criado um novo atalho no *Start* que irá abrir o nosso Widget. E heis o resultado final! :)

![Hello World Widget running](/wp-content/uploads/2009/07/Hello-World-Widget-running.jpg "Hello World Widget running")

Podem [aqui](/2009/07/25/windows-mobile-widgets-parte-1/hello-world/) descarregar o ficheiro *"Hello World.wgt"* que corresponde ao produto acabado.

Espero que tenham gostado deste pequeno tutorial, os próximos serão dentro deste alinhamento, onde tentarei explicar alguns conceitos mais avançados de forma a melhorar-mos os nossos Widgets!
