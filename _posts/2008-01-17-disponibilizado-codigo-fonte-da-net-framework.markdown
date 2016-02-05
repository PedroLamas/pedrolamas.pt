---
layout: post
status: publish
published: true
title: Disponibilizado Código-Fonte da .NET Framework
wordpress_id: 65
wordpress_url: http://www.pedrolamas.com/2008/01/17/disponibilizado-codigo-fonte-da-net-framework/
categories:
- .NET
tags:
- .NET
- ASP.NET
- Código Fonte
- Visual Studio
---
Está disponível na internet o código-fonte da Microsoft .NET Framework, permitindo o seu depuramento directo através do Visual Studio 2008.

Antes deste passo, o código-fonte poderia ser facilmente consultado utilizando o excelente utilitário [.NET Reflector](http://www.aisto.com/roeder/dotnet/) do [Lutz Roeder](http://www.aisto.com/roeder/) para uma simples descompilação, mas o facto de ter acesso ao código real permite o visionamento de comentários de desenvolvimento introduzido no próprio código, já para não falar que serve até de um manual em "boas práticas" para programação.

Especificamente, passa agora a ser possível a consulta e depuração das seguintes bibliotecas do .NET Framework:

-   .NET Base Class Libraries (including System, System.CodeDom, System.Collections, System.ComponentModel, System.Diagnostics, System.Drawing, System.Globalization, System.IO, System.Net, System.Reflection, System.Runtime, System.Security, System.Text, System.Threading, etc).
-   ASP.NET (System.Web, System.Web.Extensions)
-   Windows Forms (System.Windows.Forms)
-   Windows Presentation Foundation (System.Windows)
-   ADO.NET and XML (System.Data and System.Xml)

Mais informações sobre como utilizar este recurso podem ser obtidas [neste](http://blogs.msdn.com/sburke/archive/2008/01/16/configuring-visual-studio-to-debug-net-framework-source-code.aspx) excelente artigo no blog de [Shawn Burke](http://blogs.msdn.com/sburke).
