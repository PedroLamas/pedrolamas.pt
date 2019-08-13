---
layout: post
status: publish
published: true
title: ListPicker a... cafeína!!!
wordpress_id: 1849
wordpress_url: http://www.pedrolamas.com/?p=1849
date: 2011-06-02 01:05:40.000000000 +01:00
categories:
- Mobilidade
tags:
- Silverlight
- Programação
- Windows Phone
- WP7Dev
- Windows Phone 7
- Silverlight Toolkit
- ListPicker
---
**Actualizado a 18/08/2011:** este artigo deixa neste momento de ser válido, dado que a Microsoft já disponibilizou a [actualização de Agosto de 2011 para o Silverlight for Windows Phone Toolkit](2011/08/18/silverlight-for-windows-phone-toolkit-aug-2011/), o qual resolve a situação aqui descrita! ;)

De todos os controlos que a equipa do Silverlight lançou no Toolkit para Windows Phone 7, o que utilizo com mais regularidade é mesmo o ListPicker!

No entanto, desde que ele apareceu no Toolkit que sempre me senti frustrado com a fraca performance de scrolling deste, quando aberto no modo FullMode!

A razão para essa fraca performance é devida em grande parte ao seguinte bloco de código do ficheiro "\\WindowsPhone7\\Microsoft.Phone.Controls.Toolkit\\Themes\\Generic.xaml":

[code language="xml" firstline="566" highlight="572,573,574,575,576"] \<ListBox x:Name="FullModeSelector" Grid.Row="1" ItemTemplate="{TemplateBinding ActualFullModeItemTemplate}" FontSize="{TemplateBinding FontSize}" Margin="{StaticResource PhoneMargin}"\> \<ListBox.ItemsPanel\> \<ItemsPanelTemplate\> \<StackPanel/\> \<!-- Ensures all containers will be available during the Loaded event --\> \</ItemsPanelTemplate\> \</ListBox.ItemsPanel\> \</ListBox\> [/code]

Como podemos aqui ver, na propriedade ListBox.ItemContainer foi colocado um StackPanel, substituindo assim o VirtualizingStackPanel que seria utilizado por omissão!

Esta alteração tem como origem um "hack" que foi introduzido para simular o highlight do item seleccionado na lista, mas mantendo o ListBox.SelectedItem = null.

Mas dada a visível perda de performance, o "hack" torna-se um grande problema, e como tal, vamos ter que o remover!!!

A primeira coisa a fazer é mesmo remover toda a alteração da propriedade ListBox.ItemsPanel que vemos acima, de forma a que este volte a utilizar o VirtualizingStackPanel; para além disso, alterei o modo de selecção da ListBox para que o evento ListBox.SelectionChanged ocorra sempre permitindo seleccionar e desseleccionar items:

[code language="xml" firstline="566" highlight="572"] \<ListBox x:Name="FullModeSelector" Grid.Row="1" ItemTemplate="{TemplateBinding ActualFullModeItemTemplate}" FontSize="{TemplateBinding FontSize}" Margin="{StaticResource PhoneMargin}" SelectionMode="Multiple"\> \</ListBox\> [/code]

O passo seguinte é remover o "hack" que podemos encontrar no seguinte bloco de código do ficheiro "\\WindowsPhone7\\Microsoft.Phone.Controls.Toolkit\\ListPicker\\ListPicker.cs":

[code language="csharp" firstline="715" highlight="720,721,722,723,724,725,726,727,728,729,730,731,732,733,734,735"] if (null != \_fullModeSelectorPart) { // Find the relevant container and make it look selected // Note: Selector.SelectedItem is left null so \*any\* selection will trigger the SelectionChanged event. // However, this doesn't highlight the "currently selected" item; the following technique fakes that. ContentControl container = \_fullModeSelectorPart.ItemContainerGenerator.ContainerFromItem(SelectedItem) as ContentControl; if (null == container) { // Container isn't always available; defer until it is // Note: Assumes the container eventually WILL be available (which is why // the default Template replaces VirtualizingStackPanel with StackPanel) Dispatcher.BeginInvoke(() =\> HandleFullModeSelectorPartLoaded(sender, e)); } else { Brush phoneAccentBrush = Application.Current.Resources["PhoneAccentBrush"] as Brush; if (null != phoneAccentBrush) { container.Foreground = phoneAccentBrush; } } // Scroll item into view if possible ListBox listBox = \_fullModeSelectorPart as ListBox; if (null != listBox) { listBox.ScrollIntoView(SelectedItem); } } [/code]

As linhas marcadas em cima deverão ser todas removidas, mas temos ainda que recolocar o funcionamento do ListBox.SelectedItem, e para isso vamos começar a juntar código ao ficheiro, mais propriamente a linha indicada:

[code language="csharp" firstline="197" highlight="200"] if (null != \_fullModeSelectorPart) { \_fullModeSelectorPart.ItemsSource = Items; \_fullModeSelectorPart.SelectedItem = SelectedItem; \_fullModeSelectorPart.SelectionChanged += HandleFullModeSelectorPartSelectionChanged; \_fullModeSelectorPart.Loaded += HandleFullModeSelectorPartLoaded; } [/code]

Falta apenas alterar o tratamento do evento ListBox.SelectionChanged para processar em modo MultiSelect:

[code language="csharp" firstline="699" highlight="701,703,704,705,706,709,711"] if (null != \_fullModeSelectorPart) { object selectedItem = SelectedItem;

if (e.AddedItems.Count \> 0) selectedItem = e.AddedItems[0]; else if (e.RemovedItems.Count \> 0) selectedItem = e.RemovedItems[0];

// Commit selected item if (SelectedItem != selectedItem) { SelectedItem = selectedItem; } else { // User selected the already-selected item; just switch back to Normal view ListPickerMode = ListPickerMode.Normal; } } [/code]

E está pronto: passamos a ter um ListPicker devidamente optimizado com o VirtualizingStackPanel!

Para facilitar, os passos acima estão disponíveis [neste patch](wp-content/uploads/2011/06/ListPicker-with-VirtualizedStackPanel.zip) que eu criei especificamente para quem utiliza o TortoiseSVN para obter o código do Silverlight Toolkit directamente do Codeplex!
