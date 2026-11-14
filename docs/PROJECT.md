# PROJECT — CRM Inteligente

Última atualização: 2026-11-09

## Visão geral

Sistema web de gerenciamento de relacionamento com clientes para pequenas e médias empresas. Organiza clientes, contatos, oportunidades, tarefas, interações e indicadores de vendas em um só lugar.

## Objetivo

Produto comercial, destinado à venda. Começa com um MVP funcional para uma única empresa, com evolução futura para SaaS.

## Contexto de desenvolvimento

- Equipe: um desenvolvedor, 5 horas por dia
- Prazo do MVP: 1 mês (110 a 150 horas disponíveis)
- Orçamento de infraestrutura: zero, se possível
- Primeiro usuário real: ainda não existe

## Escopo do MVP

| # | Módulo | Funcionalidades |
|---|---|---|
| M1 | Acesso | Login, três perfis fixos, administrador cria, edita e desativa usuários e redefine senhas |
| M2 | Clientes e contatos | Cadastro único com tipo (pessoa física ou jurídica), contatos vinculados, busca e filtros |
| M3 | Funil | Oportunidades com valor, responsável e etapa; funil único com etapas editáveis pelo administrador; visão em quadro; fechamento como ganha ou perdida com motivo |
| M4 | Atividades | Tarefas com prazo e responsável; interações (nota, ligação, reunião) ligadas a cliente ou oportunidade; linha do tempo |
| M5 | Indicadores | Valor em aberto por etapa, taxa de conversão, vendas ganhas no período, tarefas vencidas |

## Perfis e visibilidade

| Perfil | Acesso |
|---|---|
| Administrador | Vê tudo, gerencia usuários e configurações |
| Gestor | Vê tudo da equipe e os indicadores de todos; não gerencia usuários |
| Vendedor | Vê todos os clientes e contatos; só as próprias oportunidades e tarefas |

## Fora do MVP

- Logo após o MVP: importação de clientes por planilha, recuperação de senha por e-mail, exportação de relatórios
- Futuro: vários funis, campos personalizados, permissões configuráveis, notificações, integrações com e-mail e WhatsApp, propostas, automações
- Na virada para SaaS: cadastro de novas empresas pelo próprio cliente, planos e cobrança
- Etapa 7: funcionalidades de IA (resumo de interações, próximas ações, classificação de leads, assistente comercial)

## Arquitetura atual

Stack aprovada em 2026-11-09 (D010 a D013):

| Camada | Tecnologia |
|---|---|
| Plataforma | .NET 10 (LTS), C# |
| Backend | ASP.NET Core Web API |
| Frontend | Blazor WebAssembly, publicado junto com a API |
| ORM | Entity Framework Core |
| Banco de dados | PostgreSQL, hospedado no Neon (plano gratuito) |
| Hospedagem | Render (plano gratuito), publicação com Docker |
| Documentação de API | OpenAPI |
| Versionamento | Git |

Organização aprovada em 2026-11-09 (D014 a D022): monólito modular simples.

```
CrmInteligente.sln
Dockerfile
docs/                              PROJECT.md, ROADMAP.md, BACKLOG.md, DECISIONS.md, PROGRESS.md
src/
  CrmInteligente.Api/              ASP.NET Core Web API, também serve o Blazor
    Modules/
      Access/                      controllers, serviços e validações de cada módulo
      Customers/
      Pipeline/
      Activities/
      Dashboard/
    Domain/                        entidades e regras de negócio
    Data/                          DbContext, mapeamentos e migrações
    Infrastructure/                autenticação, tratamento de erros, logs
  CrmInteligente.Client/           Blazor WebAssembly
    Pages/
    Components/
    Services/                      chamadas à API
  CrmInteligente.Shared/           contratos entre API e Client
tests/
  CrmInteligente.UnitTests/
  CrmInteligente.IntegrationTests/
```

Regras da arquitetura:

1. O projeto Shared contém só contratos; entidades nunca saem da API.
2. Serviços usam o DbContext diretamente, sem repositórios genéricos nem bibliotecas de mediação.
3. Regras de negócio ficam nas entidades e nos serviços, nunca em controllers ou páginas.
4. Um módulo não acessa as pastas internas de outro; usa o serviço do outro módulo.
5. API com controllers.
6. Todas as tabelas de negócio têm CompanyId, aplicado por filtro global do EF Core.
7. SQL escrito à mão é proibido no MVP, salvo decisão registrada.
8. Código em inglês, interface em português.

