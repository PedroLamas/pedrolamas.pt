---
layout: post
status: publish
published: true
title: Gerar o link de uma aplicação Windows Phone no Marketplace... de dentro da
  aplicação!
wordpress_id: 2561
wordpress_url: http://www.pedrolamas.com/?p=2561
date: 2012-05-16 15:03:41.000000000 +01:00
categories:
- Mobilidade
tags:
- Cimbalino
- Windows Phone
- WP7
- WP7Dev
- WPDev
- WMAppManifest
---
Depois de submetida ao Marketplace, cada aplicação passa a ter um link directo personalizado para a página de instalação, facilitando assim a partilha da mesma!

A título de exemplo, o link de instalação da minha aplicação [Totojogos](2012/03/01/totojogos/) é [http://windowsphone.com/s?appid=bba5b571-13d3-49e9-841e-0e2bf0009fd3](http://windowsphone.com/s?appid=bba5b571-13d3-49e9-841e-0e2bf0009fd3)

Este é o link aparece no AppHub, na página de detalhes ("details") da aplicação, com a designação de "deep link" conforme mostra a imagem:

![](wp-content/uploads/2012/05/Deep-Link-in-AppHub-application-page.png "Deep Link in AppHub application page")

Estes links tem uma parte fixa e uma variável, que é basicamente o identificador da aplicação; podemos daqui facilmente perceber que o link tem o seguinte formato:

**http://windowsphone.com/s?appid=\<ApplicationId\>**

O ApplicationId é atribuído à aplicação no momento em que é feita a sua primeira submissão (na verdade, ele aparece pela primeira vez no endereço da página do passo 2 da submissão, logo a seguir ao carregamento do ficheiro .xap), e é colocado no atributo "ProductID" do elemento "App" no ficheiro do manifesto da aplicação (WMAppManifest.xml).

Para criar o link dinamicamente basta então ler o ficheiro de manifesto, obter o valor do atributo "ProductID", e criar então o link de acordo com o formato esperado; com estes dados podemos chegar a uma classe deste tipo:

```csharp
public class DeepLinkHelper
{
    private const string AppManifestName = "WMAppManifest.xml";
    private const string AppNodeName = "App";
    private const string AppProductIDAttributeName = "ProductID";

    public static string BuildApplicationDeepLink()
    {
        var applicationId = Guid.Parse(GetManifestAttributeValue(AppProductIDAttributeName));

        return BuildApplicationDeepLink(applicationId.ToString());
    }

    public static string BuildApplicationDeepLink(string applicationId)
    {
        return @"http://windowsphone.com/s?appid=" + applicationId;
    }

    public static string GetManifestAttributeValue(string attributeName)
    {
        var xmlReaderSettings = new XmlReaderSettings
        {
            XmlResolver = new XmlXapResolver()
        };

        using (var xmlReader = XmlReader.Create(AppManifestName, xmlReaderSettings))
        {
            xmlReader.ReadToDescendant(AppNodeName);

            if (!xmlReader.IsStartElement())
            {
                throw new FormatException(AppManifestName + " is missing " + AppNodeName);
            }

            return xmlReader.GetAttribute(attributeName);
        }
    }
}
```

A parte mais importante nesta classe é a função **GetManifestAttributeValue(string)**, que permite aceder ao ficheiro WMAppManifest.xml e de lá ler a informação pretendida.

Obtido o ApplicationId, este é depois utilizado juntamente com a função **BuildApplicationDeepLink(string)** para retornar o link final!

Tendo esta class na nossa aplicação, basta então invocar a função **DeepLinkHelper.BuildApplicationDeepLink()** para obter o deep link da mesma e com ele fazer o que quisermos (enviar por e-mail, partilhar nas redes sociais, etc.)!

Para facilitar o acesso ao manifesto da aplicação mantendo uma arquitectura MVVM, o [Cimbalino Windows Phone Toolkit](http://cimbalino.org) na versão 1.3 já tem o **IApplicationManifestService** com propriedades de acesso directo, sendo uma delas o ProductID utilizado acima! ;)

[download id="9" format="2"]
