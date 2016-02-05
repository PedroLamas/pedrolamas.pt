---
layout: post
status: publish
published: true
title: Performance das Aplicações de Windows Phone
wordpress_id: 2416
wordpress_url: http://www.pedrolamas.com/?p=2416
date: 2012-02-19 20:21:57.000000000 +00:00
categories:
- Mobilidade
tags:
- Windows Phone
- WP7Dev
- WPDev
- Performance
---
Desenvolver para Windows Phone ou para qualquer outro dispositivo móvel não é o mesmo que desenvolver para o desktop ou mesmo para a web!

São várias as preocupações que devemos ter, como a baixa capacidade de processamento, a memória e o espaço de storage limitados, o consumo de energia, etc.

No caso do Windows Phone, existe uma série de considerações que se deve ter em conta, e que se seguidas à risca, permitem melhorar (e muito) a performance das aplicações!

Imagens
-------

Sempre que possível, utilizar imagens em formato JPEG em vez de PNG. É certo que os ficheiros JPEG tem perda de qualidade e que não suportam transparência, mas sempre que estes dois pontos não forem impeditivos (por exemplo, em fotos ou fundos da aplicação), este é o formato preferencial dado que a sua descodificação e leitura é em geral mais rápida.

Outra questão nas imagens é relativamente ao seu carregamento fora do thread da interface, algo que já anteriormente referi [neste artigo](/2011/10/28/bitmapimage-no-windows-phone-mango/).

Com o Expression Design é possível criar imagens vectoriais que são elementos em formato XAML; mais uma vez, sempre que possível, prefiram guardar e usar essas imagens em formato estático em vez de usarem o XAML!

Recursos
--------

Os recursos que adicionados à aplicação devem ter a propriedade **Build Action** com o valor **Content** e não **Resource**. Isto porque o Windows Phone está optimizado para acesso a ficheiros e streams de rede, mas não de memória. Se assim fizerem os recursos serão incluídos no ficheiro .XAP em vez de serem compilados directamente na assembly binária da aplicação.

Bitmap Caching
--------------

Os elementos visuais em que for manipulada a propriedade [UIElement.Opacity](http://msdn.microsoft.com/en-us/library/system.windows.uielement.opacity(v=vs.95).aspx) podem sofrer de um melhoramento de performance se colocarem na propriedade [UIElement.CacheMode](http://msdn.microsoft.com/en-us/library/system.windows.uielement.cachemode(v=vs.95).aspx) o valor [BitmapCache](http://msdn.microsoft.com/en-us/library/system.windows.media.bitmapcache(VS.95).aspx); o que isto vai garantir é que o sistema operativo vai guardar uma imagem do estado da composição tal como ela está com cada valor alterado da opacidade, e assim poderá recorrer a ela sem necessidade de fazer nova renderização.

ProgressBar
-----------

Sempre que tenham operações que vão demorar algum tempo (por exemplo, aceder à internet), recorram a uma barra de progresso na vossa aplicação. Apesar do Windows Phone incluir um controlo [ProgressBar](http://msdn.microsoft.com/en-us/library/system.windows.controls.progressbar(v=vs.95).aspx), este não é adequado dado que a sua animação é feita no thread da interface, bloqueando a mesma; a alternativa é utilizarem o **PerformanceProgressBar** que podem encontrar no [Silverlight for Windows Phone Toolkit](silverlight.codeplex.com).

Outra alternativa é recorrer ao [ProgressIndicator](http://msdn.microsoft.com/en-us/library/microsoft.phone.shell.progressindicator(v=vs.92).aspx) no [SystemTray](http://msdn.microsoft.com/en-us/library/microsoft.phone.shell.systemtray(v=vs.92).aspx) do sistema.

Web
---

Tomem em atenção que ao utilizar o [WebClient](http://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.95).aspx), este devolve o resultado no thread de composição da interface, pelo que qualquer tratamento da informação poderá bloquear a mesma!

Sempre que possível, utilizem o [HttpWebRequest](http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.95).aspx) em vez do [WebClient](http://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.95).aspx), dado que o retorno é feito num thread separado e que não irá bloquear a interface.

Bottom line...
--------------

Estas são as considerações que considerei mais importantes, mas existem outras que estão registadas [neste artigo](http://msdn.microsoft.com/en-us/library/ff967560(v=VS.92).aspx) no MSDN.

Basicamente, a mensagem a passar é de que pensem sempre duas vezes antes de começarem a escrever código para a vossa aplicação! :)
