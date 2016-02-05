---
layout: post
status: publish
published: true
title: Como detectar equipamentos Windows Phone "low cost"
wordpress_id: 2511
wordpress_url: http://www.pedrolamas.com/?p=2511
date: 2012-04-04 17:44:55.000000000 +01:00
categories:
- Mobilidade
tags:
- Cimbalino
- Windows Phone
- WP7
- WP7Dev
- WPDev
- Tango
---
Dentro em breve vai ser possível adquirir equipamentos Windows Phone "low cost" (como por exemplo o [Nokia Lumia 610](http://europe.nokia.com/find-products/devices/nokia-lumia-610)), os quais, entre outras coisas, vão ter menos memória (inferior a 256MB)!

Cabe aos programadores prepararem as suas aplicações para que estas funcionem nesta nova classe de equipamentos, e para tal, a equipa de desenvolvimento do Windows Phone já publicou um [artigo de boas-práticas](http://windowsteamblog.com/windows_phone/b/wpdev/archive/2012/03/07/optimizing-apps-for-lower-cost-devices.aspx) sobre que optimizações e cuidados devem ter!

No mesmo sentido, junto agora o meu pequeno contributo, que é uma forma muito básica de conseguir saber se a aplicação está a ser executada num equipamento "low cost" ou não:

```csharp
public bool IsLowMemoryDevice
{
    get
    {
        return Microsoft.Phone.Info.DeviceStatus.DeviceTotalMemory <= 268435456;
    }
}
```

Esta função muito simples nada mais faz do que obter o total de memória do dispositivo e saber se é inferior a 256MB; se o for, trata-se então de um equipamento "low cost"!

O [Cimbalino Windows Phone Toolkit](http://cimbalino.org) já tem esta funcionalidade disponível na implementação do método **IDeviceStatusService.IsLowMemoryDevice**, bastando assim registar o serviço **DeviceStatusService** no vosso controlador de IOC para a ela poderem aceder! ;)
