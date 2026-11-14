# BACKLOG — CRM Inteligente

Última atualização: 2026-11-09

Estados: Pendente, Em andamento, Bloqueada, Em validação, Concluída

## Itens do MVP

| Ordem | Item | Depende de | Estimativa | Estado |
|---|---|---|---|---|
| 1 | Etapa 2 — Arquitetura | — | 8 h | Concluída |
| 2 | Etapa 3 — Fundação | 1 | 14 h | Em andamento |
| 3 | M1 — Acesso | 2 | 16 h | Pendente |
| 4 | M2 — Clientes e contatos | 3 | 20 h | Pendente |
| 5 | M3 — Funil | 4 | 24 h | Pendente |
| 6 | M4 — Atividades | 4, 5 | 16 h | Pendente |
| 7 | M5 — Indicadores | 5, 6 | 10 h | Pendente |
| 8 | Etapa 5 — Qualidade e integração | 3 a 7 | 12 h | Pendente |
| 9 | Etapa 6 — Publicação | 8 | 8 h | Pendente |
| | Total | | 128 h | |

## Critérios de aceitação por módulo

| Módulo | Critério |
|---|---|
| M1 | Cada perfil só acessa o que a regra de visibilidade permite, inclusive chamando a API diretamente |
| M2 | Cadastro de PF e PJ com validação de CPF/CNPJ e bloqueio de documento duplicado |
| M3 | Vendedor move só as próprias oportunidades; toda mudança de etapa fica registrada com data e autor |
| M4 | A página do cliente mostra todo o histórico em ordem cronológica; tarefas vencidas aparecem destacadas |
| M5 | Gestor vê os números da equipe e por vendedor; vendedor vê só os próprios; os valores batem com os dados cadastrados |

## Tarefas da Etapa 3

| # | Tarefa | Depende de | Estimativa | Estado |
|---|---|---|---|---|
| 3.1 | Criar a solução, os cinco projetos, as referências e o repositório Git | — | 1,5 h | Em andamento |
| 3.2 | API servindo o Blazor (referência Api → Client e pacote de hospedagem), com uma página inicial e um endpoint de verificação, rodando localmente | 3.1 | 2 h | Pendente |
| 3.3 | PostgreSQL local em Docker, EF Core, DbContext, tabela Company, filtro global de CompanyId e primeira migração | 3.2 | 3,5 h | Pendente |
| 3.4 | Infraestrutura: tratamento de erros, logs, validação e OpenAPI | 3.3 | 2,5 h | Pendente |
| 3.5 | Base de testes: xUnit, PostgreSQL em contêiner, teste de isolamento e GitHub Actions | 3.3 | 2,5 h | Pendente |
| 3.6 | Dockerfile e primeira publicação no Render com banco no Neon; medir desempenho | 3.4, 3.5 | 2 h | Pendente |

## Tarefas da Etapa 2

| # | Tarefa | Estado |
|---|---|---|
| 2.1 | Comparar alternativas de stack (frontend, banco, hospedagem) e decidir | Concluída |
| 2.2 | Definir arquitetura e organização da solução | Concluída |
| 2.3 | Modelar entidades e relacionamentos | Concluída |
| 2.4 | Definir estratégias de autenticação, segurança e testes | Concluída |

## Pós-MVP (não priorizado em detalhe)

| Prioridade | Item |
|---|---|
| Alta | Importação de clientes por planilha |
| Alta | Recuperação de senha por e-mail |
| Média | Exportação de relatórios |
| A definir | Vários funis, campos personalizados, permissões configuráveis, notificações |
| A definir | Integrações com e-mail e WhatsApp, propostas, automações |
| A definir | SaaS: cadastro de empresas, planos e cobrança |
| A definir | Funcionalidades de IA |
