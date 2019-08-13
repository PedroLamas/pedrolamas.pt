---
layout: post
status: publish
published: true
title: Extensões em .NET
wordpress_id: 132
wordpress_url: http://www.pedrolamas.com/?p=132
categories:
- .NET
tags:
- .NET
- Programação
- Extension Methods
---
Nos últimos tempos tenho vindo a dar bastante uso ao Microsoft Visual Studio 2008 bem como à framework .NET 3.5! Assim, nos próximos posts pretendo falar de algumas das novidades que apareceram e como elas podem facilitar a vida dos programadores!

Começo hoje esta rúbrica falando de **Extension Methods**, ou "métodos de extensão".

Estes métodos de extensão permitem aos programadores adicionar métodos aos tipos e classes da framework, sem ter que fazer uma nova classe por herança (*sub-classing* ou *inheritance*), ou mesmo recompilar a classe original. Estes métodos ficam como que "colados" ao tipo de dados referenciado, ficando disponível para toda e qualquer instância do objecto.

suponhamos que no meu código eu tinha um método que me permite verificar se uma dada cadeia de caracteres contém apenas dígitos, poderia ser algo do género:

![Static Method](wp-content/uploads/2008/04/extensionmethods01.jpg "Static Method")

Assim, bastava fazer **OnlyDigits(myString)** para saber se a string é ou não composta apenas de números! Um método de extensão equivalente seria o seguinte:

![Extension Method](wp-content/uploads/2008/04/extensionmethods02.jpg "Extension Method")

A diferença é que agora, podemos fazer algo do tipo **myString.OnlyDigits()** para obter exactamente o mesmo resultado! Na verdade, até algo tipo **"a minha string".OnlyDigits()** funcionará na perfeição.

Estendemos assim o tipo de dados *string* com o nosso método *OnlyDigits*, que fica disponível em toda e qualquer instância deste tipo de dados!

Os métodos de extensão funcionam desde a framework 3.0, mas penso que apenas o Visual Studio 2008 os suporta.

Para mais informações podem consultar o [MSDN](http://msdn2.microsoft.com/en-us/library/bb383977.aspx).
