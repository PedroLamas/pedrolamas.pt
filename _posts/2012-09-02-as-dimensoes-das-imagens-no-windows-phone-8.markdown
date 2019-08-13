---
layout: post
status: publish
published: true
title: As dimensões das imagens no Windows Phone 8
wordpress_id: 2664
wordpress_url: http://www.pedrolamas.com/?p=2664
date: 2012-09-02 16:22:26.000000000 +01:00
categories:
- Mobilidade
tags:
- WPDev
- Windows Phone 8
- WP8
- WP8Dev
---
Quando o Windows Phone 8 foi apresentado pelo Joe Belfiore à umas semanas atrás ficamos a conhecer as resoluções dos novos equipamentos.

Estas resoluções vão desde o 800x480 (WVGA) que encontramos nos equipamentos actuais com o Windows Phone 7, até ao 1280x720 (720p) e 1280x768 (WXGA).

Para além disso, o novo Start Screen que vamos encontrar no WP7.8 e no WP8 apresenta uma maior ocupação do ecrã, visto que desaparece a seta com o círculo que podemos encontrar actualmente no canto superior direito.

[![](wp-content/uploads/2012/09/WP7-and-WP8-Start-Screens-thumb.png "WP7 and WP8 Start Screens")](wp-content/uploads/2012/09/WP7-and-WP8-Start-Screens.png)

A imagem anterior apresenta à esquerda o actual WP7 e à direita o WP8, e facilmente vemos que os Tiles de 173x173 foram substituídos por outros bastante maiores!

Recentemente tive acesso a uma ROM (versão beta) do Windows Phone 8 e ao extrair os Resources de imagens dos ficheiros binários, pude confirmar essas mesmas medidas, com as quais construí a seguinte tabela:

||
|**Resolution**|**Big Tiles**|**Small Tiles**|**AppBar Icons**|
|800x480|210x210|99x99|48x48|
|1280x720|314x314|147x147|72x72|
|1280x768|333x333|156x156|76x76|

Note-se que neste momento não há qualquer informação oficial destas dimensões, pelo que deveremos ainda assim aguardar pela confirmação no SDK final; no entanto serve de guia para os programadores e designers saberem o que lhes espera... ;)
