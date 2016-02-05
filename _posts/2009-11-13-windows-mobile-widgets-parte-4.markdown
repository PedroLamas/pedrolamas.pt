---
layout: post
status: publish
published: true
title: Windows Mobile Widgets - Parte 4
wordpress_id: 1087
wordpress_url: http://www.pedrolamas.com/?p=1087
categories:
- Widgets
tags:
- Mobilidade
- Programação
- Windows Mobile
- Windows Mobile 6.5
- Windows Mobile Widgets
---
![Windows Mobile Widgets Part 3](/wp-content/uploads/2009/11/Windows-Mobile-Widgets-Part-3.jpg "Windows Mobile Widgets Part 3")Em mais um artigo da série sobre desenvolvimento de [Windows Mobile Widgets](/tag/windows-mobile-widgets/), hoje venho falar sobre o seu modelo de objectos (object model) de Javascript.

Voltando ao essencial, um Widget é apenas HTML + CSS + Javascript... sendo o HTML e o CSS utilizado para o desenho da interface, o Javascript serve para a implementação da lógica e programação do Widget.

Os Widgets de Windows Mobile disponibilizam um objecto chamado ***widget***, que permite o acesso a algumas informações do estado do dispositivo, bem como a algumas funcionalidades relacionadas com o próprio Widget.

Esta é a definição do objecto *widget*:

-   Propriedades:
    -   **authorEmail** - O endereço de e-mail do autor do Widget, correspondendo ao valor do atributo*email* do elemento *\<author\>* no manifesto
    -   **authorName** - O nome do autor do Widget, correspondendo ao valor do elemento *\<author\>* no manifesto
    -   **authorURL** - O nome do autor do Widget, correspondendo ao valor do atributo *href* do elemento *\<author\>* no manifesto
    -   **currentIcon** - O icon que está em uso neste Widget; este é um object do tipo ***WidgetIcon***.
    -   **description** - A descrição do Widget, correspondendo ao valor do elemento *\<description\>* no manifesto
    -   **height** - A altura do Widget em pixels
    -   **identifier** - O identificador único do Widget, correspondendo ao atributo *id* do elemento *\<widget\>* no manifesto
    -   **locale** - O identificador do idioma (*locale*) em uso, conforme as definições regionais (*Regional Settings*) do dispositivo
    -   **menu** - O menu do Widget; este é um objecto do tipo ***Menu***.
    -   **name** - O nome do Widget, correspondendo ao valor do elemento *\<name\>* no manifesto
    -   **version** - A versão do Widget, correspondendo ao valor do atributo *version* do elemento *\<widget\>*
    -   **width** - A largura do Widget em pixels

-   Métodos
    -   **createObject(type)** - Cria uma instancia do tipo indicado
    -   **preferenceForKey(key)** - Obtém os dados persistentes relativos ao identificador indicado
    -   **setPreferenceForKey(value, key)** - Guardar persistentemente os dados indicados, relacionando-os com um identificador

-   Eventos
    -   **onHide** - Ocorre quando a janela do Widget perde o foco (minimizar)

    -   **onShow** - Ocorre ao focar a janela do Widget

Para efeito de exemplo, vamos utilizar novamente o nosso "Hello World" (a versão [deste](/2009/08/21/windows-mobile-widgets-parte-2/) post), e fazer com que este apresente no ecrã principal o seu nome e versão, alterando para isso o ficheiro *default.htm*:

[html highlight="8,14"]\<html\> \<head\> \<title\>Hello World\</title\> \<script type="text/javascript" src="resources.js"\>\</script\> \<script type="text/javascript"\> window.onload = function() { HelloWorldLabel.innerText = Resources.HelloWorldText; WidgetInformation.innerText = widget.name + " v" + widget.version; }; \</script\> \</head\> \<body\> \<p id="HelloWorldLabel"\>\</p\> \<p id="WidgetInformation"\>\</p\> \</body\> \</html\>[/html]

Abrindo o Widget no emulador, o resultado deverá ser como a seguinte imagem:

![Hello World Widget with identification](/wp-content/uploads/2009/11/Hello-World-Widget-with-identification.jpg "Hello World Widget with identification")

Como pudemos ver acima, existem ainda pelo menos mais dois objectos: o *WidgetIcon* e o *Menu*.

O *WidgetIcon* tem a seguinte estrutura:

-   Propriedades
    -   **height** - A altura do icon
    -   **src** - O url do icon
    -   **width** - A largura do icon

Por sua vez, o *Menu* apresenta a seguinte estrutura:

-   Propriedades
    -   **leftSoftKeyIndex** - Utilizado pelo método setSoftkey para representar a tecla (*soft-key*) da esquerda
    -   **rightSoftKeyIndex** - Utilizado pelo método setSoftkey para representar a tecla (*soft-key*) da direita

-   Métodos
    -   **append(item)** - acrescente um objecto do tipo *MenuItem* à tecla (*soft key*) da direita
    -   **clear()** - Limpa o conteúdo do menu principal
    -   **createMenuItem(id)** - cria um novo objecto ***MenuItem*** com o identificador indicado
    -   **getMenuItemById(id)** - obtém o objecto *MenuItem* dado o seu identificador
    -   **remove(item)** - remove do menu o objecto *MenuItem* indicado
    -   **setSoftKey(item, id)** - atribui o *MenuItem* a uma tecla (*soft key*) específica.

E aqui aparece mais um objecto, o *MenuItem*, que tem a seguinte estrutura:

-   Propriedades
    -   **text** - O texto a apresentar neste item

-   Eventos
    -   **onSelect** - Ocorre quando o item é seleccionado

Utilizando agora estes conceitos, vamos criar um novo item "About" para o nosso menu:

[html highlight="10,11,12,13,14,15,16"]\<html\> \<head\> \<title\>Hello World\</title\> \<script type="text/javascript" src="resources.js"\>\</script\> \<script type="text/javascript"\> window.onload = function() { HelloWorldLabel.innerText = Resources.HelloWorldText; };

var menu = widget.menu; var aboutMenuItem = menu.createMenuItem(1001); aboutMenuItem.text = "About"; aboutMenuItem.onSelect = function() { alert(widget.name + " v" + widget.version); }; menu.setSoftKey(aboutMenuItem, menu.leftSoftKeyIndex); \</script\> \</head\> \<body\> \<p id="HelloWorldLabel"\>\</p\> \</body\> \</html\>[/html]

A imagem seguinte mostrar o resultado obtido; reparem bem na tecla (soft key) da esquerda:

![Hello World Widget custom Menu](/wp-content/uploads/2009/11/Hello-World-Widget-custom-Menu.jpg "Hello World Widget custom Menu")

Podem [aqui](/2009/11/13/windows-mobile-widgets-parte-4/hello-world-2/) encontrar o ficheiro "Hello World.wgt" com o resultado das alterações... e até à próxima! :)