## Modelo de dados

Aprovado em 2026-11-09 (D023 a D031). Todas as tabelas têm Id (UUID), CreatedAt e UpdatedAt; todas, exceto Company, têm CompanyId.

| Tabela | Campos principais | Relacionamentos |
|---|---|---|
| Company | Name | Tem usuários, clientes e etapas |
| User | Name, Email (único), PasswordHash, Role, IsActive | Pertence a uma empresa |
| Customer | Type (PF ou PJ), Name, TradeName, Document, Email, Phone, IsArchived | Responsável: OwnerUserId; tem contatos, oportunidades e interações |
| Contact | Name, JobTitle, Email, Phone, IsPrimary | Pertence a um cliente |
| PipelineStage | Name, Order, IsActive | Contém oportunidades |
| Opportunity | Title, Value, ExpectedCloseDate, Status, LostReason, ClosedAt | Cliente, contato (opcional), etapa, responsável |
| StageHistory | ChangedAt | Oportunidade, etapa de origem (opcional), etapa de destino, autor |
| TaskItem | Title, DueDate, CompletedAt | Responsável; cliente e oportunidade opcionais |
| Interaction | Type (nota, ligação, reunião), Content, OccurredAt | Autor, cliente; oportunidade opcional |

Regras de integridade:

- Cliente é arquivado e usuário é desativado, nunca excluídos.
- Etapa só é excluída se não tiver oportunidades; caso contrário, é desativada.
- Contato pode ser excluído; oportunidades que o citavam ficam sem contato.
- Tarefa pode ser excluída pelo responsável, pelo gestor e pelo administrador.
- Interação não é excluída no MVP; o autor pode editar.
- Documento é opcional, validado e único dentro da empresa quando preenchido.
- E-mail de usuário é único em todo o sistema; nome de etapa é único dentro da empresa.
- Fechar como perdida exige motivo; no máximo um contato principal por cliente.
- StageHistory só recebe inclusões.

Fora do modelo no MVP: endereço do cliente e lista fixa de motivos de perda.

## Autenticação e segurança

Aprovado em 2026-11-09 (D032 a D034).

- ASP.NET Core Identity com cookie inacessível a JavaScript, só por HTTPS, restrito ao próprio site e com proteção contra requisições forjadas.
- A tabela User herda do usuário do Identity; o perfil fica no campo Role.
- Senha de no mínimo 8 caracteres; bloqueio temporário após 5 tentativas erradas; limite de tentativas de login por minuto.
- Usuários criados só pelo administrador, com senha temporária e troca obrigatória no primeiro acesso.
- Primeiro administrador criado na primeira execução, a partir de variáveis de ambiente.
- Autorização sempre verificada na API, por perfil e por dono do registro.
- Isolamento por CompanyId com filtro global.
- Segredos em variáveis de ambiente e nos segredos locais do .NET; nada no repositório.
- Validação declarada uma vez no projeto Shared e aplicada no Blazor e na API.
- Respostas de erro padronizadas, sem detalhes internos; logs sem senhas, documentos ou conteúdo de interações.

Fora do MVP: recuperação de senha por e-mail, auditoria completa, adequação integral à LGPD (P010).

## Testes

Aprovado em 2026-11-09 (D035).

| Tipo | Ferramenta | O que cobre |
|---|---|---|
| Unitário | xUnit | Regras de negócio |
| Integração | xUnit com a API em memória e PostgreSQL real em contêiner Docker | Permissões por perfil, isolamento entre empresas, unicidade, fluxo de cada módulo |
| Interface | Roteiro manual por módulo | Telas do Blazor |
| Automação | GitHub Actions | Compilação e testes a cada envio |

Toda regra de negócio e toda regra de permissão tem pelo menos um teste. Repositório privado no GitHub (D036).

Limitações conhecidas do ambiente gratuito: a aplicação adormece após 15 minutos sem acesso e leva cerca de um minuto para voltar; o banco suspende após 5 minutos parado. Serve para validar, não para clientes pagantes.
