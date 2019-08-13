---
layout: post
status: publish
published: true
title: ! 'Cimbalino Windows Phone Toolkit: ApplicationBarBehavior'
wordpress_id: 2252
wordpress_url: http://www.pedrolamas.com/?p=2252
date: 2011-11-30 18:41:04.000000000 +00:00
categories:
- Mobilidade
tags:
- Cimbalino
- Windows Phone
- WP7Dev
- WPDev
- ApplicationBar
- ApplicationBarBehavior
---
![](wp-content/uploads/2011/11/Cimbalino-Windows-Phone-Toolkit.png "Cimbalino Windows Phone Toolkit")A [ApplicationBar](http://msdn.microsoft.com/en-us/library/microsoft.phone.shell.applicationbar(v=vs.92).aspx) disponível para utilização no desenvolvimento de aplicações para Windows Phone foi desde o primeiro dia uma grande dor de cabeça para os programadores, pelo simples facto de este controlo não ser um [FrameworkElement](http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement(v=VS.95).aspx).

Quer isto dizer que não é possível aplicar Styles ou Templates à ApplicationBar, e mais importante ainda, não é possível fazer Binding das suas propriedades, algo que complica e muito quem quer utilizar uma arquitectura baseada em MVVM!

É certo que a arquitectura MVVM não é completamente fechada e limitada, e como tal existem alternativas para ultrapassar este problema (por exemplo, [esta](http://geekswithblogs.net/lbugnion/archive/2010/04/09/using-commands-with-applicationbarmenuitem-and-applicationbarbutton-in-windows-phone-7.aspx) e [esta](http://geekswithblogs.net/lbugnion/archive/2010/06/08/two-small-issues-with-windows-phone-7-applicationbar-buttons-and.aspx))!

No meu caso, de forma a poder utilizar a Application Bar nas minhas aplicações de uma forma mais "MVVM'ed", criei o **ApplicationBarBehavior** que podem encontrar no [Cimbalino Windows Phone Toolkit](http://cimbalino.org)!

Este [Behavior](http://msdn.microsoft.com/en-us/library/ff726531(v=expression.40).aspx), quando aplicado no elemento LayoutRoot de uma PhoneApplicationPage, permite criar e manter uma Application Bar com propriedades Bindable de forma a manter uma arquitectura MVVM mais consistente.

Para melhor explicar, nada como um exemplo:

```xml
<!-- Restante código -->

<Grid x:Name="LayoutRoot" Background="Transparent">
    <i:Interaction.Behaviors>
        <cimbalino:ApplicationBarBehavior>
            <cimbalino:ApplicationBarIconButton Command="{Binding AddItemCommand, Mode=OneTime}" IconUri="/Images/appbar.add.rest.png" Text="add" IsVisible="{Binding IsSelectionDisabled}" />
            <cimbalino:ApplicationBarIconButton Command="{Binding EnableSelectionCommand, Mode=OneTime}" IconUri="/Images/appbar.manage.rest.png" Text="select" IsVisible="{Binding IsSelectionDisabled}" />
            <cimbalino:ApplicationBarIconButton Command="{Binding DeleteItemsCommand, Mode=OneTime}" CommandParameter="{Binding SelectedItems, ElementName=ItemsMultiselectList}" IconUri="/Images/appbar.delete.rest.png" Text="delete" IsVisible="{Binding IsSelectionEnabled}" />
        </cimbalino:ApplicationBarBehavior>
    </i:Interaction.Behaviors>

    <!-- Restante código -->

</Grid>

<!-- Restante código -->
```

No excerto de código acima podemos ver o ApplicationBarBehavior com uma série de controlos ApplicationBarIconButton (tal como seria feito normalmente), e rapidamente nos apercebemos de algumas novas propriedades como o Command, CommandParameter, e IsVisible (algo que não encontramos de base na ApplicationBar do Windows Phone); apesar de neste exemplo isso não ser visível, também as propriedades Text e IconUri são Bindable!

Para melhor explorarem o ApplicationBarBehavior, proponho que façam [download](http://cimbalino.org) do código completo do Cimbalino Windows Phone Toolkit do GitHub, e depois executem a solução BindableApplicationBar que encontram na pasta "samples", ou como alternativa mais rápida, acedam ao seguinte link para a MSDN Code Gallery:

[download id="8" format="2"]
