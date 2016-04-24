---
layout: post
status: publish
published: true
title: Aceder a ficheiros enviados no XAP de uma app de Windows Phone
wordpress_id: 2520
wordpress_url: http://www.pedrolamas.com/?p=2520
date: 2012-04-11 19:24:05.000000000 +01:00
categories:
- Mobilidade
tags:
- Programação
- Windows Phone
- WP7
- WP7Dev
- WPDev
---
É comum criarmos ficheiros de Resources (.resx) na nossa aplicação, normalmente utilizados para globalizar a mesma com vários idiomas.

Mas também é possível enviar ficheiros binários por inteiro e aceder a eles em runtime!

Note-se que estes ficheiros vão dentro do ficheiro .xap da aplicação (que nada mais é que um simples ficheiro ZIP com extensão modificada) e quando esta é instalada, são colocados na mesma pasta que os binários e não no Isolated Storage (e como tal, são apenas de leitura)

let's code
----------

Vamos criar uma aplicação muito simples, apenas com uma caixa de texto onde colocaremos o conteúdo de um ficheiro .txt que pretendemos distribuir.

Começamos por adicionar um ficheiro de texto ao projecto (para este exemplo será chamado de "test.txt"), certificando que nas propriedades do mesmo, o **Build Action** do ficheiro está em **Content**, garantindo assim que o ficheiro irá junto com a aplicação.

De seguinte, abrimos o ficheiro **MainPage.xaml** e adicionamos o seguinte código no interior do elemento **ContentPanel**:

```xml
<TextBox x:Name="TestFileContentTextBox" />
```

Passando para o ficheiro **MainPage.xaml.cs**, adicionamos o seguinte Using no topo do mesmo:

```csharp
using System.IO;
```

Falta apenas substituir o construtor pelo seguinte código:

[csharp highlight="5,7"]public MainPage() { InitializeComponent();

var streamResourceInfo = App.GetResourceStream(new Uri("test.txt", UriKind.Relative));

using (var stream = streamResourceInfo.Stream) { using (var streamReader = new StreamReader(stream)) { TestFileContentTextBox.Text = streamReader.ReadToEnd(); } } }[/csharp]

No bloco de código em cima é fácil de perceber onde está toda a magia: na linha 5 utilizamos o método [Application.GetResourceStream](http://msdn.microsoft.com/en-us/library/ms596994(v=vs.95).aspx) para obter uma referência ao ficheiro pretendido e que foi distribuído com a aplicação; na linha abaixo vemos a ser feito o acesso à stream read-only do ficheiro propriamente dito.

E [aqui](/wp-content/uploads/downloads/2012/04/GetFileFromXap.zip) tem o código fonte deste exemplo!
