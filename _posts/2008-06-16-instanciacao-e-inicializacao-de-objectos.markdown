---
layout: post
status: publish
published: true
title: Instanciação e Inicialização de Objectos
wordpress_id: 178
wordpress_url: http://www.pedrolamas.com/?p=178
categories:
- .NET
tags:
- .NET
- Programação
- Object Initialization
---
A .NET Framework 3.5 trouxe muitas mudanças, mas bastante significativa no que toca a escrita de código: a inicialização de objectos!

Imaginemos que pretendemos um objecto para guardar os dados de uma pessoa, e para isso usamos uma classe como a seguinte:

[![Data Class](wp-content/uploads/2008/06/classinitialization01-209x300.jpg "Data Class")](wp-content/uploads/2008/06/classinitialization01.jpg "Data Class")

Tendo a classe definida sem qualquer tipo de construtor específico para além do herdado pré-definido (sem argumentos), basta criar uma nova instância da classe e inicializar cada uma das propriedades, da seguinte forma:

[![Class Instantiation with separate Initialization](wp-content/uploads/2008/06/classinitialization02.jpg "Class Instantiation with separate Initialization")](wp-content/uploads/2008/06/classinitialization02.jpg "Class Instantiation with separate Initialization")

Mas na framework versão 3.5 podemos fazer a instanciação do objecto e a inicialização das suas propriedades num só comando, da seguinte forma:

[![Class Instantiation with integrated Initialization](wp-content/uploads/2008/06/classinitialization03.jpg "Class Instantiation with integrated Initialization")](wp-content/uploads/2008/06/classinitialization03.jpg "Class Instantiation with integrated Initialization")

Este é mais um "atalho" que a nova framework disponibilizou, de forma a facilitar a vida aos programadores! ;)
