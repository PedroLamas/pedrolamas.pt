---
layout: post
status: publish
published: true
title: Alterações do InputScope no Windows Phone "Mango"
wordpress_id: 1963
wordpress_url: http://www.pedrolamas.com/?p=1963
date: 2011-08-01 14:39:32.000000000 +01:00
categories:
- Mobilidade
tags:
- Windows Phone
- WP7
- WP7Dev
- WPDev
- InputScope
---
Uma das funcionalidades mais interessantes no Windows Phone quanto à introdução de dados é a possibilidade de contextualizar o SIP (teclado virtual) de uma qualquer caixa de texto de acordo com os dados que esta vai receber), através da propriedade [TextBox.InputScope](http://msdn.microsoft.com/en-us/library/system.windows.controls.textbox.inputscope(v=vs.95).aspx).

Quer isto dizer que, por exemplo, se uma dada caixa de texto vai apenas conter números, nada melhor que que colocar um teclado adequado, utilizando o [InputScopeNameValue.Number](http://msdn.microsoft.com/en-us/library/system.windows.input.inputscopenamevalue(v=VS.95).aspx), o que resulta no seguinte teclado contextual:

[![](/wp-content/uploads/2011/08/Windows-Phone-InputScope-Number-pré-Mango-Thumb.jpg "Windows Phone InputScope: Number (pré Mango)")](/wp-content/uploads/2011/08/Windows-Phone-InputScope-Number-pré-Mango.jpg)

O teclado numérico que podemos ver em cima era considerado pouco prático por apresentar um conjunto de símbolos completamente inúteis para quem apenas pretendia introduzir um número, facto que levou os programadores muitas vezes a optarem pelo InputScopeNameValue.TelephoneNumber não só para números de telefone, mas também para número regulares:

[![](/wp-content/uploads/2011/08/Windows-Phone-InputScope-TelephoneNumber-Thumb.jpg "Windows Phone InputScope: TelephoneNumber")](/wp-content/uploads/2011/08/Windows-Phone-InputScope-TelephoneNumber.jpg)

A nova versão do Windows Phone "Mango" vem agora "corrigir" esta lacuna, modificando o teclado Number para algo muito mais adequado:

[![](/wp-content/uploads/2011/08/Windows-Phone-InputScope-Number-post-Mango-Thumb.jpg "Windows Phone InputScope: Number (post Mango)")](/wp-content/uploads/2011/08/Windows-Phone-InputScope-Number-post-Mango.jpg)
