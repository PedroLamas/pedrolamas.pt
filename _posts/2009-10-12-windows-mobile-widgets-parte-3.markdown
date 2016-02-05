---
layout: post
status: publish
published: true
title: Windows Mobile Widgets - Parte 3
wordpress_id: 968
wordpress_url: http://www.pedrolamas.com/?p=968
categories:
- Widgets
tags:
- Mobilidade
- Programação
- Windows Mobile
- Windows Mobile 6.5
- Windows Mobile Widgets
---
Continuando a "saga" do desenvolvimento de [Windows Mobile Widgets](/tag/windows-mobile-widgets/), este terceiro artigo vai focar o manifesto dos Widgets, mostrando para que serve e como o podemos utilizar para nosso próprio proveito.

O manifesto de um Widget para Windows Mobile tem obrigatoriamente o nome de "***config.xml***" e obedece ao standard definido pelo [draft para desenho de Widgets da W3C, de 22 Dezembro de 2008](http://www.w3.org/TR/2008/WD-widgets-20081222/). Este standard é um rascunho (draft) do que é pretendido para a versão final; no entanto, no seu aspecto base é garantida pelo Windows Mobile a retro-compatibilidade das próximas versões com as mais antigas.

Este manifesto, tal como o nome do ficheiro indica, não é nada mais do que texto XML que descreve "quem" o nosso Widget realmente é!

Assim, e pegando novamente no exemplo Hello World que venho a apresentar desde o primeiro artigo, o manifesto completo para este Widget poderia ser algo deste tipo:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<widget xmlns="http://www.w3.org/ns/widgets" id="/WindowsMobileWidgets/HelloWorld" version="1.0">
<name>Hello World</name>
<content src="default.htm" type="text/html" />
<access network="false" />
<icon src="icon.png" />
<icon src="icon.ico" />
<description>
Hello World Demo Application!
</description>
<author href="http://www.pedrolamas.pt" email="pedrolamas@gmail.com">
Pedro Lamas
</author>
<license>
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
</license>
</widget>
```

Começando pela raiz do documento XML, vemos que dentro do elemento **widget** podemos encontrar os seguintes atributos:

-   **id** - Este é o identificador único para este Widget, e permite que em futuras instalações deste mesmo Widget (com o mesmo id), sejam comparadas as versões, permitindo assim  que este seja actualizado. O seu conteúdo é obrigatoriamente um URI válido
-   **version** - A versão do Widget

Passamos agora aos elementos internos:

-   **name** - O nome do Widget, que entre outras coisas será utilizado ao criar um atalho no Start
-   **content** - Identifica o conteúdo do Widget e tem os seguintes atributos:
    -   **src** - O ficheiro de arranque do Widget
    -   **type** - O tipo de conteúdo MIME do ficheiro de arranque

-   **access** - Os acessos que este Widget irá necessitar, apresentando os seguintes atributos:
    -   **network** - Indica que o Widget precisa de acesso à internet; em caso afirmativo, será apresentado um aviso ao utilizador para que este dê efectivamente o acesso

-   **icon** - Identifica um icon a ser apresentado no Widget, com estes atributos:
    -   **src** - O ficheiro de imagem do icon propriamente dito

-   **description** - A descrição do Widget, apresentada por exemplo quando este é instalado
-   **author** - O nome do autor do Widget, bem como os seguintes atributos:
    -   **href** - Um endereço de internet, normalmente representando a página de suporte do Widget ou do seu autor
    -   **email** - Um endereço de email, sendo normalmente o email de suporte do Widget ou do seu autor

-   **license** - A licença associada ao Widget

De todo o manifesto, acho pessoalmente que o identificador (id) e versão (version) são os mais importantes e essenciais, já que é com base nestes dois atributos que o Windows Mobile nos permite efectuar actualizações aos nossos Widgets; assim, aconselho a que coloquem sempre um identificador bastante completo, de forma a garantir que este é verdadeiramente único.

Todo o manifesto do Widget pode ser acedido programaticamente através do modelo de objectos (Object Model) que os Widgets expõe para o Javascript, mas esse será o tema do próximo artigo... ;)
