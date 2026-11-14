# CRM Inteligente

Sistema web de gerenciamento de relacionamento com clientes para pequenas e médias empresas.

## Tecnologias

- .NET 10, ASP.NET Core Web API
- Blazor WebAssembly
- Entity Framework Core e PostgreSQL

## Estrutura

- `src/CrmInteligente.Api` — API e hospedagem do frontend
- `src/CrmInteligente.Client` — frontend em Blazor WebAssembly
- `src/CrmInteligente.Shared` — contratos entre API e frontend
- `tests/` — testes unitários e de integração
- `docs/` — visão do projeto, roteiro, backlog, decisões e progresso

## Como executar

    dotnet build
    dotnet test
    