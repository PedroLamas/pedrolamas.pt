---
layout: post
status: publish
published: true
title: Cimbalino Windows Phone Toolkit v1.4
wordpress_id: 2627
wordpress_url: http://www.pedrolamas.com/?p=2627
date: 2012-07-13 15:33:05.000000000 +01:00
categories:
- Mobilidade
tags:
- Cimbalino
- Programação
- Windows Phone
- WP7Dev
- WPDev
---
[![](/wp-content/uploads/2011/11/Cimbalino-Windows-Phone-Toolkit.png "Cimbalino Windows Phone Toolkit")](http://cimbalino.org/)Já está disponível a **versão 1.4** do [**Cimbalino Windows Phone Toolkit**](/tag/cimbalino/)!

Aqui tem o resumo de alterações desta nova versão:

-   Introduzido o XML Namespace "http://cimbalino.org" para os Behaviors, Converters e TriggerActions
-   Adicionadas as classes [MD5](https://github.com/Cimbalino/Cimbalino-Phone-Toolkit/blob/master/src/Cimbalino.Phone.Toolkit/System/Security/Cryptography/MD5.cs) e [HMACMD5](https://github.com/Cimbalino/Cimbalino-Phone-Toolkit/blob/master/src/Cimbalino.Phone.Toolkit/System/Security/Cryptography/HMACMD5.cs), extremamente úteis em cenários de autenticação e segurança
-   Adicionado um conjunto de [TriggerActions](https://github.com/Cimbalino/Cimbalino-Phone-Toolkit/tree/master/src/Cimbalino.Phone.Toolkit/Actions) muito úteis, e totalmente compatíveis com o Expression Blend
-   Corrigido um problema com a [implementação](https://github.com/Cimbalino/Cimbalino-Phone-Toolkit/blob/master/src/Cimbalino.Phone.Toolkit/Services/NavigationService.cs) do INavigationService, relativa a um bug na framework (podem ler [aqui](/2012/06/21/phoneapplicationframe-cangoback-navigationservice-cangoback/) os detalhes)
-   Várias correcções e melhoramentos de performance!

O código está disponível no [sítio do costume](http://cimbalino.org), bem como os [pacotes de NuGet](http://nuget.org/packages?q=cimbalino)!
