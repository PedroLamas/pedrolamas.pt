---
layout: post
status: publish
published: true
title: Utilizem o Windows Phone Connect Tool com o Visual Studio
wordpress_id: 1616
wordpress_url: http://www.pedrolamas.com/?p=1616
date: 2010-12-02 19:05:07.000000000 +00:00
categories:
- Mobilidade
tags:
- Programação
- Windows Phone
- WP7
- WP7Dev
- Windows Phone 7
---
É normal utilizar-se o Zune para fazer a ligação de equipamentos Windows Phone 7 ao computador, e com essa ligação activa fazer a depuração de aplicações com o Visual Studio.

No entanto, esta não é a melhor opção, conforme vim recentemente a comprovar!

Isto porque, enquanto a ligação com o Zune está activa, não é possível interagir com nenhuma API multimédia, dado que este bloqueia a base de dados multimédia local! Alguns exemplos destas API's são o caso dos Media Launchers e Choosers ou das capacidades de apresentar conteúdos de vídeo no Silverlight.

A solução passa por recorrer ao **Windows Phone Connect Tool** para efectuar a ligação do computador ao equipamento físico!

Esta ferramenta faz parte do [Windows Phone Developer Tools October 2010 Update](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=49b9d0c5-6597-4313-912a-f0cca9c7d277) (já [aqui](/2010/10/22/windows-phone-developer-tools-october-2010-update/) falado anteriormente), pelo que terão de instalar esta actualização antes para poderem utilizar esta ferramenta.

Podem encontrar [aqui](http://msdn.microsoft.com/en-us/library/gg180729(v=VS.92).aspx) mais informações sobre esta ferramenta essencial, incluindo alguma informação de ajuda para possíveis problemas!
