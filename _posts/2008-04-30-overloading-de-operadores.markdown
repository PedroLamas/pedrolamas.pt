---
layout: post
status: publish
published: true
title: ! '"Overloading" de Operadores'
wordpress_id: 141
wordpress_url: http://www.pedrolamas.com/?p=141
categories:
- .NET
tags:
- .NET
- Programação
- Operator Overloading
---
Na continuação dos artigos que venho a publicar sobre algumas características que mais me atraem no .NET Framework, apresento hoje o **"Overload" de Operadores**!

Suponhamos que para um dado projecto, temos que registar para uma equipa o total de golos marcados e golos sofridos no decurso de um conjunto de jogos. Para tal, podemos usar uma classe como a seguinte para representar os dados:

[![Data Class](/wp-content/uploads/2008/04/operatoroverloading01-300x280.jpg "operatoroverloading01")](/wp-content/uploads/2008/04/operatoroverloading01.jpg "Data Class")

Assim, basta então criar uma instância da nossa classe para guardar os resultados e outras tantas para os jogos, e depois somar tudo!

Para efeitos de demonstração, suponhamos que pretendemos registar o resultado de 3 jogos e depois apresentar o resultado final:

[![Main Program without Operator Overloading](/wp-content/uploads/2008/04/operatoroverloading02-300x198.jpg "operatoroverloading02")](/wp-content/uploads/2008/04/operatoroverloading02.jpg "Main Program without Operator Overloading")

Neste caso, vamos precisamos de somar os valores das propriedades separadamente à nossa instância "total", como podemos ver dentro do ciclo *For*; mas podemos evitar este passo, simplesmente indicando ao compilador como é que ele pode somar instâncias da nossa classe!

Introduzimos assim na classe de dados um "overload" ao operador de soma:

[![Data Class with Operator Overloading](/wp-content/uploads/2008/04/operatoroverloading03-300x274.jpg "operatoroverloading03")](/wp-content/uploads/2008/04/operatoroverloading03.jpg "Data Class with Operator Overloading")

De notar que este "overload" é um método estático (*Static*) e tem a indicação do tipo de dados e do operador a implementar (*operator +*).

Finalmente, temos apenas de actualizar o nosso código para passar a usar o operador agora criado:

[![Main Program using Operator Overloading](/wp-content/uploads/2008/04/operatoroverloading04-300x190.jpg "operatoroverloading04")](/wp-content/uploads/2008/04/operatoroverloading04.jpg "Main Program using Operator Overloading")

O resultado final é exactamente o mesmo, mas em termos práticos mostra-se uma solução muito mais agradável de implementar do que simplesmente somar as propriedades isoladas!

Existem algumas [regras](http://msdn.microsoft.com/en-us/library/2sk3x8a7(VS.71).aspx) que devem saber antes de se lançaram a criar operadores para tudo o que é classe que usam, pelo que a sua leitura é aconselhada! ;)
