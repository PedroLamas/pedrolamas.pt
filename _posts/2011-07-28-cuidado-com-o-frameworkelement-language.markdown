---
layout: post
status: publish
published: true
title: Cuidado com o FrameworkElement.Language!!
wordpress_id: 1947
wordpress_url: http://www.pedrolamas.com/?p=1947
date: 2011-07-28 17:14:45.000000000 +01:00
categories:
- Mobilidade
tags:
- Programação
- Windows Phone
- WP7
- WP7Dev
- Language
- Globalization
- CultureInfo
---
Ontem dei com um [post](http://forums.create.msdn.com/forums/p/88151/529864.aspx) do MVP [Paulo Morgado](http://paulomorgado.net/) nos fóruns do [App Hub](http://create.msdn.com/) com uma questão muito curiosa!

> Se todas as definições do emulador estão com "Português (Portugal)" (tanto interface como definições regionais), como é que ao utilizar um [IValueConverter](http://msdn.microsoft.com/en-us/library/system.windows.data.ivalueconverter(v=VS.95).aspx) num qualquer Binding da interface, o parâmetro "culture" apresenta uma CultureInfo de "en-US"?...

Para investigar este caso, criei uma pequena aplicação, com 2 TextBlocks para apresentar os valores do [Thread.CurrentThread.CurrentCulture](http://msdn.microsoft.com/en-us/library/system.threading.thread.currentculture(v=VS.95).aspx) e [Thread.CurrentThread.CurrentUICulture](http://msdn.microsoft.com/en-us/library/system.threading.thread.currentuiculture(v=VS.95).aspx) respectivamente, e um terceiro TextBlock que utiliza um IValueConverter que devolve o culture.Name por ele utilizado, e o resultado foi no mínimo... surpreendente!

Esta é a classe CultureDebugValueConverter que implementa o IValueConverter:

```csharp
public class CultureDebugValueConverter : IValueConverter
{
    public object Convert (object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        return culture.Name;
    }

    public object ConvertBack(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

Reparem que em vez de efectuar uma qualquer conversão, ela apenas retorna o "culture.Name"

O ContentPanel principal da MainPage.xaml está definido da seguinte forma:

```xml
<StackPanel x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
    <TextBlock Style="{StaticResource PhoneTextSubtleStyle}">CurrentCulture</TextBlock>
    <TextBlock Text="{Binding CurrentCulture}" Style="{StaticResource PhoneTextTitle2Style}" />
    <TextBlock Style="{StaticResource PhoneTextSubtleStyle}">CurrentUICulture</TextBlock>
    <TextBlock Text="{Binding CurrentUICulture}" Style="{StaticResource PhoneTextTitle2Style}" />
    <TextBlock Style="{StaticResource PhoneTextSubtleStyle}">Converter Culture</TextBlock>
    <TextBlock Text="{Binding Converter={StaticResource CultureDebugValueConverter}}" Style="{StaticResource PhoneTextTitle2Style}" />
</StackPanel>
```

Note-se que o último TextBlock apenas tem o Binding.Converter a apontar para um StaticResource do tipo CultureDebugValueConverter.

No MainPage.xaml.cs coloquei as restantes propriedades para os outros Bindings:

```csharp
public partial class MainPage : PhoneApplicationPage
{
    public MainPage()
    {
        InitializeComponent();

        this.DataContext = this;
    }

    public string CurrentCulture
    {
        get
        {
            return Thread.CurrentThread.CurrentCulture.Name;
        }
    }

    public string CurrentUICulture
    {
        get
        {
            return Thread.CurrentThread.CurrentUICulture.Name;
        }
    }
}
```

Depois, lancei o emulador, mudei todas as definições para Português, e este foi o resultado ao executar a aplicação:

[![](wp-content/uploads/2011/07/LanguageTestApp-original-version.jpg "LanguageTestApp: original version")](wp-content/uploads/2011/07/LanguageTestApp-original-version.jpg)

Ora se o sistema operativo, o CurrentCulture, e o CurrentUICulture estão todos "pt-PT", de onde vem aquele "en-US"?

A razão não me parece nada trivial, e para a perceber, temos que entender primeiro de onde vem o valor do parâmetro "culture" no [IValueConverter.Convert](http://msdn.microsoft.com/en-us/library/system.windows.data.ivalueconverter.convert.aspx); na documentação pode-se ler o seguinte:

> The culture is determined in the following order:
>
> -   The converter looks for the ConverterCulture property on the Binding object.
> -   If the ConverterCulture value is null, **the value of the Language property is used**.

Ora como eu não defini o Binding.ConverterCulture, quer isso então dizer que ele vai utilizar o [FrameworkElement.Language](http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement.language(v=vs.95).aspx), que por sua vez diz o seguinte na documentação:

> The default is an XmlLanguage object that has its IetfLanguageTag value set to the string "en-US"

**E heis que encontramos finalmente o "culpado"!!!**

A solução passa assim por corrigir a propriedade Page.Language, no XAML ou directamente no código; assim, o código corrigido é algo deste tipo:

[csharp highlight="3"]public MainPage() { Language = System.Windows.Markup.XmlLanguage.GetLanguage(Thread.CurrentThread.CurrentUICulture.Name);

InitializeComponent();

this.DataContext = this; }[/csharp]

Na terceira linha pode-se ver que estou a inicializar a propriedade Language da página actual com o XmlLanguage que posso obter utilizando por base o CurrentUICulture.Name.

Uma nota final: para ter o efeito esperado, esta correcção tem de ser efectuada **antes** de ser invocado o InitializeComponent() da própria página!

E aqui está o resultado final:

[![](wp-content/uploads/2011/07/LanguageTestApp-fixed-version.jpg "LanguageTestApp: fixed version")](wp-content/uploads/2011/07/LanguageTestApp-fixed-version.jpg)

Francamente não sei o porquê deste comportamento, mas é um factor muito importante a ter em conta quando estivermos a pensar em Globalization nas nossas aplicações Windows Phone!

Se quiserem experimentar, podem fazer o [download do código](wp-content/uploads/2011/07/PedroLamas.LanguageTestApp.zip) que utilizei neste artigo!
