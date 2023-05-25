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
O aplicativo tem quatro terminais de API configurados no API Gateway para demonstrar os recursos com opções de segurança baseadas em token ativadas. Estas rotas estão expostas ao
