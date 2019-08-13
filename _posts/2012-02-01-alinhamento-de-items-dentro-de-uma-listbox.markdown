---
layout: post
status: publish
published: true
title: Alinhamento de items dentro de uma ListBox
wordpress_id: 2392
wordpress_url: http://www.pedrolamas.com/?p=2392
date: 2012-02-01 20:35:30.000000000 +00:00
categories:
- Mobilidade
tags:
- Programação
- Windows Phone
- WP7Dev
- WPDev
- ListBox
- ListBoxItem
---
Ao desenvolver diariamente para Windows Phone por vezes deparo-me com situações em que nem tudo o que parece, realmente o é... esta é uma dessas situações!

Cenário inicial: numa PhoneApplicationPage vazia, adicionamos o seguinte bloco de XAML ao conteúdo da Grid ContentPanel:

```xml
<ListBox ItemsSource="{Binding Items}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock Text="{Binding}" Style="{StaticResource PhoneTextTitle2Style}" />
            </Grid>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Como podem aqui ver, estamos a criar uma [ListBox](http://msdn.microsoft.com/en-us/library/system.windows.controls.listbox(v=vs.95).aspx) com um template muito simples (para cada item, apresentar um [TextBlock](http://msdn.microsoft.com/en-us/library/system.windows.controls.textblock(v=vs.95).aspx) usando o próprio item como texto).

Do lado do código temos uma propriedade Items que tem apenas e só 3 items e que vão servir para alimentar a propriedades [ItemsSource](http://msdn.microsoft.com/en-us/library/system.windows.controls.itemscontrol.itemssource(v=vs.95).aspx) da nossa ListBox.

Até aqui tudo bem, ao executar a aplicação o resultado deverá ser este:

![](wp-content/uploads/2012/02/ListBox-with-left-aligned-items.png "ListBox with left aligned items")

Digamos que agora pretendemos alinhar o texto de cada item à direita; normalmente, a seguinte alteração seria suficiente:

[xml firstline="5"]\<TextBlock Text="{Binding}" HorizontalAlignment="Right" Style="{StaticResource PhoneTextTitle2Style}" /\>[/xml]

Ao executar novamente a aplicação, poderemos ver que o resultado se mantém inalterado!

Nesta altura, começamos a testar as propriedades da própria ListBox, ao ponto de fazer algo deste tipo:

```xml
<ListBox HorizontalContentAlignment="Right">
```

E mais uma vez, ao executar poderemos reparar que nada mudou!!!

O que se passa é que cada item da ListBox é instanciado dentro de um [ListBoxItem](http://msdn.microsoft.com/en-us/library/system.windows.controls.listboxitem(v=vs.95).aspx), e este controlo ignora por completo a propriedade [HorizontalContentAlignment](http://msdn.microsoft.com/en-us/library/system.windows.controls.control.horizontalcontentalignment(v=vs.95).aspx) colocada na própria ListBox.

Para resolver este problema, temos que definir a propriedade [ListBox.ItemContainerStyle](http://msdn.microsoft.com/en-us/library/system.windows.controls.listbox.itemcontainerstyle(v=vs.95).aspx) e ai sim colocar o [ListBoxItem.HorizontalAlignment](http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement.horizontalalignment(v=vs.95).aspx) pretendido.

O código corrigido ficará estão da seguinte forma:

[xml highlight="2,3,4,5,6"]\<ListBox ItemsSource="{Binding Items}"\> \<ListBox.ItemContainerStyle\> \<Style TargetType="ListBoxItem"\> \<Setter Property="HorizontalAlignment" Value="Right"/\> \</Style\> \</ListBox.ItemContainerStyle\> \<ListBox.ItemTemplate\> \<DataTemplate\> \<Grid\> \<TextBlock Text="{Binding}" Style="{StaticResource PhoneTextTitle2Style}" /\> \</Grid\> \</DataTemplate\> \</ListBox.ItemTemplate\> \</ListBox\>[/xml]

E aqui temos o resultado final:

![](wp-content/uploads/2012/02/ListBox-with-right-aligned-items.png "ListBox with right aligned items")

Pessoalmente, coloco sempre o [ListBoxItem.HorizontalAlignment](http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement.horizontalalignment(v=vs.95).aspx) com o valor [HorizontalAlignment.Stretch](http://msdn.microsoft.com/en-us/library/system.windows.horizontalalignment(v=vs.95).aspx), e depois controlo o alinhamento do item propriamente dito dentro do template; com esta segunda abordagem tenho a garantia de que o item dispõe realmente da largura total da ListBox para apresentar resultados!
