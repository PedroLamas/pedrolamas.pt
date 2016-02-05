---
layout: post
status: publish
published: true
title: Compressão GZip no Windows Phone
wordpress_id: 2420
wordpress_url: http://www.pedrolamas.com/?p=2420
date: 2012-02-22 12:56:12.000000000 +00:00
categories:
- Mobilidade
tags:
- Windows Phone
- WP7Dev
- WPDev
- Performance
- GZip
---
Os servidores modernos de internet permitem comprimir os dados por eles devolvidos utilizando [GZip](http://en.wikipedia.org/wiki/Gzip), um algoritmo standard para compressão de dados.

A compressão de dados nas comunicações é especialmente importante para os dispositivos móveis, em que normalmente o acesso à internet feito através da rede móvel é limitado (e caro!)

Se os dados foram comprimidos antes de serem devolvidos ao cliente, vamos gastar menos bytes no envio, e como tal estamos a ganhar tanto em tráfego como em velocidade: os ganhos da compressão com GZip **podem ir de 50% a 80%**, de acordo com [algumas estatísticas online](http://www.jeff.wilcox.name/2012/01/windows-phone-gzip-support-by-morten/).

Apesar de grande parte dos dispositivos de acesso à internet estar preparado para receber dados comprimidos neste formato, o Windows Phone aparentemente não está, pelo menos no que toca ao [HttpWebRequest](http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.95).aspx), o objecto utilizado por base para comunicação em HTTP na .NET Framework!

Opção 1: a minha aplicação usa HttpWebRequest ou WebClient
----------------------------------------------------------

Uma solução para esta situação é utilizar o **GZipWebClient** do [Morten Nielsen](http://www.sharpgis.net/), disponível por [NuGet](http://nuget.org/packages/SharpGIS.GZipWebClient) e em [código aberto no GitHub](https://github.com/dotMorten/SharpGIS.GZipWebClient).

Depois de juntarem o componente ao vosso projecto, juntem estas duas linhas no início da aplicação:

```csharp
WebRequest.RegisterPrefix("http://", SharpGIS.WebRequestCreator.GZip);
WebRequest.RegisterPrefix("https://", SharpGIS.WebRequestCreator.GZip);
```

Feito isto, sempre que criarem uma nova instância do HttpWebRequest ou do WebClient, este estará pronto para funcionar com GZip!

Opção 2: A minha aplicação usa RestSharp
----------------------------------------

Bem, no caso do [RestSharp](http://restsharp.org/), desde a versão 102.6 que tudo o que tem a fazer é, antes de executarem o vosso pedido, juntar o Http Header que indica que aceitam dados comprimidos com GZip na resposta!

[csharp highlight="3"]var client = new RestClient("http://some.url/service");

client.AddDefaultHeader("Accept-Encoding", "gzip");[/csharp]

Como podem aqui ver, o truque está na ultima linha, em que damos indicação ao servidor que o cliente vai aceitar dados comprimidos com GZip.

O RestSharp ao receber a resposta vai validar o formato dos dados, e se estes foram comprimidos encarregar-se-á de os descomprimir!

Simples e eficáz! :)

**Nota:** Dado que a alteração para suporte de GZip no RestSharp foi incluída por mim, agradeço que se detectarem algum problema me façam chegar essa informação! ;)
