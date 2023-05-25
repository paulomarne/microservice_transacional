# Transação Simples: arquitetura de amostra de microsserviços para aplicativos .Net Core
### Exemplo do .Net Core 2.2 com C#.Net, EF e SQL Server
* [Introdução](#Introdução)
* [Arquitetura de aplicativos](#Arquitetura de aplicativos)
* [Desenho de Microsserviço](#Design-of-Microservice)
* [Segurança: autenticação baseada em token JWT](#Segurança--autenticação baseada em token JWT)
* [Ambiente de Desenvolvimento](#Ambiente de Desenvolvimento)
* [Tecnologias](#Tecnologias)
* [Ferramentas de código aberto usadas](#Ferramentas de código aberto usadas)
* [Serviços de plataforma em nuvem](#Serviços de plataforma em nuvem)
* [Design de banco de dados](#Design de banco de dados)
* [Pontos de extremidade da WebApi](#Pontos de extremidade da WebApi)
* [Estrutura da solução](#Estrutura-Solução)
* [Tratamento de exceção](#Manipulação de exceção)
* [Manipulação de simultaneidade Db](#Db-Concurrency-Handling)
* [Azure AppInsights: registro e monitoramento](#Azure-AppInsights-Logging-and-Monitoring)
* [Swagger: Documentação da API](#Swagger-API-Documentation)
* [Coleção Postman](#Postman-Collection)
* [Como executar o aplicativo](#How-to-run-the-application)
* [Console App - Gateway Client](#Console-App---Gateway-Client)
---
## Introdução
Este é um aplicativo de exemplo .Net Core e um exemplo de como criar e implementar um sistema de back-end baseado em microsserviços para um recurso bancário automatizado simples, como Saldo, Depósito, Retirada em ASP.NET Core Web API com C#.Net, Entity Framework e SQLServer.

## Arquitetura do aplicativo

O aplicativo de exemplo é construído com base na arquitetura de microsserviços. Existem várias vantagens na construção de um aplicativo usando a arquitetura de microsserviços, pois os serviços podem ser desenvolvidos, implantados e dimensionados independentemente. O diagrama abaixo mostra o design de alto nível da arquitetura de back-end.

- **Identity Microservice** - Autentica o usuário com base no nome de usuário, senha e emite um token JWT Bearer que contém informações de identidade baseadas em declarações.
- **Microsserviço de transação** - Lida com transações de conta como Obter saldo, depositar, sacar
- **API Gateway** - Atua como um ponto central de entrada para o aplicativo de back-end, fornece agregação de dados e caminho de comunicação para microsserviços.

![Arquitetura da Aplicação](https://8dmbiq.dm.files.1drv.com/y4mKz6TDtiwhrfo2mdUgvzle36Bnj7PMCvY6fP6kixwU3c3_CMb_rnnYOxg9WKn8LMmc5F__p2w3NWJc0o1vmCFmhHd5hRbr0S4MnMFnx09qvdSHE_E_40H0pQOxE0om2T2czVDOAInkTXn4xgdx_FmRgo8OaBh2XYqFHTf2zmYmF71tqRqlLzlsYBo1x1_CvdCt8U6AbjMhYznbgeBkGUKPQ?width=625&height=243&cropmode=none)

## Design de Microsserviço
Este diagrama mostra o design interno do microsserviço de transação. A lógica de negócios e a lógica de dados relacionadas ao serviço de transação são escritas em uma estrutura de processamento de transação separada. A estrutura recebe entrada via Web Api e processa essas solicitações com base em algumas regras simples. Os dados da transação são armazenados no banco de dados SQL.

![Projeto de microsserviço](https://8dk2lg.dm.files.1drv.com/y4md899yaH9aFP7Z1qhi_kCicZwQMYJWDA4SAdihporow8okXYUFcl-lp-2Awv5ldmlGmOEqwrxv3je-XaQqM7fnZZLzJKFzv7WDrC7Hyd2QLLglJfjNhWaFiCRJXzaXjghqK8y1XZJUuHAJiVdfl3_90NuPyNV-zsb5UOKBpRBbeFx3LpI0gPivXhIRBtFq6ZdInV5ub8r5U-Ibw9Zb-0YzQ?width=631&height=617&cropmode=none)



## Segurança: autenticação baseada em token JWT
A autenticação baseada em token JWT é implementada para proteger os serviços WebApi. **Identity Microservice** atua como um servidor Auth e emite um token válido após validar as credenciais do usuário. O API Gateway envia o token para o cliente. O aplicativo cliente usa o token para a solicitação subsequente.

![Segurança baseada em token JWT](https://h9yrga.dm.files.1drv.com/y4mCbiAcoeieS5tBZu_z1z1z42C8eoVGWUmC_re1VkLWpxWtywvsOBH73brVXA4gzKm6G59h3b3vbUVF1C3jbYRlpf-7t-faZE4m8-wYplZusss5Fm-71AH87c1aXlKoULtFoUNl5Oh9h6nZDDfgLXeo_LKOH8Q0b4BGVTpg1w7TcCZQPkX5tBZtSiQj67JGqsg4lySz2ghzB9R9ArGtaA7wA?width=702&height=422&cropmode=none)

## Ambiente de desenvolvimento

- [.Net Core 2.2 SDK](https://dotnet.microsoft.com/download)
- [Visual Studio .Net 2017](https://visualstudio.microsoft.com/downloads/)
- [SQL Server Management Studio 17.9.1](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

## Tecnologias
- C#.NET
- Núcleo da API WEB ASP.NET
- Servidor SQL

## Ferramentas de código aberto usadas
- Automapper (para mapeamento objeto a objeto)
- Entity Framework Core (para acesso a dados)
- Swashbucke (para documentação da API)
- XUnit (para caso de teste de unidade)
- Ocelot (para API Gateway Aggregation)

## Serviços de plataforma em nuvem
- Azure App Insights (para log e monitoramento)
- Banco de dados SQL do Azure (para armazenamento de dados)

## Projeto de banco de dados
![Projeto de banco de dados](https://8dmprw.dm.files.1drv.com/y4mFOnWFyfydK_fr_Cm6EA0spR2Ms8gG4XHVs01AO_pJ04-rHLI_GL6gWbEcXL2FIIDJN4AmAHs4K7o_BzO22oAuTP1hyT1wJdeOx_5kRI9J0t-XLRSkIHenLo0n7xbEcanL2luIHM16WawZ5__xJmMATY5YK8O8UT8P8W8CBp_bWnbsxcndOtUBN4E34tJHGqNb1GC2-hCW2fKKkHFMXo56w?width=1226&height=428&cropmode=none)

## Pontos de extremidade da WebApi
O aplicativo tem quatro terminais de API configurados no API Gateway para demonstrar os recursos com opções de segurança baseadas em token ativadas. Essas rotas são expostas ao aplicativo cliente para consumir os serviços de back-end.

### End-points configurados e acessíveis por meio do API Gateway

1. Rota: **"/user/authenticate"** [HttpPost] - Para autenticar o usuário e emitir um token
2. Rota: **"/account/balance"** [HttpGet] - Para recuperar o saldo da conta.
3. Rota: **"/account/deposit"** [HttpPost] - Para depositar valor em uma conta.
4. Rota: **"/account/withdraw"** [HttpPost] - Para retirar o valor de uma conta.

### End-points implementados no nível do microsserviço

1. Rota: **"/api/user/authenticate"** [HttpPost]- Para autenticar o usuário e emitir um token
2. Rota: **"/api/account/balance"** [HttpGet]- Para recuperar o saldo da conta.
3. Rota: **"/api/account/deposit"** [HttpPost]- Para depositar valor em uma conta.
4. Rota: **"/api/account/withdraw"** [HttpPost]- Para retirar o valor de uma conta.

## Estrutura da solução
![Estrutura da solução](https://h9x1lw.dm.files.1drv.com/y4mWm9vkzmf7oSUvJGiXl7JvkGz2FFOPs2-A3d2qH1PieYR4Wb55NseX4yOFl5igZTB5gzLW3guxaBqyyuhk6cyHEVjVy3vHt_cYHzwGg6jKvAPW2HN1evRmFWZkSii082F00RBDUozTBslIzMH6wEgyRWJmNpb14WEYWOZjBV7SG4WdkC9RZ7DKvGDNHoiBFctU9b3yEZnAo-RhL2Hm-ED-g?width=448&height=940&cropmode=none)

- **Identity.WebApi**
     - Manipula a parte de autenticação usando nome de usuário, senha como parâmetro de entrada e emite um token JWT Bearer com informações de identidade de declarações nele.
- **Transaction.WebApi**
     - Suporta três métodos http 'Balance', 'Deposit' e 'Withdraw'. Recebe solicitação http para esses métodos.
     - Lida com exceção através de um middleware
     - Lê as informações de identidade do cabeçalho de autorização que contém o token do portador
     - Chama a função apropriada na estrutura 'Transação'
     - Retorna o resultado da resposta da transação para o cliente
- **Transaction.Framework**
     - Define a interface para a camada de repositório (dados) e camada de serviço (negócios)
     - Define o modelo de domínio (objetos de negócios) e o modelo de entidade (modelo de dados)
     - Define as exceções de negócios e a validação do modelo de domínio
     - Define os tipos de dados necessários para o framework 'Struct', 'Enum', 'Consants'
     - Implementa a lógica de negócios para realizar as transações de conta necessárias
     - Implementa a lógica de dados para ler e atualizar os dados de e para o banco de dados SQL
     - Executa a tarefa de mapear o modelo de domínio para o modelo de entidade e vice-versa
     - Lida com o conflito de simultaneidade de atualização do banco de dados
     - Cadastra as Interfaces e sua Implementação na Coleta de Serviços através de injeção de dependência
- **Gateway.WebApi**
     - Valida a solicitação Http recebida verificando o token JWT autorizado.
     - Reencaminhar o pedido Http para um serviço downstream.
- **SimpleBanking.ConsoleApp**
     - Um aplicativo cliente de console que se conecta ao Api Gateway, pode ser usado para fazer login com nome de usuário, senha e realizar transações como 'Saldo', 'Depósito' e 'Retirada' em uma conta.

## Manipulação de exceção
Um Middleware é escrito para lidar com as exceções e é registrado na inicialização para ser executado como parte da solicitação http. Cada solicitação http passa por esse middleware de manipulação de exceção e, em seguida, executa o método de ação do controlador da API da Web.

* Se o método de ação for bem-sucedido, a resposta de sucesso será enviada de volta ao cliente.
* Se qualquer exceção for lançada pelo método de ação, a exceção será capturada e tratada pelo Middleware e a resposta apropriada será enviada de volta ao cliente.

![Middleware do manipulador de exceção](https://h9wqda.dm.files.1drv.com/y4mgc5I1iveH8tv63QAu-nSpHVmAAHNFMb9J4KRpywPRZsM7orJiFBKAKEG-wV9r1-Ox7gsODTJZFlnMajsyedcfccUWU25GTswug3z47cr9S4itzbCkCuSG_SHVhZG91uwxvQMLnhg6TaHwOwvBrKrTI3XMzLt86TwjZHyKw4ow6vZ5372OenRnyOtfUFhFtbzThwKD2V3N2GX9v8DrLDJZw?width=431&height=371&cropmode=none)

```
Tarefa assíncrona pública InvokeAsync (contexto HttpContext, RequestDelegate a seguir)
{
     tentar
     {
         aguarde próximo(contexto);
     }
     pegar (Exceção ex)
     {
         var mensagem = CreateMessage(contexto, ex);
         _logger.LogError(mensagem, ex);

         aguarde HandleExceptionAsync(contexto, ex);
     }
}
```
## Manipulação de simultaneidade de banco de dados

A simultaneidade do banco de dados está relacionada a um conflito quando várias transações tentam atualizar os mesmos dados no banco de dados ao mesmo tempo. No diagrama abaixo, se você perceber que a Transação 1 e a Transação 2 estão na mesma conta, uma tentando depositar o valor na conta e o outro sistema tentando Retirar o valor da conta ao mesmo tempo. A estrutura contém duas camadas lógicas, uma lida com a lógica de negócios e a outra com a lógica de dados.

![Exceção de atualização de simultaneidade do banco de dados](https://h9wjxq.dm.files.1drv.com/y4mQaKTurkARMHvgn2Btinv1zGVwyGPEiDN8n2hoiNkj6dqFsrkVRBD1ivMGlyCw_wyS2uIB5acPPZHGGJfYMZHtKJ7_ipf7Ltj--wdVJmQYiZP3tsR7CSEq_JVTz8eoA86pBrmy6aYT6quacE5PReiczk71Q11NUjnGSnbuN_0LuvheU5L2ns0scFeEBy3Szu9IoUq-uvDVUJ2Fagj8Ftlsw?width=921&height=536&cropmode=none)

Quando um dado é lido do BD e quando a lógica de negócio é aplicada aos dados, neste contexto, haverá três estados diferentes para os valores relativos ao mesmo registro.

- **Valores do banco de dados** são os valores atualmente armazenados no banco de dados.
- **Valores originais** são os valores que foram originalmente recuperados do banco de dados
- **Valores atuais** são os novos valores que o aplicativo está tentando gravar no banco de dados.

O estado dos valores em cada uma das transações produz um conflito quando o sistema tenta salvar as alterações e identifica usando o token de concorrência que os valores que estão sendo atualizados no banco de dados não são os valores originais que foram lidos do banco de dados e lança DbUpdateConcurrencyException .

[Referência: docs.microsoft.com](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

A abordagem geral para lidar com o conflito de simultaneidade é:

1. Capture **DbUpdateConcurrencyException** durante SaveChanges
2. Use **DbUpdateConcurrencyException.Entries** para preparar um novo conjunto de alterações para as entidades afetadas.
3. **Atualize os valores originais** do token de simultaneidade para refletir os valores atuais no banco de dados.
4. **Repita o processo** até que nenhum conflito ocorra.

```
while (! isSaved)
{
     tentar
     {
         aguarde _dbContext.SaveChangesAsync();
         isSaved = verdadeiro;
     }
     catch (DbUpdateConcurrencyException ex)
     {
         foreach (var entrada em ex.Entries)
         {
             if (entry.Entity é AccountSummaryEntity)
             {
                 var databaseValues = entrada.GetDatabaseValues();

                 if (databaseValues != null)
                 {
                     entrada.OriginalValues.SetValues(databaseValues);
                     CalculateNewBalance();

                     void CalculateNewBalance()
                     {
                         var saldo = (decimal)entry.OriginalValues["Saldo"];
                         var valor = accountTransactionEntity.Amount;

                         if (accountTransactionEntity.TransactionType == TransactionType.Deposit.ToString())
                         {
                             accountSummaryEntity.Balance =
                             saldo += valor;
                         }
                         else if (accountTransactionEntity.TransactionType == TransactionType.Withdrawal.ToString())
                         {
                             if(valor > saldo)
                                 lançar novo InsufficientBalanceException();

                             accountSummaryEntity.Balance =
                             saldo -= montante;
                         }
                     }
                 }
                 outro
                 {
                     lançar novo NotSupportedException();
                 }
             }
         }
     }
}
```

## Azure AppInsights: log e monitoramento

Azure AppInsights integrado ao "Microsserviço de Transação" para coletar a Telemetria do aplicativo.

```
public void ConfigureServices(serviços IServiceCollection)
{
    services.AddApplicationInsightsTelemetry(Configuration);
}
```

AppInsights SDK para Asp.Net Core fornece um método de extensão AddApplicationInsights em ILoggerFactory para configurar o log. Todas as transações relacionadas a depósito e retirada são registradas por meio do ILogger nos logs do AppInsights.

```
public void Configure (aplicativo IApplicationBuilder, IHostingEnvironment env, log ILoggerFactory)
{
    log.AddApplicationInsights(app.ApplicationServices, LogLevel.Information);
}
```

Para usar o AppInsights, você precisa ter uma conta do Azure e criar uma instância do AppInsights no Portal do Azure para seu aplicativo, que fornecerá uma chave de instrumentação que deve ser configurada no appsettings.json

```
  "Insights de aplicativos": {
     "InstrumentationKey": "<Sua chave de instrumentação>"
   },
```
---
## Swagger: Documentação da API

Pacote Swashbuckle Nuget adicionado ao "Transaction Microservice" e Swagger Middleware configurado no startup.cs para documentação da API. ao executar o serviço WebApi, a IU do swagger pode ser acessada por meio do endpoint swagger "/swagger".

```
public void ConfigureServices(serviços IServiceCollection)
{
      services.AddSwaggerGen(c => {
         c.SwaggerDoc("v1", new Info { Title = "Simple Transaction Processing", Version = "v1" });
      });
}
```

```
public void Configure (aplicativo IApplicationBuilder, IHostingEnvironment env, log ILoggerFactory)
{
      app.UseSwagger();
      app.UseSwaggerUI(c => {
         c.SwaggerEndpoint("/swagger/v1/swagger.json", "Simple Transaction Processing v1");
      });
}
```

! [Swagger api doc] (https://h9wqrg.dm.files.1drv.com/y4m_QKAwbWqvjgqVire-cVSue_EKgCJsNEwUca9MztPMbsMlDBmFtaIQbllXjiLVwZCJnittfbHBYQ3DiQwnyZrJ2Tz_qf8Paqms91ZVgWUm8_Wcc9b39y9_PbYSxtQ0KhA5u9E4FBkg6aNDgL207ZODhVQl8UgQyYpAV1II_UVmSnEfD5SionZ0kwbSZCnsMyv7kK-e7ym00Nk5sjbvlODiw?width=1892&height=874&cropmode=none)

---
## Coleção do Postman

Baixe a coleção do Postman [aqui](https://1drv.ms/u/s!AshsJMsiN-avgQCgr5NIFpZCoM5r) para executar os endpoints da API por meio do gateway

![Postman](https://h9ykoq.dm.files.1drv.com/y4mROgcn_on2fHOuDePdeG-CfHybIpOETMHlCfLqoBJIUqLa-PFKk8InJ2Ei6XLkHyrS7dXtF0htZQnHyjFX3fZ-lDHAi93QY568uJRbPuJGHBANeBpd3rt9A4iBGCXf3akpCHEumuB7kLPSILOww_U6HFHp5UBF8aylwj5DcCSxJPt161RfVKDFKjyNuK5yxYqc--M9XbTTJ12mqZL-dtrbA?width=1898&height=1026&cropmode=none)

## Como executar o aplicativo

1. Baixe o script Sql [aqui](https://1drv.ms/u/s!AshsJMsiN-avgQILUXHhHHp1oOVa),
2. Execute o script no servidor SQL para criar as tabelas necessárias e os dados de amostra
3. Abra a solução (.sln) no Visual Studio 2017 ou versão posterior
4. Configure a string de conexão SQL em Transaction.WebApi -> Appsettings.json file
5. Configure a chave de instrumentação do AppInsights em Transaction.WebApi -> arquivo Appsettings.json. Se você não tiver uma chave ou não precisar de logs, comente o código relacionado ao AppInsight no arquivo Startup.cs
6. Verifique o arquivo Identity.WebApi -> UserService.cs para informações de identidade. Os detalhes do usuário são codificados para algumas contas no serviço de identidade, que podem ser usadas para executar o aplicativo. Mesmos detalhes mostrados na tabela abaixo.
7. Execute os seguintes projetos na solução
     - Identity.WebApi
     - Transação.WebApi
     - Gateway.WebApi
     - SimpleBanking.ConsoleApp
8. O host e a porta do gateway devem ser configurados corretamente no ConsoleApp
9. O host e a porta do serviço de identidade e transação devem ser configurados corretamente no gateway -> configuration.json

- Dados de amostra para testar

| Número da conta | Moeda | Nome de usuário | Senha |
| -------- | -------- | -------- | -------- |
| 3628101 | EUR | espectro | teste@123 |
| 3637897 | EUR | gwoodhouse| pass@123 |
| 3648755 | EUR | jsmith | admin@123 |


## Aplicativo de Console - Cliente Gateway


![Console App](https://ulccxq.dm.files.1drv.com/y4m672RTTTxmbasRn2Ve8aElEASEJ282kImiYfISbt541JIB9x-LzJGYMWgFwRqGPA7F0OZEuR4OCJ4Vs6JhcSoy1min8jZQ5jjqeW2J_Ayd50dVYFh20qHCXQsuSe6lPuBk1MgiEDnaf7dOdXihgxi-qfbv2Ezg83g_g2hZOHnsi8kO4XMJyOEIFjILi4bkN4OTz1cuXZwNWfFY_VUQc9iNw?width=812&height=939&cropmode=none)
