# DECISIONS — CRM Inteligente

Última atualização: 2026-11-09

## Decisões aprovadas

| # | Data | Decisão | Justificativa |
|---|---|---|---|
| D001 | 2026-11-09 | O projeto é um produto comercial, para venda | Definição do responsável pelo projeto |
| D002 | 2026-11-09 | O MVP atende uma única empresa | Reduz o escopo para caber em um mês; SaaS fica para depois |
| D003 | 2026-11-09 | Três perfis fixos: Administrador, Gestor e Vendedor, sem permissões configuráveis | Cobre a hierarquia comum em PMEs sem o custo de um sistema de permissões |
| D004 | 2026-11-09 | Clientes e contatos visíveis a toda a equipe; oportunidades e tarefas do vendedor visíveis só a ele, ao gestor e ao administrador | Esconder cadastros gera duplicidade; a informação sensível está nas negociações |
| D005 | 2026-11-09 | Funil único com etapas editáveis pelo administrador, criado com um conjunto padrão | Atende empresas com processos diferentes sem a complexidade de vários funis |
| D006 | 2026-11-09 | O sistema atende B2B e B2C | Definição do responsável; forma do cadastro a detalhar na Etapa 2 |
| D007 | 2026-11-09 | Escopo do MVP: módulos M1 a M5, conforme PROJECT.md | Corte necessário para o prazo de um mês com um desenvolvedor |
| D008 | 2026-11-09 | Ordem de cortes se o prazo apertar: quadro vira lista; indicadores caem para dois | Preserva o ciclo comercial completo e sacrifica apenas conforto de uso |
| D009 | 2026-11-09 | IA fica fora do MVP, a avaliar na Etapa 7 | Depende de histórico de interações estruturado e de priorização |
| D010 | 2026-11-09 | Plataforma: .NET 10 | LTS com suporte até novembro de 2028; .NET 8 e 9 perdem suporte em 10/11/2026; .NET 11 é STS e recém-lançada |
| D011 | 2026-11-09 | Frontend: Blazor WebAssembly publicado junto com a API, em um único serviço | Mesma linguagem do backend, reuso de classes e validações, um só deploy e sem CORS; menor risco para o prazo. Blazor Server descartado por depender de conexão permanente |
| D012 | 2026-11-09 | Banco de dados: PostgreSQL, hospedado no Neon (plano gratuito) | Limite de computação gratuito bem maior que o do Azure SQL para uso diário; sem licença ao crescer; passagem para plano pago sem migração |
| D013 | 2026-11-09 | Hospedagem da aplicação: Render (plano gratuito), publicação com Docker | Não exige cartão; saída paga barata (US$ 7/mês) elimina a suspensão. O banco não fica no Render porque o Postgres gratuito de lá expira em 30 dias |
| D014 | 2026-11-09 | Arquitetura: monólito modular simples, com três projetos (Api, Client, Shared) e dois de teste (UnitTests, IntegrationTests), organizado por módulo de negócio | Menor custo inicial e menor risco para o prazo; módulos isolados podem ser extraídos depois. Clean Architecture consumiria cerca de 10% das horas em estrutura |
| D015 | 2026-11-09 | O projeto Shared contém só contratos (objetos de entrada e saída, enumerações, validações); entidades nunca saem da API | Evita vazar campos internos e impede alteração indevida de campos pelo cliente |
| D016 | 2026-11-09 | Serviços usam o DbContext diretamente, sem repositórios genéricos nem bibliotecas de mediação | O EF Core já cumpre esse papel; a camada extra duplicaria código |
| D017 | 2026-11-09 | Regras de negócio ficam nas entidades e nos serviços, nunca em controllers ou páginas | Separação de responsabilidades e testabilidade |
| D018 | 2026-11-09 | Um módulo não acessa as pastas internas de outro; usa o serviço do outro módulo | Baixo acoplamento entre módulos |
| D019 | 2026-11-09 | API com controllers, não Minimal APIs | Organização por módulo e regras de perfil mais explícitas |
| D020 | 2026-11-09 | Tabela de empresas com um registro e coluna CompanyId em todas as tabelas de negócio, com filtro global do EF Core e teste de isolamento | Custa 3 a 4 horas agora e evita reescrever banco e consultas na virada para SaaS (resolve P001) |
| D021 | 2026-11-09 | SQL escrito à mão é proibido no MVP, salvo decisão registrada | O filtro global de CompanyId não cobre SQL manual |
| D022 | 2026-11-09 | Código em inglês, interface em português; termos como CPF e CNPJ permanecem | Padrão do mercado e coerência com os nomes do .NET |
| D023 | 2026-11-09 | Modelo de dados com nove tabelas: Company, User, Customer, Contact, PipelineStage, Opportunity, StageHistory, TaskItem, Interaction; todas com CreatedAt e UpdatedAt | Cobre os módulos M1 a M5 sem tabelas além do necessário |
| D024 | 2026-11-09 | Cadastro único de clientes (Customer) com campo Type para pessoa física ou jurídica; pessoa física é um cliente sem contatos | Duas tabelas dobrariam telas e complicariam as oportunidades (resolve P002) |
| D025 | 2026-11-09 | Documento (CPF/CNPJ) opcional, validado e único dentro da empresa quando preenchido | Lead novo raramente tem documento; obrigar leva a dados inventados |
| D026 | 2026-11-09 | Situação da oportunidade (aberta, ganha, perdida) separada da etapa; a oportunidade fechada permanece na etapa em que estava | Permite medir em qual etapa as vendas se perdem |
| D027 | 2026-11-09 | Histórico de etapas em tabela própria (StageHistory), somente inclusão | Base da taxa de conversão e do tempo por etapa |
| D028 | 2026-11-09 | Tarefas (TaskItem) e interações (Interaction) em tabelas separadas; interação sempre ligada a um cliente e opcionalmente a uma oportunidade | Ciclos de vida diferentes; histórico completo por cliente serve à IA futura |
| D029 | 2026-11-09 | Identificadores UUID ordenados por tempo; valores em decimal(18,2) em moeda única (real); datas em UTC | Não expõem contagem de registros nem colidem entre empresas |
| D030 | 2026-11-09 | Regras de integridade: cliente é arquivado e usuário é desativado, nunca excluídos; etapa só é excluída sem oportunidades; interação não é excluída no MVP; e-mail de usuário único no sistema; nome de etapa único na empresa; motivo obrigatório ao perder; no máximo um contato principal por cliente | Preserva o histórico e a consistência dos indicadores |
| D031 | 2026-11-09 | Endereço do cliente fica fora do MVP; motivo da perda em texto livre | Redução de escopo; lista fixa de motivos é melhoria pós-MVP |
| D032 | 2026-11-09 | Autenticação com ASP.NET Core Identity e cookie (inacessível a JavaScript, só HTTPS, restrito ao próprio site, com proteção contra requisições forjadas) | API e Blazor no mesmo endereço; Identity traz hash de senha, bloqueio e invalidação de sessão. Token JWT no navegador ficaria exposto a scripts; provedor externo adiciona dependência e custo |
| D033 | 2026-11-09 | Ajuste em D023: a tabela User herda do usuário do Identity e ganha campos de controle; o perfil continua no campo Role, sem as tabelas de papéis do Identity | Consequência de D032 |
| D034 | 2026-11-09 | Medidas de segurança do MVP: senha de no mínimo 8 caracteres e bloqueio após 5 erros; usuários criados só pelo administrador, com troca de senha no primeiro acesso; primeiro administrador criado por variáveis de ambiente; autorização sempre na API, por perfil e por dono do registro; segredos fora do repositório; validação declarada no Shared; erros padronizados; limite de tentativas de login; logs sem dados pessoais | Segurança desde o início com custo baixo |
| D035 | 2026-11-09 | Testes: xUnit; integração com a API em memória e PostgreSQL real em contêiner Docker; interface por roteiro manual; GitHub Actions a cada envio. Toda regra de negócio e de permissão tem pelo menos um teste; sem meta percentual de cobertura | Banco em memória não aplica restrições nem se comporta como o PostgreSQL |
| D036 | 2026-11-09 | Repositório privado no GitHub | Produto destinado à venda |

