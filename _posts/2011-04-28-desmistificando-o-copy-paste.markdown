---
layout: post
status: publish
published: true
title: Desmistificando o "Copy & Paste"
wordpress_id: 1810
wordpress_url: http://www.pedrolamas.com/?p=1810
date: 2011-04-28 13:13:13.000000000 +01:00
categories:
- Mobilidade
tags:
- Programação
- Windows Phone
- WP7
- WP7Dev
- Windows Phone 7
- NoDo
- Copy&amp;Paste
---
A última versão do Windows Phone 7 (com nome de código "NoDo") apresentou uma nova funcionalidade largamente pedida por todos: capacidade para copiar e colar texto (copy & paste)!

A sua utilização é muito simples: em qualquer caixa de texto (e no Internet Explorer), basta carregar numa palavra para que apareça de imediato os selectores de início/fim de selecção, modificar a selecção arrastando os selectores conforme o pretendido, e carregar no pequeno icon de cópia que aparece junto ao texto.

Depois é ir à caixa de texto onde pretendemos colar o texto, e carregar no botão que aparece logo acima do teclado (uma dica: podem fazer multiplas colagens simplesmente deslizando o dedo na área do botão de "colar" para que ele torne a aparecer).

[![](/wp-content/uploads/2011/04/Funcionalidade-Copy-Paste-no-Windows-Phone-7-thumb.jpg)](/wp-content/uploads/2011/04/Funcionalidade-Copy-Paste-no-Windows-Phone-7.jpg "Funcionalidade Copy & Paste no Windows Phone 7")

Seguindo a documentação disponibilizada pela Microsoft, quem desenvolve aplicações para o Windows Phone 7 tem apenas que colocar na interface controlos TextBox para que automaticamente o sistema operativo disponibilize esta funcionalidade nesses controlos!

Pelo menos para já, não é possível programaticamente aceder à funcionalidade de copy & paste, logo esta apenas pode ser invocada explicitamente por acção do próprio utilizador.

Mas aqui surge uma questão que tenho visto ser feita vezes sem conta: como fazer copy & paste em controlos TextBlock? Bem, na verdade, tal não é possível, mas podemos no limite fazer com que uma TextBox seja apresentada e se comporte como um TextBlock!!

Para isso, podem usar o seguinte Style nas vossas aplicações:

```xml
<Style x:Key="ReadOnlyTextBox" TargetType="TextBox">
    <Setter Property="Foreground" Value="{StaticResource PhoneForegroundBrush}" />
    <Setter Property="IsReadOnly" Value="True" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate>
                <Grid>
                    <ContentPresenter x:Name="ContentElement" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Assim, o passo seguinte será substituir os controlos TextBlock por TextBox e aplicar o estilo ReadOnlyTextBox para que estas passem visualmente a ser como os TextBlock, mas com a funcionalidade de copy & paste sempre presente! :)
