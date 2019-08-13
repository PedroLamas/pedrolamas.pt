---
layout: post
status: publish
published: true
title: Currency Converter for Windows Phone 7
wordpress_id: 1591
wordpress_url: http://www.pedrolamas.com/?p=1591
date: 2010-12-01 20:09:46.000000000 +00:00
categories:
- Mobilidade
tags:
- Microsoft
- Programação
- Windows Phone
- WP7
- WP7Dev
- Coding4Fun
- Marketplace
- Currency Converter
---
Já está publicado no [Coding4Fun](http://blogs.msdn.com/b/coding4fun/) o meu [artigo](http://blogs.msdn.com/b/coding4fun/archive/2010/11/30/10098706.aspx) sobre o desenvolvimento do [Currency Converter](tag/currency-converter/), uma simples aplicação de conversão de moedas para o Windows Phone 7, utilizando por base o [Bing](http://www.bing.com) para fazer os câmbios!

O [código-fonte](http://currency.codeplex.com/) está disponível no [Codeplex](http://www.codeplex.com/), e a aplicação completa pode ser instalada através do [Zune Marketplace](http://social.zune.net/redirect?type=phoneApp&id=e01bf520-cfe6-df11-a844-00237de2db9e).

Para quem assim desejar, obtive a permissão da Coding4Fun para fazer o "cross-posting" do artigo na língua de Camões, o qual poderão ler já abaixo!

Espero que gostem!!! ;)

Currency Converter for Windows Phone 7
--------------------------------------

O objectivo deste artigo é mostrar como qualquer um pode fazer o seu próprio conversor de moedas para Windows Phone 7, utilizando o Bing para fazer todo o trabalho duro dos câmbios!

**Código-fonte:** [CodePlex](http://currency.codeplex.com/)

**Aplicação:** [Zune Marketplace](http://social.zune.net/redirect?type=phoneApp&id=e01bf520-cfe6-df11-a844-00237de2db9e)

**Dificuldade:** Iniciado

**Tempo Necessário:** Cerca de 2 a 3 horas

**Custo:** Grátis!

**Software Necessário:** [Windows Phone Developer Tools](http://developer.windowsphone.com/windows-phone-7/), [Silverlight for Windows Phone Toolkit](http://silverlight.codeplex.com/)

**Hardware:** Um Computador

### Introdução

A Microsoft fez um trabalho absolutamente incrível com o Windows Phone 7, dando a cada um de nós todas as ferramentas necessárias para fazer com que as nossas aplicações tenham o melhor aspecto possível nele; e melhor parte - as ferramentas são, e serão sempre, grátis!! :)

### Utilizar o Bing para o câmbio de moedas

Vejamos o que acontece quando abro o Bing no meu Internet Explorer, e pesquisar por algo como "1 US Dollar in Euros":

[![](wp-content/uploads/2010/12/Using-Bing-to-exchange-currencies-thumb.jpg)](wp-content/uploads/2010/12/Using-Bing-to-exchange-currencies.jpg "Utilizar o Bing para efectuar câmbios")

Como podem verificar, o Bing entendeu perfeitamente a minha questão, e sabia que eu estava a procurar uma taxa de câmbio. Aí então o MSN Money fez o câmbio e apresentado o resultado.

Além disso, reparem que no endereço apareceu:

http://www.bing.com/search?q=**1+US+Dollar+in+Euros**&go=&form=QBRE&qs=n&sk=&sc=1-20

OK, mas como podemos nós usar isso para alimentar a nossa aplicação WP7? Abram o Internet Explorer Developer Tools (no IE8 ou superior, basta carregar em F12), escolham a opção "Select element by click" (Ctrl + B) e cliquem sobre o resultado na página para ver o código HTML. O resultado será algo como isto:

[![](wp-content/uploads/2010/12/Using-IE-Developer-Tools-thumb.jpg)](wp-content/uploads/2010/12/Using-IE-Developer-Tools.jpg "Utilizando o IE Developer Tools")

Agora já sabemos como fazer um pedido ao Bing para fazer câmbios (usando "http://www.bing.com/search?q=**{VALUE}**+**{SOURCE\_CURRENCY}**+in+**{DESTINATION\_CURRENCY}**&go=&form=QBRE&qs=n&sk=&sc=1-20") e também como encontrar os resultados no HTML devolvido (procurando por "\<span class="sc\_bigLine"\>**RESULT**\</span\>").

A última tarefa é encontrar o elemento "span" no HTML completo e extrair o valor convertido; para essa tarefa, eu vou recorrer à seguinte expressão regular (usando o namespace System.Text.RegularExpressions):

```csharp
static Regex _resultRegex = new Regex("<span class=\"sc_bigLine\">.*? = (?<value>[0-9.,]+).*?</span>");
```

Esta expressão procura no código HTML um elemento "span" com o atributo "class" igual "sc\_bigLine" e com o conteúdo interno a ir de encontro a algo do género presente no exemplo: "1 USD = 0,71270 euros".

A expressão também me permite extrair o número após o sinal "=", usando a propriedade Groups[“value"].value.

#### O MVVM é nosso amigo!

Vou começar citando a Wikipedia:

*O Model View ViewModel (MVVM) é um padrão de arquitetura de software (...) visa plataformas de desenvolvimento de interface de utilizador modernas (Windows Presentation Foundation e Silverlight), no qual há um programador de User Experience (UX) que tem necessidades diferentes do programador "tradicional" (ou seja, voltado para a lógica de negócio e desenvolvimento do back-end).*

Basicamente, o objectivo é separar claramente a interface de utilizador de todas as lógicas de negócio e dados que a nossa aplicação irá utilizar!

Podemos resumir o padrão MVVM com este esquema:

[![](wp-content/uploads/2010/12/The-MVVM-pattern.jpg)](wp-content/uploads/2010/12/The-MVVM-pattern.jpg "O padrão MVVM")

A View (Vista) é o XAML que serve de base à interface de utilizador (embora às vezes ele possa ter um ficheiro de C\# anexado) e que vai utilizar a fonte de dados para ler e alterar as propriedades da ViewModel e Actions para invocar os métodos.

O ViewModel é a abstracção da View e vai fazer todo o trabalho de ligação (Binding) da View ao Model, com as necessárias conversões de dados.

O Model (Modelo) pode representar a instância actual do modelo de objectos num dado estado conhecido, ou apenas a camada de acesso a dados.

Existe uma regra básica em MVVM: não olhar para cima! O Model não tem conhecimento quer do ViewModel quer da View, e o ViewModel não tem conhecimento da View.

Isso traz um novo desafio em como fazer as três componentes comunicar entre si, e é aí que as notificações entram!

Utilizamos notificações de forma a comunicar as alterações de um componente inferior para um superior; quando o componente superior é notificado, ele vai consultar o abaixo para saber o que mudou, e agir em conformidade com o novo estado!

Podem ler mais sobre MVVM por toda a internet, mas há um artigo de Shawn Wildermuth que é um dos melhores que já li; podem ler-lo aqui: [http://msdn.microsoft.com/enus/magazine/dd458800.aspx](http://msdn.microsoft.com/enus/magazine/dd458800.aspx).

#### O Model

Vamos começar na parte mais abaixo (também conhecido por, The Model) e, para tal criei as seguintes interfaces de apoio ao nosso modelo de dados:

[![](wp-content/uploads/2010/12/Base-interfaces.jpg)](wp-content/uploads/2010/12/Base-interfaces.jpg "Interfaces base")

```csharp
using System;

public interface ICurrencyExchangeService
{
    ICurrency[] Currencies { get; }

    void ExchangeCurrency(double amount, ICurrency fromCurrency, ICurrency toCurrency, Action<ICurrencyExchangeResult> callback);
}

public interface ICurrency
{
    string Name { get; }
}

public interface ICurrencyExchangeResult
{
    Exception Error { get; }

    string ExchangedCurrency { get; }

    double ExchangedAmount { get; }
}
```

A partir destas três interfaces vamos estender o nosso "conversor de moedas do Bing":

A classe abstrata CurrencyExchangeServiceBase implementa o método ICurrencyExchangeService.ExchangeCurrency para fazer todo o trabalho assíncrono necessário para requisitar dados a um servidor web e obter o HTML resultante; para atingir o seu objectivo, o CurrencyExchangeServiceBase tem dois métodos abstratos que terão de ser implementados: CreateRequestUrl, para obter o URL do pedido que será passado para o WebRequest, e GetResultFromResponseContent, para analisar o HTML recebido e retornar o valor do câmbio.

Em seguida, obtemos o BingCurrencyExchangeService apartir do CurrencyExchangeServiceBase, implementando os métodos CreateRequestUrl e GetResultFromResponseContent, e a propriedade ICurrencyExchangeService.Currencies, incluindo todas as moedas suportados por esta instância de serviço.

Temos também uma implementação básica da interface ICurrency chamada BingCurrency.

Esta é a implementação da classe abstracta CurrencyExchangeServiceBase:

[![](wp-content/uploads/2010/12/BingCurrency-classes.jpg)](wp-content/uploads/2010/12/BingCurrency-classes.jpg "Classes de suporte ao BingCurrency")

```csharp
public abstract class CurrencyExchangeServiceBase : ICurrencyExchangeService
{
    public abstract ICurrency[] Currencies { get; }

    protected abstract string CreateRequestUrl(double amount, ICurrency fromCurrency, ICurrency toCurrency);

    protected abstract double GetResultFromResponseContent(string responseContent);

    public void ExchangeCurrency(double amount, ICurrency fromCurrency, ICurrency toCurrency, Action<ICurrencyExchangeResult> callback)
    {
        var url = CreateRequestUrl(amount, fromCurrency, toCurrency);

        var request = HttpWebRequest.Create(url);

        request.BeginGetResponse(ar =>
        {
            try
            {
                var response = (HttpWebResponse)request.EndGetResponse(ar);

                if (response.StatusCode == HttpStatusCode.OK)
                {
                    string responseContent;

                    using (var streamReader = new StreamReader(response.GetResponseStream()))
                    {
                        responseContent = streamReader.ReadToEnd();
                    }

                    var exchangedCurrency = GetResultFromResponseContent(responseContent);

                    callback(new CurrencyExchangeResult(toCurrency.Name, exchangedCurrency));
                }
                else
                {
                    throw new Exception(string.Format("Http Error: ({0}) {1}",
                        response.StatusCode,
                        response.StatusDescription));
                }
            }
            catch (Exception ex)
            {
                callback(new CurrencyExchangeResult(ex));
            }
        }, null);
    }
}
```

Como foi dito anteriormente, esta é uma classe abstracta que exige que qualquer classe herdeira implemente a propriedade Currencies, bem como as funções CreateRequestUrl e GetResultFromResponseContent - e é exactamente isso que a classe BingCurrencyExchangeService faz:

Como podem aqui ver, a classe BingCurrencyExchangeService sabe como construir o endereço de pesquisa para o Bing com base nas moedas escolhidas e no valor a converter, e também como obter o valor convertido do código HTML da resposta (reparem na utilização da nossa expressão regular).

#### O ViewModel

O ViewModel tem uma exigência de herança básica: a interface INotifyPropertyChanged para notificar a interface de utilizador quando o valor de uma propriedade é alterado.

A nossa classe MainViewModel terá de expor três propriedades de dados base para os controlos na interface de utilizador: FromCurrency, ToCurrency e Amount.

Tudo devidamente montado, o ViewModel é assim:

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private ICurrencyExchangeService _currencyExchangeService;
    private double _amount;
    private ICurrency _fromCurrency;
    private ICurrency _toCurrency;

    public ICurrencyExchangeService CurrencyExchangeService
    {
        get
        {
            return _currencyExchangeService;
        }
        set
        {
            if (_currencyExchangeService == value)
                return;

            _currencyExchangeService = value;

            _fromCurrency = Currencies.FirstOrDefault(x => x.Name == "US Dollar") ?? Currencies[0];
            _toCurrency = Currencies.FirstOrDefault(x => x.Name == "Euro") ?? Currencies[1];

            RaisePropertyChanged("CurrencyExchangeService");
            RaisePropertyChanged("Currencies");
        }
    }

    public ICurrency[] Currencies
    {
        get
        {
            return _currencyExchangeService.Currencies;
        }
    }

    public string Amount
    {
        get
        {
            return _amount.ToString("0.00");
        }
        set
        {
            double amount;

            if (double.TryParse(value, out amount))
            {
                if (_amount == amount)
                    return;

                _amount = amount;

                RaisePropertyChanged("Amount");
            }
            else
                throw new Exception("Please enter a valid Amount");
        }
    }

    public ICurrency FromCurrency
    {
        get
        {
            return _fromCurrency;
        }
        set
        {
            if (_fromCurrency == value)
                return;

            _fromCurrency = value;

            RaisePropertyChanged("FromCurrency");
        }
    }

    public ICurrency ToCurrency
    {
        get
        {
            return _toCurrency;
        }
        set
        {
            if (_toCurrency == value)
                return;

            _toCurrency = value;

            RaisePropertyChanged("ToCurrency");
        }
    }

    public MainViewModel()
    {
        CurrencyExchangeService = new BingCurrencyExchangeService();
        Amount = "100";
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string propertyName)
    {
        if (PropertyChanged != null)
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

De forma a permitir que o utilizador altere as propriedades FromCurrency e ToCurrency, precisamos da lista das moedas disponíveis; e para tal, adicionamos uma propriedade Currencies para retornar a propriedade ICurrency[] Currencies da instância do Model.

No construtor do MainViewModel, inicializamos a propriedade CurrencyExchangeService para uma nova instância da classe BingCurrencyExchangeService, fazendo com que ele obtenha as moedas "Euro" e "US Dollar" (se disponíveis; se não, obter a 1ª e 2ª) como sendo as duas moedas de troca por omissão, e em seguida, inicializar a propriedade Amount com o valor por omissão de "100".

O evento PropertyChanged é uma implementação básica da interface INotifyPropertyChanged, com um método auxiliar chamado RaisePropertyChanged que permite invocar o evento.

Agora precisamos adicionar a funcionalidade necessária para solicitar uma operação de câmbio e mostrar os resultados na interface de utilizador:

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    //remaining code...

    private ICurrencyExchangeResult _result;

    public ICurrencyExchangeResult Result
    {
        get
        {
            return _result;
        }
        protected set
        {
            if (_result == value)
                return;

            _result = value;

            RaisePropertyChanged("Result");
            RaisePropertyChanged("ExchangedCurrency");
            RaisePropertyChanged("ExchangedAmount");
        }
    }

    public string ExchangedCurrency
    {
        get
        {
            if (_result == null)
                return string.Empty;

            return _result.ExchangedCurrency;
        }
    }

    public string ExchangedAmount
    {
        get
        {
            if (_result == null)
                return string.Empty;

            return _result.ExchangedAmount.ToString("N2");
        }
    }

    public void ExchangeCurrency()
    {
        _currencyExchangeService.ExchangeCurrency(_amount, _fromCurrency, _toCurrency, CurrencyExchanged);
    }

    private void CurrencyExchanged(ICurrencyExchangeResult result)
    {
        System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
        {
            Result = result;
        });
    }
}
```

O método ExchangeCurrency será utilizado para solicitar uma operação de câmbio na instância do Model, incluindo os dados introduzidos. Quando a operação termina, o método CurrencyExchanged será invocado e vamos então obter uma instância do ICurrencyExchangeResult com os resultados; este método será chamado de forma assíncrona noutro thread, por isso precisamos de obter o Dispatcher actual e usá-lo para invocar as alterações na propriedade MainViewModel.Result (de modo que ela ocorra no thread da interface de utilizador).

#### A View

A View que vai suportar a nossa aplicação é realmente muito simples!

Os requisitos básicos são dois controlos ListPicker (implementados no Silverlight Toolkit), um para cada moeda (as propriedades FromCurrency e ToCurrency do MainViewModel) e uma TextBox para o valor (Amount).

Vamos precisar ainda de duas TextAreas para mostrar os resultados do câmbio (uma para a propriedade ExchangedCurrency e outra para a propriedade ExchangedAmount, ambas do nosso MainViewModel).

Usando o MainPage.xaml actual, começamos por adicionar uma referência para o Silverlight Toolkit para Windows Phone, assim:

```xml
<phone:PhoneApplicationPage
xmlns:toolkit="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone.Controls.Toolkit"
<!--remaining code... -->
```

Depois, mudamos controle raiz "ContentPanel" de uma Grid para um StackPanel, e adicionar os controlos necessários introduzir as moedas e o valor a converter:

```xml
<StackPanel x:Name="ContentPanel" Grid.Row="1" Margin="12,0">
            <TextBlock Margin="12,0,0,-5" Style="{StaticResource PhoneTextSubtleStyle}">Amount</TextBlock>
            <TextBox InputScope="TelephoneNumber" Text="{Binding Amount, Mode=TwoWay, ValidatesOnExceptions=True, NotifyOnValidationError=True}" />
            <TextBlock Margin="12,10,0,-5" Style="{StaticResource PhoneTextSubtleStyle}">From</TextBlock>
            <toolkit:ListPicker ItemsSource="{Binding Currencies}" SelectedItem="{Binding FromCurrency, Mode=TwoWay}" FullModeHeader="FROM CURRENCY" Style="{StaticResource CurrencyListPicker}" />
            <TextBlock Margin="12,10,0,-5" Style="{StaticResource PhoneTextSubtleStyle}">To</TextBlock>
            <toolkit:ListPicker ItemsSource="{Binding Currencies}" SelectedItem="{Binding ToCurrency, Mode=TwoWay}" FullModeHeader="TO CURRENCY" Style="{StaticResource CurrencyListPicker}" />
            <StackPanel>
                <TextBlock Style="{StaticResource PhoneTextGroupHeaderStyle}" Text="{Binding ExchangedCurrency}"></TextBlock>
                <TextBlock Margin="25, 0, 0, 0" Style="{StaticResource PhoneTextTitle1Style}" Text="{Binding ExchangedAmount}"></TextBlock>
            </StackPanel>
</StackPanel>
```

E aqui está como ela vai aparecer:

[![](wp-content/uploads/2010/12/Main-user-interface.jpg)](wp-content/uploads/2010/12/Main-user-interface.jpg "Interface do Utilizador")

Repararam no Style CurrencyListPicker que foi aplicado nos dois controlos ListPicker no código acima? Aqui está ele:

```xml
<phone:PhoneApplicationPage.Resources>
    <Style x:Key="CurrencyListPicker" TargetType="toolkit:ListPicker">
        <Setter Property="DisplayMemberPath" Value="Name" />
        <Setter Property="CacheMode" Value="BitmapCache" />
        <Setter Property="FullModeItemTemplate">
            <Setter.Value>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal" Margin="16 21 0 20">
                        <TextBlock Text="{Binding Name}" FontSize="43" FontFamily="{StaticResource PhoneFontFamilyLight}"/>
                    </StackPanel>
                </DataTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</phone:PhoneApplicationPage.Resources>
```

Este Style define os valores por omissão de três propriedades dos controlos ListPicker: o DisplayMemberPath (será ligado (Binding) à propriedade ICurrency.Name), o CacheMode (apenas para efeitos de performance) e, a mais importante, o FullModeItemTemplate, que indica como deve ser exibido cada item, quando a janela do ListPicker aparece.

O próximo passo será adicionar o botão de "Exchange" (Cambiar) no ApplicationBar no fundo da página, assim:

```xml
<phone:PhoneApplicationPage.ApplicationBar>
    <shell:ApplicationBar IsVisible="True" IsMenuEnabled="False">
        <shell:ApplicationBarIconButton IconUri="/Images/appbar.money.usd.png" Text="Exchange" Click="ExchangeIconButton_Click" />
    </shell:ApplicationBar>
</phone:PhoneApplicationPage.ApplicationBar>
```

Lembrem-se de criar uma pasta "Images" no projecto e copiar a imagem "appbar.money.usd.png" para lá (podem encontrar este e outros ícones para a ApplicationBar em "%ProgramFiles%\\Microsoft SDKs\\Windows Phone\\v7.0\\Icons\\dark"); de seguida, coloquem as propriedades "Build" e "Copy to Output Directory" do ficheiro para "Content" e "Copy always" respectivamente.

No ficheiro MainPage.xaml.cs, vamos adicionar o método ExchangeIconButton\_Click():

```csharp
private void ExchangeIconButton_Click(object sender, EventArgs e)
{
    var viewModel = DataContext as MainViewModel;

    if (viewModel == null)
        return;

    Focus();

    Dispatcher.BeginInvoke(() =>
    {
        if (!NetworkInterface.GetIsNetworkAvailable())
        {
            MessageBox.Show("No network connection found!", "Error", MessageBoxButton.OK);
            return;
        }

        viewModel.ExchangeCurrency();
    });
}
```

Este método vai verificar se temos uma instância do MainViewModel válido como DataContext, remover o foco de qualquer TextBox (e assim ocultar o teclado do ecrã), verificar se há uma ligação de rede activa, e depois invocar o método MainViewModel.ExchangeCurrency().

### Tombstoning

A peça final para terminar o desenvolvimento e criar a aplicação perfeita para Windows Phone 7, é fazer com que a aplicação faça "Tombstoning"!

Dado que o WP7 não faz "multitasking" (só permite uma aplicação aberta de cada vez), sempre que uma aplicação é desactivada ou fechada, o seu estado deve ser guardado, e restaurado quando a aplicação for reactivada ou reaberta; este processo é conhecido por "Tombstoning"

Para isso, usamos as classes disponíveis no namespace System.Runtime.Serialization, em especial o atributo DataMemberAttribute para marcar todas as propriedades do MainViewModel que pretendemos guardar (FromCurrency, ToCurrency e Amount), e o atributo IgnoreDataMemberAttribute para marcar as que não pretendemos (CurrencyExchangeService, Currencies, Result, ExchangedCurrency, e ExchangedAmount).

Não podemos (ou pelo menos, não devemos!) tentar guardar directamente o estado das propriedades FromCurrency e ToCurrency dado que são de tipos complexos (em vez de um tipo base, como int ou string); vamos então marcar estas propriedades com o IgnoreDataMemberAttribute e adicionar duas novas propriedades, uma para cada, que irão obter / definir o índice relativo no array da propriedade Currencies:

```csharp
[DataMember]
public int FromCurrencyIndex
{
    get
    {
        return Array.IndexOf(Currencies, FromCurrency);
    }
    set
    {
        FromCurrency = Currencies[value];
    }
}

[DataMember]
public int ToCurrencyIndex
{
    get
    {
        return Array.IndexOf(Currencies, ToCurrency);
    }
    set
    {
        ToCurrency = Currencies[value];
    }
}
```

Vamos agora adicionar os métodos necessários para carregar e guardar o estado da instância do MainViewModel; uma vez que o método Save() tem que ser invocado nos eventos Application\_Deactivated() e Application\_Closing() do App.xaml.cs, a forma mais simples é fazer com que o nosso ModelView seja um singleton. Para isso, adicionamos o seguinte código na classe MainViewModel:

```csharp
[IgnoreDataMember]
private const string SettingFileName = "mainviewmodel.dat";

public static MainViewModel Instance { get; protected set; }

static MainViewModel()
{
    Instance = Load();
}

public static MainViewModel Load()
{
    return StorageHelper.LoadContract<MainViewModel>(SettingFileName, true);
}

public void Save()
{
    StorageHelper.SaveContract(SettingFileName, this, true);
}
```

A classe StorageHelper que aqui podem ver é apenas um auxiliar utilizado para serializar e desserializar um ficheiro que será guardado no IsolatedStorage da aplicação.

Agora tudo o que precisamos é mudar o construtor do MainPage.xaml.cs para que ele utilize o nosso singleton em vez de criar uma nova instância do MainViewModel:

```csharp
public MainPage()
{
    InitializeComponent();

    this.DataContext = MainViewModel.Instance;
}
```

Por fim, invocar o método Save() nos dois eventos do ficheiro App.xaml.cs:

```csharp
private void Application_Deactivated(object sender, DeactivatedEventArgs e)
{
    MainViewModel.Instance.Save();
}

private void Application_Closing(object sender, ClosingEventArgs e)
{
    MainViewModel.Instance.Save();
}
```

E está pronto! Para testar a aplicação, abram-na, pressionem a tecla Windows para que o ecrã principal do Windows Phone apareceça, aguardem alguns segundos e pressionem a tecla Back para voltar à aplicação: vai aparecer uma mensagem "resuming" enquanto a aplicação restaura o seu estado!

A aplicação pode agora ser "Tombstoned" a qualquer momento, e quando for aberta novamente, ela surgirá como se nunca tivesse fechado!

### Conclusão

Como podem ver, é possível construir aplicações muito simples para o Windows Phone 7 recorrendo a uma arquitetura MVVM básica, e ainda ter tempo para sair e tomar um café com os amigos!

Lembrem-se: Se quiserem experimentar isto, o endereço para download do código-fonte está no topo do artigo!

O código de exemplo usa o BingCurrencyExchangeService, mas tem também um segundo fornecedor para o MSN Money, o MsnMoneyCurrencyExchangeService, que você podem utilizar se assim quiserem!
