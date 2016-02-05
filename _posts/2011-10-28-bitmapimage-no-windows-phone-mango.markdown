---
layout: post
status: publish
published: true
title: BitmapImage no Windows Phone "Mango"
wordpress_id: 2170
wordpress_url: http://www.pedrolamas.com/?p=2170
date: 2011-10-28 14:56:29.000000000 +01:00
categories:
- Mobilidade
tags:
- Silverlight
- Windows Phone
- WP7
- WP7Dev
- WPDev
- BitmapImage
- CreateOptions
---
Várias alterações foram feitas no Windows Phone "Mango" para melhorar a performance das aplicações, grande parte delas totalmente transparente para os programadores!

Uma delas tem a ver com o carregamento de imagens no [BitmapImage](http://msdn.microsoft.com/en-us/library/system.windows.media.imaging.bitmapimage(v=VS.95).aspx): por omissão, todas as imagens são carregadas apenas quando necessárias, mas esse trabalho é feito no thread principal, causando o bloqueio (ou atraso!) da aplicação até que a imagem seja completamente carregada; ou seja, se a intenção era de fazer o carregamento das imagens num thread separado para não bloquear a interface, teriamos que implementar nós mesmos essa funcionalidade!

Neste momento, existe a possibilidade de deixar que o Silverlight no Windows Phone se encarregue de carregar as imagens assincronamente em threads separados do principal, recorrendo à propriedade [BitmapImage.CreateOptions](http://msdn.microsoft.com/en-us/library/system.windows.media.imaging.bitmapimage.createoptions(v=VS.95).aspx).

Esta propriedade do tipo [BitmapCreateOptions](http://msdn.microsoft.com/en-us/library/system.windows.media.imaging.bitmapcreateoptions(v=VS.95).aspx) tem o valor por omissão de **DelayCreation**, que tal como indicado anteriormente, carrega a imagem apenas quando esta for realmente necessária, mas no thread principal.

A título de exemplo, sabemos que este bloco de código:

```xml
<Image Source="http://domain/image.png" />
```

É equivalente a este:

```xml
<Image>
    <Image.Source>
        <BitmapImage CreateOptions="DelayCreation" UriSource="http://domain/image.png" />
    </Image.Source>
</Image>
```

Para forçar o carregamento assíncrono de imagens, devemos colocar neste propriedade o valor **BackgroundCreation**, em que o Silverlight se encarrega de criar e manter uma cache de imagens carregadas em background.

```xml
<Image>
    <Image.Source>
        <BitmapImage CreateOptions="BackgroundCreation" UriSource="http://domain/image.png" />
    </Image.Source>
</Image>
```

O ideal será mesmo colocar o valor das duas propriedades, "DelayCreation,BackgroundCreation" , forçando assim o Silverlight para carregar as imagens apenas quando forem necessárias, mas sempre num thread separado!

```xml
<Image>
    <Image.Source>
        <BitmapImage CreateOptions="DelayCreation,BackgroundCreation" UriSource="http://domain/image.png" />
    </Image.Source>
</Image>
```
