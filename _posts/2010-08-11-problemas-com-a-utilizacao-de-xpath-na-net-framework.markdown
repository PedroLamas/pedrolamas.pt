---
layout: post
status: publish
published: true
title: Problemas com a utilização de XPath na .NET Framework
wordpress_id: 1353
wordpress_url: http://www.pedrolamas.com/?p=1353
date: 2010-08-11 23:37:20.000000000 +01:00
categories:
- .NET
tags:
- .NET
- Programação
- XPath
- XML
---
Este é um artigo um pouco diferente do que normalmente coloco no blog!

Recentemente tive de recorrer à utilização de XPath numa aplicação em .NET 4.0, e deparei-me com alguns problemas inesperados!

Suponhamos que temos um ficheiro chamado "teste.xml" com o seguinte conteúdo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003" DefaultTargets="Build">
  <PropertyGroup>
    <ProductVersion>9.0.30729</ProductVersion>
  </PropertyGroup>
</Project>
```

Agora, com base neste ficheiro, executam o seguinte código em .NET:

```csharp
var doc = new XmlDocument(); //Criar XmlDocument

doc.Load("teste.xml"); //Carregar o ficheiro teste.xml

var node = doc.SelectSingleNode("/Project"); //Obter o nó raiz com uma expressão de XPath
```

Ora pela lógica, neste momento a variável "node" deveria ter o nó raiz do nosso XML, certo?

ERRADO!!!

Acontece que pelo menos até à actual .NET Framework 4.0, as base classes apenas contemplam XPath 1.0, em que a noção de "default namespace" (indicado no XML com o atributo "xmlns") não existe, sendo assim obrigatória a sua indicação!

Para ultrapassar este problema, sugiro algumas soluções:

Solução 1: passar o default namespace a ser apenas mais um namespace declarado (isto sim, compatível com XPath 1.0)

[csharp highlight="5,6,7,8,9"]var doc = new XmlDocument(); //Criar XmlDocument

doc.Load("teste.xml"); //Carregar o ficheiro teste.xml

var nsm = new XmlNamespaceManager(doc.NameTable);

nsm.AddNamespace("xxx", doc.DocumentElement.NamespaceURI);

var node = doc.SelectSingleNode("/xxx:Project", nsm); //Obter o nó raiz com uma expressão de XPath[/csharp]

Solução 2: remover o "default namespace" (carinhosamente chamada de "solução-martelo")

[csharp highlight="5,6,7,8,9"]var doc = new XmlDocument(); //Criar XmlDocument

doc.Load("teste.xml"); //Carregar o ficheiro teste.xml

if (doc.DocumentElement.HasAttribute("xmlns")) { doc.DocumentElement.SetAttribute("xmlns", string.Empty); doc.Load(doc.OuterXml); }

var node = doc.SelectSingleNode("/Project"); //Obter o nó raiz com uma expressão de XPath[/csharp]

Solução 3: utilizar bibliotecas de terceiros em vez do normal System.Xml, quem implementam as mais recentes tecnologias como XPath 2.0

-   [Saxon](http://saxon.sourceforge.net)
-   [XQSharp](http://www.xqsharp.com/)
-   [QueryMachine](http://qm.codeplex.com/)

Apesar de eu ter utilizado XmlDocument, este é um comportamento que afecta também o "LINQ to XML" (XDocument e afins... :-P ) quando pretendem aplicar XPath a estes!

Bom XPath'ing a todos!
