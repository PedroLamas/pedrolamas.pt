---
layout: page
status: publish
published: true
title: TodayTime
wordpress_id: 40
wordpress_url: http://www.pedrolamas.com/projectos/todaytime/
categories:
- Geral
tags: []
---
![TodayTime Main Window](/wp-content/uploads/2007/12/todaytimemain.png)

Código Fonte
------------

Disponível no [GitHub](https://github.com/PedroLamas/TodayTime)!

Download
--------

Última Versão: [0.1.3.1](/wp-content/uploads/2007/12/todaytimecab_0131.zip "TodayTime 0.1.3.1")

Change Log
----------

**0.1.3.1** - 04/09/2006 - Bug da 'Segunda-feira' (excesso de caracteres na formatação) corrigido; aumentado o tamanho máximo da formatação de 30 para 50 caracteres.

**0.1.3.0** - 01/07/2006 - Indicação do campo "Text Size" (tamanho do tipo de letra) adicionado. - O piscar ao actualizar a imagem (flicker) foi reduzido.

**0.1.2.0** - 02/04/2006 - Possibilidade de configurar o tamanho do texto.

**0.1.1.0** - 30/03/2006 - Formatação de data e hora tanto à esquerda como à direita. - Introdução do "@" para indicar Swatch Internet Time na formatação.

**0.1.0.1** - 27/03/2006 - Possibilidade de usar o DPAD (setas!) para seleccionar e abrir as opções de relógio.

**0.1.0.0** - 26/03/2006 - Primeira release!

Bugs Conhecidos
---------------

- Problemas com a scroll vertical do Today quando o espaço ocupado por todos os plugins ultrapassa a altura disponível no ecrã. - Reportados alguns problemas de compatibilidade com o SPB Diary e as suas skins.

Acerca
------

Para quem não sabe, nas ROM's mais recentes a i-mate "lembrou-se" de remover o relógio da barra de título no Today Screen e colocar o mesmo junto com o plugin da data, duplicando o seu tamanho... Ora isso é um autêntico desperdício de espaço no Today!!! Assim resolvi criar um pequeno plugin que mostra a data, hora internet ([Swatch Internet Time](http://www.swatch.com/internettime)) e a hora actual! Ao clicar no plugin aparece o painél de controlo da Data/Hora, como já acontecia no plugin nativo da data.

O resultado é o que podem na imagem acima!

Como podem ver na imagem, o relógio no topo já era; agora aparece isso sim é um icon da bateria e até ver não existe maneira de como repor a situação...

As configurações estão acessíveis em *Start* -\> *Settings* -\> *Personal* e depois *Today* -\> *Items*, escolhem o **TodayTime**, carregam no botão *Options* e podem configurar a data seguindo a tabela abaixo.

Para a data, usem o seguinte:

-   d -\> Dia do mês (1, 2, 3, ...)
-   dd -\> Dia do mês (01, 02, 03, ...)
-   ddd -\> Abreviatura do dia da semana (Dom, Seg, Ter, ...)
-   dddd -\> Dia da semana (Domingo, Segunda, Terça, ...)
-   M -\> Número do Mês de 1 a 12 (1, 2, 3)
-   MM -\> Número do Mês de 1 a 12 (01, 02, 03, ...)
-   MMM -\> Abreviatura do Mês (Jan, Fev, Mar, ...)
-   MMMM -\> Mês (Janeiro, Fevereiro, Março, ...)
-   y -\> Último 2 dígitos do ano (99, 0, 1, ...)
-   yy -\> Últimos 2 dígitos do ano (99, 00, 01, ...)
-   yyyy -\> Ano (1999, 2000, 2001, ...)
-   gg -\> Era (creio que é ignorado no nosso calendário)

Para a hora, usem o seguinte:

-   h -\> Hora (8, 9, 10, 11, 12, 1, 2, ...)
-   hh-\> Hora (08, 09, 10, 11, 12, 01, 02, ...)
-   H -\> Hora (8, 9, 10, 11, 12, 13, 14, ...)
-   HH -\> Hora (08, 09, 10, 11, 12, 13, 14, ...)
-   m -\> Minuto (1, 2, 3, ...)
-   mm -\> Minuto (01, 02, 03, ...)
-   s -\> Segundos (1, 2, 3, ...)
-   ss -\> Segundos (01, 02, 03, ...)
-   t -\> A ou P
-   tt -\> AM ou PM
-   @ -\> Swatch Internet Time (@100)

Podem ainda usar o texto que quiserem, se o delimitarem com ' (plica). Por exemplo para uma data do tipo "Domingo, 5 de Março de 2006", devem usar o formato "dddd, d 'de' MMMM 'de' yyyy".

**Atenção:** Este programa é uma versão de teste!! Até ver foi apenas testado em emuladores (sempre com sucesso) e no meu XDA Exec, pelo que poderá ainda haver alguns bugs.

Por favor, reportem todos os bugs/problemas que encontrem e enviem qualquer sugestão que tenham a fazer! :)