## Decisões pendentes

| # | Assunto | Observação |
|---|---|---|
| P010 | Adequação à LGPD: exportação e eliminação de dados a pedido do titular, política de privacidade, termos de uso | Resolver antes da primeira venda; consultar advogado |

P001 foi resolvida por D020, P002 por D024, P003 a P006 por D010 a D013, P007 por D032 a D034, P008 por D035 e P009 por D014 a D019.

## Alternativas descartadas

| Assunto | Alternativa | Motivo |
|---|---|---|
| Arquitetura | Clean Architecture | Seis ou mais projetos e muito código de cerimônia; 10 a 15 horas a mais antes de qualquer funcionalidade |
| Arquitetura | Fatias verticais | Custo médio; regras comuns tendem a se espalhar |
| Autenticação | Token JWT guardado no navegador | Exposto a scripts maliciosos; renovação e revogação dão mais trabalho |
| Autenticação | Provedor externo | Dependência externa e gratuidade limitada |
| Testes | Banco em memória do EF Core | Não aplica unicidade nem se comporta como o PostgreSQL |
| Frontend | React | Segunda linguagem e duplicação de modelos; risco médio para o prazo |
| Frontend | Angular | Curva de aprendizado alta para o prazo de um mês |
| Banco | SQL Server (Azure SQL gratuito) | 100.000 vCore-segundos por mês podem acabar no meio do mês com uso diário |
| Hospedagem | Azure App Service F1 | Limite de 60 minutos de CPU por dia, sem domínio próprio, saída paga cara |
