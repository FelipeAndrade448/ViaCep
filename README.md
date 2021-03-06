# ViaCep
Um módulo para a consulta de endereços usando o Web Service da ViaCep.

## Características

* Fácil manipulação.
* Retorna todos os formatos (Json, Xml, Piped, Querty).
* Uso da Interface Fluent.
* Uso de Callback Methods.

## Como Utilizar

Primeiro você precisa instanciar um objeto da classe `ViaCepService`.

``` c#
var viaCepService = new ViaCepService();
```

Existem duas formas de consultar endereços.

* Consultando por Cep.

Crie um objeto do tipo `Cep` ou use uma `string` se preferir.

``` c#
Cep cep = "01001000";

var endereco = viaCepService.ObterEndereco(cep); // viaCep.ObterEndereco("01001000");
```
Nesse caso o endereço será retornado como um objeto do tipo [Endereco](MosaicoSolutions.ViaCep/Modelos/Endereco.cs).
Se desejar retornar como outros formatos:

``` c#
var enderecoJson = viaCepService.ObterEnderecoComoJson(cep); //viaCep.ObterEnderecoComoJson("01001000");
```
Você ainda pode retornar com `Xml`, `Piped`, ou `Querty` utilizando os métodos `ObterEnderecoComoXml` , `ObterEnderecoComoPiped` e 
`ObterEnderecoComoQuerty`, respectivamente, ambos métodos da classe [ViaCepService](MosaicoSolutions.ViaCep/ViaCepService.cs).

* Consultando por Endereco

A consulta também pode ser realizada a partir de três parâmetros: Sigla do estado, Nome da Cidade, e Nome do Logradouro.

``` c#
var requisicao = new EnderecoRequisicao {
                    UF = UF.RS,
                    Cidade = "Porto Alegre",
                    Logradouro = "Olavo"
                };

var enderecos = viaCepService.ObterEnderecos(requisicao);
```
Neste exemplo será pesquisado na cidade de "Porto Algre/RS" por todos os logradouros que contenham "Olavo" em seu nome. 
Quando o nome da cidade ou do logradouro não contiver ao menos três caracteres será lançado uma *Exception*;

O resultado desta consulta é um `IEnumerable<Endereco>` contendo todos os resultados. Caso nenhum endereço seja encontrado uma lista vazia será retornada.

Use os métodos `ObterEnderecosComoJson` e `ObterEnderecosComoXml`, ambos da classe `ViaCepService`, para retornar os resultados nos formatos 
`Json` e `Xml`, respectivamente.

A classe [ViaCepService](MosaicoSolutions.ViaCep/ViaCepService.cs) também fornece métodos assíncronos.

``` c#
var xml = await viaCepService.ObterEnderecoComoXmlAsync("01001000");
```

* Fluent Interface

Para facilitar ainda mais as consultas utilize a *Fluent Interface* veja como é simples.

``` c#
var json = ViaCepFluent.De("01001000").ComoJson();
```
Se desejar utilizar callbacks adicione o namespace `MosaicoSolutions.ViaCep.Fluent.Callback`.

``` c#
var requisicao = new EnderecoRequisicao {
                    UF = UF.RS,
                    Cidade = "Porto Alegre",
                    Logradouro = "Olavo"
                };

ViaCepFluent.De(requisicao)
            .ComoListaDeEnderecos(enderecos => {
                foreach(var endereco in enderecos)
                {
                      Console.WriteLine($"Cep: {endereco.Cep}");
                      Console.WriteLine($"Cidade: {endereco.Localidade}");
                      Console.WriteLine($"Logradouro: {endereco.Logradouro}");
                      Console.WriteLine(Environment.NewLine);
                }
             });
```

Você pode consultar mais sobre fluent [aqui](MosaicoSolutions.ViaCep/Fluent).

## Dependências

[Newtonsoft.Json](http://www.newtonsoft.com/json) 9.0.1 
