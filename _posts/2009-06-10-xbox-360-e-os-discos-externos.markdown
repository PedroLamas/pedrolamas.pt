---
layout: post
status: publish
published: true
title: Xbox 360 e os discos externos
wordpress_id: 767
wordpress_url: http://www.pedrolamas.com/?p=767
categories:
- Xbox 360
tags:
- Microsoft
- Xbox 360
- NTFS
---
Para além da normal utilização da Xbox 360 como consola de jogos(!), esta tem ainda funcionalidades que facilitam a utilização como centro multimédia, permitindo ver fotos, ouvir música e ver filmes, tudo na mais alta qualidade e fidelidade!

Uma das formas de o fazer é aproveitar o suporte da Xbox 360 para discos externos, ligando um numa das suas portas USB disponíveis; depois é só navegar pelas opções da consola para consultar os dados do dispositivo externo!

Até aqui parece tudo muito fácil, mas heis que ao ligar o meu próprio disco portátil (um simples disco 2.5" externo, de 160GB) para ver alguns filmes e fotos que lá tenho... ele não funciona!!! Depois de várias tentativas e de "googlar" um pouco, descubro que **a Microsoft não disponibilizou suporte na XBox 360 para discos formatados com [NTFS](http://en.wikipedia.org/wiki/NTFS)**!

E o que significa isto? Significa que se quiserem utilizar discos externos na vossa Xbox 360 vão ter que os formatar no já ultrapassado [FAT32](http://en.wikipedia.org/wiki/FAT32), o que impõe algumas restrições muito complicadas no que toca aos conteúdos, nomeadamente no limite do tamanho dos ficheiros para 4GB.

Sabendo que os filmes de alta-definição (a 1080p ou mesmo 720p) ocupam bem mais que isso, é aqui que está o grande problema... mais uma pesquisa pela internet levou-me ao [EncodeHD](http://dcunningham.net/encodehd/), um software muito simples que permite recodificar os filmes para MP4, formato que a Xbox consegue ler, e ainda "cortar" os filmes em vários ficheiros de 4GB cada, de forma a os poder colocar em unidades formatadas com FAT32! Para já, parece ser esta a "solução"...

Mas tendo em conta que o NTFS é actualmente o formato recomendado pela Microsoft, como explicam eles esta "falha"? Sinceramente, espero que num futuro próximo a consola venha a sofrer algum tipo de actualização que lhe dê o tão necessário suporte para discos formatados em NTFS... :(
