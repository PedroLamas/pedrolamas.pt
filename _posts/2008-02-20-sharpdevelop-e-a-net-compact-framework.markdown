---
layout: post
status: publish
published: true
title: SharpDevelop e a .NET Compact Framework
wordpress_id: 99
wordpress_url: http://www.pedrolamas.com/2008/02/20/sharpdevelop-e-a-net-compact-framework/
categories:
- .NET
tags:
- .NET
- .NET Compact Framework
- SharpDevelop
---
Há algum tempo atrás, publiquei no [PocketPT.net](http://www.pocketpt.net) um [artigo](http://www.pocketpt.net/forum/index.php?showtopic=16851) sobre a utilização do excelente IDE para .NET, [SharpDevelop](http://www.sharpdevelop.net/OpenSource/SD/Default.aspx), com a .NET Compact Framework!

Resolvi agora recuperar esse artigo e o recolocar aqui no blog para todos!

### SharpDevelop

[![SharpDevelop - Splash Screen](wp-content/uploads/2008/02/sd_splash_screen.thumbnail.jpg)](wp-content/uploads/2008/02/sd_splash_screen.jpg "SharpDevelop - Splash Screen")

Este pequeno texto tem por intenção apresentar o SharpDevelop, um IDE de .NET de código aberto sob licença GPL.

De notar que para além do SharpDevelop, para criarem programas para a .NET Compact Framework vão ainda precisar dos seguintes SDK's:

-   [Microsoft .NET Framework 2.0 SDK (x86)](http://www.microsoft.com/downloads/details.aspx?familyid=FE6F2099-B7B4-4F47-A244-C96D69C35DEC&displaylang=en)
-   [.NET Compact Framework 2.0 Service Pack 2 Redistributable](http://www.microsoft.com/downloads/details.aspx?familyid=AEA55F2F-07B5-4A8C-8A44-B4E1B196D5C0&displaylang=en)

### Instalação

A instalação é relativamente linear, sem grandes opções, apenas chamo a atenção para a segunda parte, onde fala das associações de ficheiros (convém ter isto em atenção para o caso de já termos estes tipos de ficheiros associados a outro tipo de programa!) [![SharpDevelop - Install](wp-content/uploads/2008/02/sd_install.thumbnail.jpg)](wp-content/uploads/2008/02/sd_install.jpg "SharpDevelop - Install")

### O IDE

Feita a instalação, entramos no programa e deparamos-nos com o seguinte ecrã: [![SharpDevelop - IDE](wp-content/uploads/2008/02/sd_ide.thumbnail.jpg)](wp-content/uploads/2008/02/sd_ide.jpg "SharpDevelop - IDE")

Como podemos ver, temos os menus normalmente esperados num ambiente de desenvolvimento tradicional (File, Edit, View, Build, Debug, Tools, Window, Help)! Do lado esquerdo encontramos a Barra de Explorador de Projecto (Projects) e a Barra de Ferramentas e Controlos (Tools); do lado direito a Barra do Explorador de Classes (Classes) e a Barra de Propriedades (Properties). Ao centro ficam as restantes janelas do editor, como o Explorador de Internet Interno, e os Editores de Código e Formulários. Na base da janela encontram-se recolhidas as Janelas de Erros (Errors), Saída (Output), Lista de Tarefas (Task List) e Vista de Definições (Definitions View)

### Projecto de Teste

Este projecto de teste reflecte o clássico modelo "Hello World" da programação, adaptado para o .NET Compact Framework!

A primeira coisa a fazer é criar um novo projecto, fazendo **File** -\> **New** -\> **Solution**; no nosso exemplo vamos criar uma solução em VB.NET chamada "teste", com base no .NET Compact Framework: [![SharpDevelop - New Project](wp-content/uploads/2008/02/sd_new_project.thumbnail.jpg)](wp-content/uploads/2008/02/sd_new_project.jpg "SharpDevelop - New Project")

Podemos ver que o IDE automaticamente criou um formulário vazio chamado "MainForm", constituido pelos ficheiros "*MainForm.vb*" e "*MainForm.Design.vb*" e o ficheiro "*AssemblyInfo.vb*" com algumas definições de compilação. [![SharpDevelop - Code Editor](wp-content/uploads/2008/02/sd_code_editor.thumbnail.jpg)](wp-content/uploads/2008/02/sd_code_editor.jpg "SharpDevelop - Code Editor")

Ao formulário vamos juntar um *Label* chamado "lblRequestname", uma *TextBox* chamada "txtName", e um *Button* chamado "btOk". [![SharpDevelop - Form Editor](wp-content/uploads/2008/02/sd_form_editor.thumbnail.jpg)](wp-content/uploads/2008/02/sd_form_editor.jpg "SharpDevelop - Form Editor")

No código do formulário criamos o evento *Click* do nosso botão e colocamos código para apresentar uma mensagem de saudação: [![SharpDevelop - Button Code](wp-content/uploads/2008/02/sd_button_code.jpg)](wp-content/uploads/2008/02/sd_button_code.jpg "SharpDevelop - Button Code")

A compilação directa do projecto, fazendo **Build** -\> **Build Solution** vai dar alguns erros, que poderão ser semelhantes a esta imagem: [![SharpDevelop - Errors](wp-content/uploads/2008/02/sd_errors.thumbnail.jpg)](wp-content/uploads/2008/02/sd_errors.jpg "SharpDevelop - Errors")[](wp-content/uploads/2008/02/sd_errors.jpg "SharpDevelop - Errors")

Nesta janela vemos que existem dois erros, ambos tem a ver com o facto de no código o SharpDevelop ter colocado código para o .NET Framework que não é compatível com a sua irmã mais pequena. Para os corrigir, deve-se editar o ficheiro "*.Designer.vb*" de cada um dos formulários e alterar alguns blocos de código.

O primeiro tem a ver com o facto de no botão ter aparecido este código: [![SharpDevelop - Code Before](wp-content/uploads/2008/02/sd_code_before.jpg)](wp-content/uploads/2008/02/sd_code_before.jpg "SharpDevelop - Code Before")

Esta linha e todas as que referirem a propriedade "UseVisualStyleBackColor" devem ser simplesmente apagadas, pois não tem qualquer utilidade no .NET Compact Framework.

No segundo erro o problema também tem a ver com as diferenças entre as duas frameworks. Para resolver este caso, temos de editar o ficheiro "*FormMain.Designer.vb*" e alterar o bloco seguinte de [![SharpDevelop - Code Before](wp-content/uploads/2008/02/sd_code_before_2.jpg)](wp-content/uploads/2008/02/sd_code_before_2.jpg "SharpDevelop - Code Before") para [![SharpDevelop - Code After](wp-content/uploads/2008/02/sd_code_after.jpg)](wp-content/uploads/2008/02/sd_code_after.jpg "SharpDevelop - Code After")

Para além destes dois erros deparei-me também com o facto de os controlos do tipo *Label* não terem propriedade *TabIndex* no Compact Framework, pelo que poderá ainda ser necessária a remoção manual de todos os *TabIndex* dos *Label* nos ficheiros "*.Designer.vb*".

E está o nosso programa pronto a compilar!!! :D

Por fim é só pegar no ficheiro executável criado (no nosso exemplo, "teste.exe"), copiar para o PDA, executar e apreciar o resultado! :) [![SharpDevelop - Test](wp-content/uploads/2008/02/sd_test.thumbnail.jpg)](wp-content/uploads/2008/02/sd_test.jpg "SharpDevelop - Test")

Há que ter em conta que outros problemas poderão surgir com mais controlos, pois o próprio SharpDevelop está preparado para criar código para o .NET Framework, pelo que a alteração manual do código gerado será sempre necessário! Esperemos que numa próxima versão ele já gere correctamente para o .NET Compact Framework!

Bons programas! ;)
