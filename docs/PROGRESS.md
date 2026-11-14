# PROGRESS — CRM Inteligente

Última atualização: 2026-11-09

## Situação atual

- Etapa atual: 3 — Fundação (em andamento, 0 de 6 tarefas concluídas)
- Tarefa atual: 3.1 — Criação da solução (em andamento; comandos entregues, aguardando execução e resultado)
- Ambiente de desenvolvimento: Windows, Visual Studio, .NET SDK 10.0.401
- Marco MV1 atingido: arquitetura aprovada e registrada em DECISIONS.md
- Horas consumidas do MVP: 0 de 128 estimadas
- Código produzido: nenhum

## Tarefas concluídas

| Data | Tarefa | Resultado |
|---|---|---|
| 2026-11-09 | 0.1 — Visão inicial e primeira rodada de perguntas | Visão do produto e riscos iniciais apresentados |
| 2026-11-09 | 0.2 — Consolidação dos requisitos | Requisitos, perfis e funil aprovados |
| 2026-11-09 | 1.1 — Escopo, critérios de aceitação e backlog | MVP (M1 a M5), cortes e backlog aprovados |
| 2026-11-09 | 1.2 — Documentação inicial | PROJECT, ROADMAP, BACKLOG, DECISIONS e PROGRESS criados |
| 2026-11-09 | 2.1 — Comparação e escolha da stack | .NET 10, Blazor WebAssembly servido pela API, PostgreSQL no Neon, Render com Docker (D010 a D013) |
| 2026-11-09 | 2.2 — Arquitetura e organização da solução | Monólito modular com três projetos e dois de teste, regras da arquitetura, CompanyId desde o início, código em inglês (D014 a D022) |
| 2026-11-09 | 2.3 — Modelagem de entidades e relacionamentos | Nove tabelas, cadastro único de clientes, regras de integridade (D023 a D031) |
| 2026-11-09 | 2.4 — Autenticação, segurança e testes | Identity com cookie, medidas de segurança, estratégia de testes, repositório privado (D032 a D036). Etapa 2 concluída |

## Problemas e riscos conhecidos

| # | Descrição | Impacto | Tratamento |
|---|---|---|---|
| R1 | Não existe primeiro usuário real | Critérios de aceitação validados só pelo desenvolvedor; risco comercial alto | Procurar uma empresa para testar durante o desenvolvimento |
| R2 | Margem de prazo apertada (128 h estimadas contra 110 a 150 disponíveis) | Atraso provável se alguma estimativa falhar | Cortes definidos em D008; rever estimativas ao fim de cada módulo |
| R3 | Estimativas ainda não testadas no ritmo real | Planejamento pode estar otimista | Comparar horas previstas e gastas a partir da Etapa 3 |
| R4 | Ambiente gratuito: aplicação adormece após 15 minutos e leva cerca de um minuto para voltar | Inaceitável para cliente pagante, ruim em demonstração | Planejar US$ 7/mês no Render quando houver um interessado real |
| R6 | Instância gratuita do Render tem 0,1 CPU e 512 MB | Desempenho do .NET nesse tamanho ainda não medido | Medir na Etapa 3, ao publicar a primeira versão |
| R7 | Limites dos planos gratuitos mudam; fontes divergem sobre o armazenamento do Neon (0,5 GB ou 1 GB) | Planejamento pode ficar desatualizado | Conferir no painel ao criar as contas |
| R5 | MVP para uma empresa em um produto que será SaaS | Retrabalho na virada para várias empresas | Mitigado por D020 (CompanyId e filtro global); implementar e testar na Etapa 3 |
| R8 | O CRM guarda dados pessoais e a adequação à LGPD não está no MVP | Obrigações legais para um produto à venda | Resolver P010 antes da primeira venda; consultar advogado |
| R9 | Sem recuperação de senha por e-mail; se o único administrador esquecer a senha, a recuperação é manual | Suporte manual pelo desenvolvedor | Recuperação por e-mail é o segundo item pós-MVP |

## Ressalvas registradas

Nenhuma.
