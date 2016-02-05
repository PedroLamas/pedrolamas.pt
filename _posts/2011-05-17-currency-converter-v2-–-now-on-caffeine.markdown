---
layout: post
status: publish
published: true
title: Currency Converter v2 – Now on Caffeine!
wordpress_id: 1823
wordpress_url: http://www.pedrolamas.com/?p=1823
date: 2011-05-17 16:36:43.000000000 +01:00
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
Algumas semanas atrás foi publicado no [Channel9](http://channel9.msdn.com/) o meu [segundo artigo](http://channel9.msdn.com/coding4fun/articles/Currency-Converter-v2--Now-on-Caffeine) para o Coding4Fun, sobre a versão 2 do [Currency Converter](/tag/currency-converter/)!

Este é esse mesmo artigo, agora traduzido para Português e com algumas correcções à mistura! :)

Currency Converter v2 – Now on Caffeine!
----------------------------------------

**Código-fonte:** [CodePlex](http://currency.codeplex.com/)

**Aplicação:** [Zune Marketplace](http://social.zune.net/redirect?type=phoneApp&id=e01bf520-cfe6-df11-a844-00237de2db9e)

**Dificuldade:** Iniciado

**Tempo Necessário:** Pouco!

**Custo:** Grátis!

**Software Necessário:** [Windows Phone Developer Tools](http://developer.windowsphone.com/windows-phone-7/), [Silverlight for Windows Phone Toolkit](http://silverlight.codeplex.com/)

**Hardware:** Um Computador

### Introdução

Tomando em consideração as reacções dos utilizadores quanto à aplicação, está na altura de fazermos alguns melhoramentos! :)

Aqui estão alguns dos comentários que nos chegaram:

-   A aplicação é demasiado lenta a fazer a conversão
-   Utiliza demasiado tráfego de dados / deveria armazenar as taxas de câmbio para reutilização
-   Não funciona correctamente com algumas moedas
-   Os resultados são imprecisos / utiliza taxas de câmbio desactualizadas

A conclusão que podemos retirar destes comentários é que precisamos de uma fonte de dados mais fidedigna, e que deveriamos usar algum tipo de mecanismo de caching para as taxas de câmbio...

Acho que vou ligar a chaleira para fazer café!...

### To Bing or not to Bing...

A primeira versão do Currency Converter utiliza o Bing para fazer os câmbios, o que levou a alguns dos relatos que podemos ler anteriormente!

Contudo, nesta versão decidimos utilizar o MSN Money dado que é mais preciso e com dados actuais, e porque funciona sempre bem, independentemente de quais as moedas que estamos a utilizar!

Para além disso, o MSN Money fornece uma página onde podemos ver as taxas de câmbio mais actuais em relação ao dólar americano (US Dollar); basta abrir o Internet Explorer 8.0+ e navegar para este endereço:

[http://moneycentral.msn.com/investor/market/exchangerates.aspx](http://moneycentral.msn.com/investor/market/exchangerates.aspx)

[![](/wp-content/uploads/2011/05/Taxas-de-Câmbio-para-USD-no-MSN-Money-Thumb.jpg)](/wp-content/uploads/2011/05/Taxas-de-Câmbio-para-USD-no-MSN-Money.jpg "Taxas de Câmbio para USD no MSN Money")

Como podem ver aqui, temos todos os dados que precisamos para converter de X para USD e de USD para X, e até conseguimos converter de X para USD para Y.

Então, porque não obter todos estes dados num único pedido, fazer cache deles, e utilizar de forma offline para fazer as conversões? :)

Tal como anteriormente, vamos obter os dados que precisamos directamente do HTML da página utilizando expressões regulares (Regular Expressions). Para isso, abram o Internet Explorer Develop Tools (carreguem em F12), utilizem a ferramenta "Seleccionar elemento por click" (Ctrl + B), e cliquem sobre o texto "Argentine Peso"; deverão obter algo similar a esta imagem:

[![](/wp-content/uploads/2011/05/Utilizando-o-IE-Developer-Tools-Thumb.jpg)](/wp-content/uploads/2011/05/Utilizando-o-IE-Developer-Tools.jpg "Utilizando o IE Developer Tools")

Utilizando a informação acima, conseguimos facilmente detectar um padrão no código:

[code language="html"] \<tr\> \<td\>CURRENCY\</td\> \<td style=”text-align:right”\>\<a SOMETHING\>VALUE\_IN\_USD\</a\>\</td\> \<td style=”text-align:right”\>\<a SOMETHING\>VALUE\_PER\_USD\</a\>\</td\> \</tr\> [/code]

E agora que sabemos qual o padrão, estamos em condições de construir a expressão regular:

[code language="csharp"] private static Regex \_resultRegex = new Regex("\<tr\>\<td\>(?\<currency\>[\^\<\>]+)\</td\>\<td style=""text-align:right""\>.\*?\>(?\<value\>[0-9.,]+)\</a\>\</td\>\</tr\>"); [/code]

Ao aplicar esta expressão regular sobre o HTML vamos obter todas as linhas que a satisfazem, e com isso o nome da Moeda e o valor da taxa de câmbio para USD!

### É altura de produzir algum código

Agora que sabemos como obter todas as taxas de câmbio a partir de um único URL, está na altura de fazermos as alterações necessárias no nosso código para acomodar os novos dados!

Tal como no artigo anterior, vamos manter a arquitectura MVVM, apresentando o código de baixo (Model) para cima (View).

#### O (Re)Model

Aqui estão as modificações que vamos precisar de fazer no nosso Model de forma a acomodar e guardar as taxas de câmbio obtidas:

-   Fazer com que cada moeda guarde o valor da sua taxa de câmbio e a data da última actualização
-   Marcar a "moeda base" (US Dollar), sendo esta a que terá a taxa de câmbio de 1.0 (a tentar converter de USD para USD? Pois, pois...)
-   Adicionar uma operação para "Actualizar Taxas de Câmbio" ao serviço

E aqui está todo o modelo de classes completo, com as alterações marcadas:

[![](/wp-content/uploads/2011/05/Interfaces-base-Thumb.jpg)](/wp-content/uploads/2011/05/Interfaces-base.jpg "Interfaces base")

[code language="csharp" highlight="7,11,18,20,32,33,34,35,36,37"] using System;

public interface ICurrencyExchangeService { ICurrency[] Currencies { get; }

ICurrency BaseCurrency { get; }

void ExchangeCurrency(double amount, ICurrency fromCurrency, ICurrency toCurrency, Action\<ICurrencyExchangeResult\> callback);

void UpdateCachedExchangeRates(Action\<CachedExchangeRatesUpdateResult\> callback, object state); }

public interface ICurrency { string Name { get; }

double CachedExchangeRate { get; set; }

DateTime CachedExchangeRateUpdatedOn { get; set; } }

public interface ICurrencyExchangeResult { Exception Error { get; }

string ExchangedCurrency { get; }

double ExchangedAmount { get; } }

public interface ICachedExchangeRatesUpdateResult { Exception Error { get; }

object State { get; } } [/code]

Na interface ICurrencyExchangeService adicionamos uma nova propriedade chamada BaseCurrency onde vamos colocar a instância da moeda relativa a "US Dollar", e um novo método chamado UpdateCachedExchangeRates para actualizar todas as taxas de câmbio.

Na interface ICurrency, temos agora duas novas propriedades: a CachedExchangeRate para guardar o valor da taxa de câmbio, e a CachedExchangeRateUpdatedOn para registar a data da última actualização.

Foi ainda adicionada uma nova interface chamada ICachedExchangeRatesUpdateResult de forma a devolver qualquer excepção lançada pela chamada assíncrona do método ICurrencyExchangeService.UpdateCachedExchangeRates.

Vamos agora ver a implementação das interfaces:

[![](/wp-content/uploads/2011/05/Classes-de-suporte-ao-MsnMoneyV2CurrencyExchangeService-Thumb.jpg)](/wp-content/uploads/2011/05/Classes-de-suporte-ao-MsnMoneyV2CurrencyExchangeService.jpg "Classes de suporte ao MsnMoneyV2CurrencyExchangeService")

A primeira novidade é que agora temos uma classe abstracta chamada CurrencyBase. É desta classe que vamos estender a classe MsnMoneyCurrency, adicionando uma simples propriedade Id para guardar o valor numérico do identificador da moeda encontrado no MSN Money.

De seguida temos a classe MsnMoneyV2CurrencyExchangeService, uma implementação directa da interface ICurrencyExchangeService.

Contrariamente à classe BingCurrencyExchangeService que utilizamos na versão anterior, reparem que a classe MsnMoneyV2CurrencyExchangeService não estende a classe abstracta CurrencyExchangeServiceBase, e que apenas faz pedidos de dados no método UpdateCachedExchangeRates e não a cada chamada do método ExchangeCurrency.

Aqui está o código para estas classes:

[code language="csharp"] public class MsnMoneyV2CurrencyExchangeService : ICurrencyExchangeService { private const string MsnMoneyUrl = "http://moneycentral.msn.com/investor/market/exchangerates.aspx?selRegion=1&selCurrency=1";

\#region Static Globals

private static Regex \_resultRegex = new Regex(@"\<tr\>\<td\>(?\<currency\>[\^\<\>]+)\</td\>\<td style=""text-align:right""\>.\*?\>(?\<value\>[0-9.,]+)\</a\>\</td\>\</tr\>");

private static ICurrency[] \_currencies = new ICurrency[] { //The currencies exposed by MSN Money will go here };

\#endregion

\#region Properties

public ICurrency[] Currencies { get { return \_currencies; } }

public ICurrency BaseCurrency { get; protected set; }

\#endregion

public MsnMoneyV2CurrencyExchangeService() { BaseCurrency = Currencies.First(x =\> x.Name == "US Dollar"); }

public void ExchangeCurrency(double amount, ICurrency fromCurrency, ICurrency toCurrency, bool useCachedExchangeRates, Action\<ICurrencyExchangeResult\> callback, object state) { if (useCachedExchangeRates) { try { ExchangeCurrency(amount, fromCurrency, toCurrency, callback, state);

return; } catch { } }

UpdateCachedExchangeRates(result =\> { if (result.Error != null) { callback(new CurrencyExchangeResult(result.Error, state));

return; }

try { ExchangeCurrency(amount, fromCurrency, toCurrency, callback, state); } catch (Exception ex) { callback(new CurrencyExchangeResult(ex, state)); } }, state); }

private void ExchangeCurrency(double amount, ICurrency fromCurrency, ICurrency toCurrency, Action\<ICurrencyExchangeResult\> callback, object state) { var fromExchangeRate = fromCurrency.CachedExchangeRate; var toExchangeRate = toCurrency.CachedExchangeRate; var timestamp = DateTime.Now;

if (fromCurrency == BaseCurrency) fromExchangeRate = 1.0; else { if (timestamp \> fromCurrency.CachedExchangeRateUpdatedOn) timestamp = fromCurrency.CachedExchangeRateUpdatedOn; }

if (toCurrency == BaseCurrency) toExchangeRate = 1.0; else { if (timestamp \> toCurrency.CachedExchangeRateUpdatedOn) timestamp = toCurrency.CachedExchangeRateUpdatedOn; }

if (fromExchangeRate \> 0 && toExchangeRate \> 0) { var exchangedAmount = amount / fromExchangeRate \* toExchangeRate;

callback(new CurrencyExchangeResult(toCurrency, exchangedAmount, timestamp, state)); } else throw new Exception("Conversion not returned!"); }

public void UpdateCachedExchangeRates(Action\<CachedExchangeRatesUpdateResult\> callback, object state) { var request = HttpWebRequest.Create(MsnMoneyUrl);

request.BeginGetResponse(ar =\> { try { var response = (HttpWebResponse)request.EndGetResponse(ar);

if (response.StatusCode == HttpStatusCode.OK) { string responseContent;

using (var streamReader = new StreamReader(response.GetResponseStream())) { responseContent = streamReader.ReadToEnd(); }

foreach (var match in \_resultRegex.Matches(responseContent).Cast\<Match\>()) { var currencyName = match.Groups["currency"].Value.Trim();

var currency = Currencies.FirstOrDefault(x =\> string.Compare(x.Name, currencyName, StringComparison.InvariantCultureIgnoreCase) == 0);

if (currency != null) { currency.CachedExchangeRate = double.Parse(match.Groups["value"].Value, CultureInfo.InvariantCulture); currency.CachedExchangeRateUpdatedOn = DateTime.Now; } }

callback(new CachedExchangeRatesUpdateResult(ar.AsyncState)); } else { throw new Exception(string.Format("Http Error: ({0}) {1}", response.StatusCode, response.StatusDescription)); } } catch (Exception ex) { callback(new CachedExchangeRatesUpdateResult(ex, ar.AsyncState)); } }, state); } } [/code]

Este é o funcionamento: quando o método ExchangeCurrency é invocado, nós passamos um parâmetro (useCachedExchangeRates) que indica ao método que deve (ou não!) utilizar as taxas de câmbio anteriormente obtidas e guardadas.

De seguida, fazemos a operação de conversão e devolvemos os resultados. Se a operação lançar uma excepção, ou se não lhe permitimos usar as taxas guardadas, invocamos o método UpdateCachedExchangeRates para actualizar as taxas de câmbio e depois tentamos novamente a conversão com os novos dados.

E é tudo relativamente ao Model!

#### O ViewModel

O ViewModel da versão anterior será mantido na integra, mas vamos adicionar algumas funcionalidades novas. Aqui está o código (as principais alterações estão marcadas):

[code language="csharp" highlight="24,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,82,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,134,135,136,137,138,139,143,144,145,146,147,148,149,150,151,152,153"] public class MainViewModel : INotifyPropertyChanged { //Full previous code

\#region Properties

[IgnoreDataMember] public ICurrencyExchangeResult Result { get { return \_result; } protected set { if (\_result == value) return;

\_result = value;

RaisePropertyChanged("Result"); RaisePropertyChanged("ExchangedCurrency"); RaisePropertyChanged("ExchangedAmount"); RaisePropertyChanged("ExchangedTimeStamp"); } }

[IgnoreDataMember] public string ExchangedTimeStamp { get { if (\_result == null) return string.Empty;

return string.Format("Data freshness:\\n{0} at {1}", \_result.Timestamp.ToShortDateString(), \_result.Timestamp.ToShortTimeString()); } }

[DataMember] public CurrencyCachedExchangeRate[] CurrenciesCachedExchangeRates { get { return Currencies .Select(x =\> new CurrencyCachedExchangeRate() { CurrencyIndex = Array.IndexOf(Currencies, x), CachedExchangeRate = x.CachedExchangeRate, CachedExchangeRateUpdatedOn = x.CachedExchangeRateUpdatedOn }) .ToArray(); } set { foreach (var currencyData in value) { if (currencyData.CurrencyIndex \>= Currencies.Length) continue;

var currency = Currencies[currencyData.CurrencyIndex];

currency.CachedExchangeRate = currencyData.CachedExchangeRate; currency.CachedExchangeRateUpdatedOn = currencyData.CachedExchangeRateUpdatedOn; } } }

\#endregion

//Full previous code

public void ExchangeCurrency() { if (Busy) return;

BusyMessage = "Exchanging amount...";

\_currencyExchangeService.ExchangeCurrency(\_amount, \_fromCurrency, \_toCurrency, true, CurrencyExchanged, null); }

public void UpdateCachedExchangeRates() { if (Busy) return;

BusyMessage = "Updating cached exchange rates...";

\_currencyExchangeService.UpdateCachedExchangeRates(ExchangeRatesUpdated, null); }

private void CurrencyExchanged(ICurrencyExchangeResult result) { InvokeOnUiThread(() =\> { Result = result;

BusyMessage = null;

if (result.Error != null) { if (System.Diagnostics.Debugger.IsAttached) System.Diagnostics.Debugger.Break(); else MessageBox.Show("An error has ocorred!", "Error", MessageBoxButton.OK); } }); }

private void ExchangeRatesUpdated(ICachedExchangeRatesUpdateResult result) { InvokeOnUiThread(() =\> { BusyMessage = null;

Save();

if (result.Error != null) { if (System.Diagnostics.Debugger.IsAttached) System.Diagnostics.Debugger.Break(); else MessageBox.Show("An error has ocorred!", "Error", MessageBoxButton.OK); } }); }

private void InvokeOnUiThread(Action action) { var dispatcher = System.Windows.Deployment.Current.Dispatcher;

if (dispatcher.CheckAccess()) action(); else dispatcher.BeginInvoke(action); }

\#region Auxiliary Classes

public class CurrencyCachedExchangeRate { [DataMember] public int CurrencyIndex { get; set; }

[DataMember] public double CachedExchangeRate { get; set; }

[DataMember] public DateTime CachedExchangeRateUpdatedOn { get; set; } }

\#endregion } [/code]

A primeira coisa que vão aqui notar é uma nova propriedade só de leitura chamada ExchangedTimeStamp que fornece à interface a data da última actualização dos dados. A interface é notificada que o valor desta propriedade foi alterado quando a propriedade Result é ela mesma alterada.

Mais abaixo encontramos outra nova propriedade, CurrenciesCachedExchangeRates, que guarda os valores das taxas de câmbio. Para o funcionamento correcto deste mecanismo, temos uma classe auxiliar chamada CurrencyCachedExchangeRate, que guarda o índice da moeda juntamente com a taxa da câmbio e a data da última actualização.

O método UpdateCachedExchangeRates permite aos utilizadores forçarem uma actualização das taxas de câmbio.

Os callbacks CurrencyExchanged e ExchangeRatesUpdated utilizam o método InvokeOnUiThread para que todo o seu código seja executado correctamente no thread da interface o utilizador.

#### A View

Duas simples alterações foram feitas no MainPage.xaml (a nossa View principal): adicionamos uma área no ecrã para mostrar a data da operação de câmbio, e uma nova opção foi adicionada ao menu para forçar a actualização das taxas de câmbio.

No caso da primeira alteração, adicionou-se uma TextArea no fundo do StackPanel e ligamos a mesma à propriedade ExchangedTimeStamp no ViewModel:

[code language="xml" highlight="11"] \<StackPanel x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0"\> \<TextBlock Margin="12,0,0,-5" Style="{StaticResource PhoneTextSubtleStyle}"\>Amount\</TextBlock\> \<TextBox InputScope="TelephoneNumber" Text="{Binding Amount, Mode=TwoWay, ValidatesOnExceptions=True, NotifyOnValidationError=True}" /\> \<TextBlock Margin="12,10,0,-5" Style="{StaticResource PhoneTextSubtleStyle}"\>From\</TextBlock\> \<toolkit:ListPicker ItemsSource="{Binding Currencies}" SelectedItem="{Binding FromCurrency, Mode=TwoWay}" FullModeHeader="FROM CURRENCY" Style="{StaticResource CurrencyListPicker}" /\> \<TextBlock Margin="12,10,0,-5" Style="{StaticResource PhoneTextSubtleStyle}"\>To\</TextBlock\> \<toolkit:ListPicker ItemsSource="{Binding Currencies}" SelectedItem="{Binding ToCurrency, Mode=TwoWay}" FullModeHeader="TO CURRENCY" Style="{StaticResource CurrencyListPicker}" /\> \<StackPanel\> \<TextBlock Style="{StaticResource PhoneTextGroupHeaderStyle}" Text="{Binding ExchangedCurrency}"\>\</TextBlock\> \<TextBlock Margin="25, 0, 0, 0" Style="{StaticResource PhoneTextTitle1Style}" Text="{Binding ExchangedAmount}"\>\</TextBlock\> \<TextBlock Style="{StaticResource PhoneTextSubtleStyle}" Text="{Binding ExchangedTimeStamp}" TextWrapping="Wrap" TextAlignment="Right"\>\</TextBlock\> \</StackPanel\> \</StackPanel\> [/code]

Já para a opção "update exchange rates" no menu, adicionamos um novo ApplicationBarMenuItem à colecção de MenuItems, associamos o texto apropriado, e criamos o método handler para o evento Click deste elemento:

[code language="xml" highlight="5"] \<phone:PhoneApplicationPage.ApplicationBar\> \<shell:ApplicationBar IsVisible="True" IsMenuEnabled="True"\> \<shell:ApplicationBarIconButton IconUri="/Images/appbar.money.usd.png" Text="Exchange" Click="ExchangeIconButton\_Click" /\> \<shell:ApplicationBar.MenuItems\> \<shell:ApplicationBarMenuItem Text="update exchange rates" Click="UpdateExchangeRatesMenuItem\_Click" /\> \<shell:ApplicationBarMenuItem Text="about" Click="AboutMenuItem\_Click" /\> \</shell:ApplicationBar.MenuItems\> \</shell:ApplicationBar\> \</phone:PhoneApplicationPage.ApplicationBar\> [/code]

Agora, tudo o que falta mesmo é implementar o método UpdateExchangeRatesMenuItem\_Click:

[code language="csharp"] private void UpdateExchangeRatesMenuItem\_Click(object sender, EventArgs e) { var viewModel = DataContext as MainViewModel;

if (viewModel == null) return;

Dispatcher.BeginInvoke(() =\> { viewModel.UpdateCachedExchangeRates(); }); } [/code]

### Conclusão

A grande moral a retirar é que as nossas aplicações são apenas tão boas quanto as suas fontes de dados o forem. Ao utilizar uma nova (melhor) fonte de dados, algumas alterações muito simples no código, ficamos agora com o Currency Converter mais rápido do que nunca!

E mesmo a tempo: o café está pronto!!! :)
