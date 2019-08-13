---
layout: post
status: publish
published: true
title: Character Encoding no Windows Phone
wordpress_id: 2537
wordpress_url: http://www.pedrolamas.com/?p=2537
date: 2012-04-24 11:41:07.000000000 +01:00
categories:
- Mobilidade
tags:
- Silverlight
- Programação
- Windows Phone
- WP7
- WP7Dev
- WPDev
- Encoding
---
O SDK do Windows Phone apresenta-nos de base duas classes de codificação de texto, [UTF8Encoding](http://msdn.microsoft.com/en-us/library/system.text.utf8encoding(v=vs.95).aspx) e [UnicodeEncoding](http://msdn.microsoft.com/en-us/library/system.text.unicodeencoding(v=vs.95).aspx), limitando assim as possibilidades de codificação ao UTF-8 e UTF-16 respectivamente.

Mas se é verdade que actualmente a grande parte dos recursos se encontram codificados em UTF-8 (por exemplo, as páginas do [Público](http://www.publico.pt), [Sapo](http://www.sapo.pt), e [Google](http://www.google.com)), existem ainda alguns com outras codificações, sendo a mais comum a ISO-8859-1, designada de *Western Europe Encoding* (exemplo disso são as páginas do [Diário de Notícias](http://www.dn.pt/), e a [Rádio Popular](http://www.radiopopular.pt)).

Na .NET Framework completa, podemos utilizar o seguinte comando para obter um objecto capaz de processar este formato:

```csharp
var encoding = System.Text.Encoding.GetEncoding("iso-8859-1");
```

Mas infelizmente, este processo não funciona no Silverlight (e por inerência, no Windows Phone)!

A alternativa pode passar por termos um serviço intermediário (tipo *proxy*) o qual seria invocado pela aplicação Silverlight, e que por sua vez faria um pedido à pagina com dados codificados em ISO-8859-1, e assim trataria da sua conversão para UTF-8 antes de os devolver à aplicação.

[![](wp-content/uploads/2012/04/Silverlight-Encoding-Generator.png "Silverlight Encoding Generator")](http://www.hardcodet.net/2010/03/silverlight-text-encoding-class-generator)Mas podemos resolver o problema directamente na aplicação, criando uma classe [Encoding](http://msdn.microsoft.com/en-us/library/system.text.encoding(v=vs.95).aspx) para podermos correctamente ler e processar a informação; nesse sentido, o [Silverlight Encoding Generator](http://www.hardcodet.net/2010/03/silverlight-text-encoding-class-generator) é a ferramenta a utilizar!

Esta ferramenta permite gerar uma classe de Encoding compatível com Silverlight, de forma muito ágil e rápida: basta indicar o nome da codificação ([lista completa](http://msdn.microsoft.com/en-us/library/86hf4sb8(v=vs.100).aspx) das disponíveis na .NET Framework 4.0), o namespace e nome da classe a criar e está pronto!
