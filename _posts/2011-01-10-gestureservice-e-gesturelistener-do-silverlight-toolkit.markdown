---
layout: post
status: publish
published: true
title: GestureService e GestureListener do Silverlight Toolkit
wordpress_id: 1645
wordpress_url: http://www.pedrolamas.com/?p=1645
date: 2011-01-10 16:55:04.000000000 +00:00
categories:
- Mobilidade
tags:
- WP7
- WP7Dev
- Windows Phone 7
- Silverlight Toolkit
- GestureService
- GestureListener
---
A esta altura do campeonato, pouco serão os programadores que não utilizam o [Silverlight Toolkit](http://silverlight.codeplex.com/) nas suas aplicações Windows Phone 7, em grande parte devido aos controlos ListPicker e LongListSelector, e às Page Transitions.

De todos os componentes que fazem parte do Toolkit, o GestureService e o GestureListener são provavelmente dos que menos se fala e escreve, mas pessoalmente parecem-me ser dos mais importantes!

Quem ouve falar destes dois componentes relaciona de imediato com as manipulações, como por exemplo fazer Pinch com dois dedos para modificar o zoom aplicado numa imagem; mas estes componentes são capazes de fazer muito mais do que isto!

Suponhamos que temos um controlo Image numa PhoneApplicationPage e que queremos realizar uma acção ao clicar na Image; rapidamente nos vamos aperceber que este controlo não tem um evento Click que possamos subscrever...

De imediato, a solução que vem à cabeça é de simplesmente colocar a imagem dentro de um controlo Button ou HyperlinkButton, estes sim com evento Click; outra solução será utilizarmos o GestureService e o GestureListener e remeter para o evento Tap exposto por este último, conforme podem ver neste exemplo:

```xml
<Image Source="NacaoDoCimbalino.png">
    <sltoolkit:GestureService.GestureListener>
        <sltoolkit:GestureListener Tap="GestureListener_Tap" />
    </sltoolkit:GestureService.GestureListener>
</Image>
```
