---
layout: post
status: publish
published: true
title: PhoneApplicationFrame.CanGoBack != NavigationService.CanGoBack
wordpress_id: 2618
wordpress_url: http://www.pedrolamas.com/?p=2618
date: 2012-06-21 18:24:52.000000000 +01:00
categories:
- Mobilidade
tags:
- WP7
- WP7Dev
- WPDev
- Bug
- PhoneApplicationFrame
- NavigationService
- RemoveBackEntry
- CanGoBack
---
Apercebi-me hoje que a funcionalidade de remover entradas da pilha de navegação do Windows Phone introduziu uma falha no [PhoneApplicationFrame](http://msdn.microsoft.com/en-us/library/microsoft.phone.controls.phoneapplicationframe(v=vs.92).aspx), mais especificamente na propriedade [CanGoBack](http://msdn.microsoft.com/en-us/library/system.windows.controls.frame.cangoback(v=vs.92).aspx)!

É muito simples de simular este problema:

-   Começamos por criar duas páginas
-   Na página de arranque da aplicação, adicionar um botão que quando clicado inicie a navegação para a segunda página (colocando assim um item na pilha de navegação)
-   Estando a segunda página carregada, chamar o método [NavigationService.RemoveBackEntry](http://msdn.microsoft.com/en-us/library/system.windows.navigation.navigationservice.removebackentry(v=vs.92).aspx) (removemos assim o item anteriormente adicionada à pilha de navegação)
-   Obter o objecto PhoneApplicationFrame através do código **(PhoneApplicationFrame)App.Current.RootVisual**
-   Comparar os valores das propriedades [NavigationService.CanGoBack](http://msdn.microsoft.com/en-us/library/system.windows.navigation.navigationservice.cangoback(v=vs.92)) e [PhoneApplicationFrame.CanGoBack](http://msdn.microsoft.com/en-us/library/system.windows.controls.frame.cangoback(v=vs.92).aspx): o primeiro indica **false**, o valor correcto e esperado, ao passo que o segundo indica **true**, algo que não deveria ser verdade dado que acabamos de limpar a stack de navegação ao remover a única entrada que lá existia!!!

Recorrendo a uma ferramenta de *reflection* para ver o código do PhoneApplicationFrame, facilmente pude-me aperceber que, enquanto os métodos Navigate, GoBack, RemoveBackEntry e outros, internamente chamam os mesmos no NavigationService, o mesmo já não acontece com as propriedades CanGoBack e CanGoForward, as quais são apenas actualizadas após uma navegação!

E aí está o problema: os valores dessas propriedades deveriam ser obtidos directamente do NavigationService como os restantes métodos, ou então elas deveriam também ser actualizadas aquando do RemoveBackEntry; como não são, surge este comportamento errado!

Para rapidamente poderem confirmar este comportamento, podem fazer [download](http://www.pedrolamas.com/2013/01/07/the-phoneapplicationframe-bug-is-still-alive/) de uma aplicação que criei para esse efeito!

Basta executarem o código, clicarem em "Navigate to self", confirmarem que a stack passou a 1, clicar num dos dois botões de "RemoveBackEntry" e ver que as propriedades CanGoBack estão agora com valores diferentes!

Estou neste momento a mitigar esta situação na [implementação do INavigationService](https://github.com/Cimbalino/Cimbalino-Phone-Toolkit/blob/master/src/Cimbalino.Phone.Toolkit/Services/NavigationService.cs) do [Cimbalino Windows Phone Toolkit](http://cimbalino.org), mas a verdade é que pelo que pude constatar, as implementações de serviços semelhantes que andam por aí sofrem do mesmo problema...

Talvez amanhã tenha novidades quanto à resolução no Cimbalino! ;)

**Actualização:** neste momento este é já um bug confirmado pela própria Microsoft, conforme pode ser [aqui](http://forums.create.msdn.com/forums/p/106075/625214.aspx) visto!
