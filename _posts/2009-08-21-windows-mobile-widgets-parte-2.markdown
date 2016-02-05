---
layout: post
status: publish
published: true
title: Windows Mobile Widgets - Parte 2
wordpress_id: 872
wordpress_url: http://www.pedrolamas.com/?p=872
categories:
- Widgets
tags:
- Mobilidade
- Programação
- Windows Mobile
- Windows Mobile 6.5
- Windows Mobile Widgets
---
Na continuação da série de artigos sobre [Windows Mobile Widgets](/tag/windows-mobile-widgets/) que tenho pensada, este segundo artigo focará em essencial como podemos "internacionalizar" os nossos Widgets, ou seja, como fazer para que a interface do Widget seja multi-língua (*Localization*), apresentando o idioma correcto conforme as *Regional Settings* (Definições Regionais) do PDA onde ele estiver a ser executado.

A localização da interface de Widgets consiste em dividir a aplicação em "recursos", sejam eles imagens, javascript, ou outros tipos de ficheiros, e colocar cópias desses recursos devidamente localizados em pastas individuais que indicam qual o idioma (*Locale*).

Antes de mais, há que perceber o conceito de *Locale ID* (ou LCID): trata-se de um código que identifica o idioma utilizado num dado elemento. Os códigos LCID que podem ser utilizados nos Widgets do Windows Mobile podem ser consultados [aqui](http://msdn.microsoft.com/en-us/goglobal/bb896001.aspx).

Utilizando por base o exemplo "Hello World" apresentado no artigo anterior, para que o interface deste widget tenha suporte para multi-língua, teremos de remover o texto da própria interface e fazer com que este seja carregado dinamicamente de um ficheiro javascript (.js), ficheiro este que será posteriormente replicado para as línguas que pretendermos. Assim, o primeiro passo será criar o ficheiro "***resources.js***" com os recursos do nosso Widget:

```javascript
var Resources = {
 HelloWorldText: "Hello World"
};
```

De seguida, vamos alterar o ficheiro de interface "***default.htm***" para que este consuma o ficheiro de recursos:

[html highlight="4,5,6,7,8,9,12"]\<html\> \<head\> \<title\>Hello World\</title\> \<script type="text/javascript" src="resources.js"\>\</script\> \<script type="text/javascript"\> window.onload = function() { HelloWorldLabel.innerText = Resources.HelloWorldText; }; \</script\> \</head\> \<body\> \<p id="HelloWorldLabel"\>\</p\> \</body\> \</html\>[/html]

As linhas marcadas acima indicam os locais em que o ficheiro original foi alterado.

Para efeitos de exemplo, vamos fazer agora a tradução dos recursos para Português de Portugal. Para isso, vamos criar directamente na raiz do nosso Widget uma nova pasta e dar-lhe o nome de "***pt***"; depois criamos uma cópia do ficheiro "*resources.js*" dentro da pasta "*pt*". Falta agora modificar o conteúdo do ficheiro, traduzindo os textos para a língua correcta, sendo o resultado final como o seguinte:

[javascript highlight="2"]var Resources = { HelloWorldText: "Olá Mundo" };[/javascript]

Esta é a estrutura de ficheiros do nosso Widget já alterado:

![Hello World localized files](/wp-content/uploads/2009/08/Hello-World-localized-files.jpg "Hello World localized files")

Falta apenas empacotar os ficheiros seguindo as indicações do [primeiro artigo](/2009/07/25/windows-mobile-widgets-parte-1/), instalar e testar!

Para correctamente podermos ver o nosso Widget em Português, teremos que indicar ao PDA que pretendemos utilizar as Definições Regionais de Portugal, utilizando para isso as opções que podemos encontram em *Start \\ Settings \\ System \\ Regional Settings*, e alterando as definições tal como mostra a imagem seguinte:

[![Regional Settings](/wp-content/uploads/2009/08/Regional-Settings.jpg "Regional Settings")](/wp-content/uploads/2009/08/Regional-Settings.jpg)

Irá aparecer um aviso para reiniciar o PDA, que no nosso caso poderá ser ignorado visto que para a execução de Widgets a alteração das Definições Regionais são imediatas.

De seguida executando o Widget, poderemos ver a interface em Português de Portugal tal como pretendiamos:

[![Hello World Widget running localized](/wp-content/uploads/2009/08/Hello-World-Widget-running-localized.jpg "Hello World Widget running localized")](/wp-content/uploads/2009/08/Hello-World-Widget-running-localized.jpg)

Podem [aqui](/2009/07/25/windows-mobile-widgets-parte-2/localized-hello-world/) descarregar o ficheiro "Localized Hello World.wgt" que tem o resultado desta demonstração... encontramo-nos no próximo artigo! ;)
