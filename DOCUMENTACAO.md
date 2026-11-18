# Central de Eventos do ICET

Documento inicial de requisitos, regras de negócio e organização do projeto.

- Versão: 0.1
- Data: 18/11/2026
- Situação: levantamento inicial; decisões sugeridas aguardam validação.
- Fonte de negócio: `Proposta-Web (1).pdf`, página 3, seções 1 a 4.
- Referência de organização: imagem fornecida pelo solicitante.
- Nome adotado: Central de Eventos do ICET


## 1. Objetivo e escopo

Centralizar a divulgação, a programação e as inscrições em eventos acadêmicos presenciais e gratuitos do ICET. Um evento pode durar um ou mais dias e reunir palestras, minicursos, apresentações e outras atividades, cada uma com horário, local, responsável e quantidade de vagas próprios.

### 1.1 Escopo inicial

- Cadastro e autenticação de usuários com perfil de aluno ou organizador.
- Gestão dos eventos e atividades de cada organizador.
- Consulta pública de eventos publicados e da programação por dia e horário.
- Inscrição do aluno nas atividades, consulta das próprias inscrições e solicitação de cancelamento.
- Controle de capacidade, período de inscrição, duplicidade e conflito de horário.
- Consulta de inscritos por atividade e total de participantes distintos por evento.
- Preservação do histórico de inscrições ao cancelar ou excluir eventos e atividades.

### 1.2 Itens sem previsão na proposta

Pagamentos, certificados, lista de espera, registro de presença, notificações por e-mail, integração com login institucional, eventos remotos, exportação de relatórios e um perfil de administrador não integram o escopo inicial documentado. Sua inclusão depende de definição posterior.

## 2. Perfis e permissões

| Perfil | Ações previstas | Origem |
| --- | --- | --- |
| Visitante | Consultar eventos publicados e sua programação sem autenticação | Proposta |
| Aluno | Consultar programação, inscrever-se em atividades, consultar suas inscrições e solicitar cancelamento | Proposta |
| Organizador | Cadastrar, editar, publicar, cancelar e excluir seus eventos e atividades; acompanhar inscrições | Proposta |

**Detalhamento sugerido:** o organizador acessa dados de inscritos apenas nos eventos sob sua responsabilidade. O aluno acessa apenas suas próprias inscrições. A API verifica essas permissões a cada operação, independentemente dos controles da interface.

O mecanismo para conceder o perfil de organizador e a possibilidade de uma pessoa acumular perfis estão pendentes de definição; não presumir concessão livre desse perfil no cadastro público.

## 3. Requisitos funcionais

| ID | Requisito | Critério de aceite inicial |
| --- | --- | --- |
| RF-01 | Cadastrar usuários | Permitir criar uma conta com dados válidos e sem identificador de acesso duplicado; campos e concessão de perfil serão definidos |
| RF-02 | Autenticar e aplicar acesso por perfil | Credenciais válidas permitem acesso às funções autorizadas; uma chamada direta à API também respeita perfil e titularidade |
| RF-03 | Gerenciar eventos | Organizador consegue cadastrar, editar, publicar, cancelar e excluir eventos próprios; eventos de terceiros ficam protegidos |
| RF-04 | Gerenciar atividades | Organizador executa as mesmas operações nas atividades de seus eventos, informando horário, local, responsável e vagas |
| RF-05 | Exibir eventos e programação publicamente | Visitante consulta eventos publicados e suas atividades visíveis ordenadas por dia e horário, com local, ministrante e vagas disponíveis |
| RF-06 | Realizar inscrição por atividade | Aluno autenticado recebe confirmação apenas quando há vaga, prazo válido, ausência de duplicidade e ausência de conflito de horário |
| RF-07 | Consultar minhas inscrições | Aluno visualiza suas inscrições, respectivas atividades, eventos e situação |
| RF-08 | Solicitar cancelamento de inscrição | Aluno registra pedido sobre inscrição própria e recebe sua situação; aprovação ou processamento automático permanece pendente |
| RF-09 | Consultar inscritos por atividade | Organizador consulta os inscritos das atividades de seus eventos; dados exibidos e tratamento de cancelados serão definidos |
| RF-10 | Contar participantes distintos | Organizador consulta o número de alunos distintos do evento, sem contar a mesma pessoa novamente por participar de várias atividades |
| RF-11 | Preservar histórico | Cancelar ou excluir evento ou atividade não apaga os registros históricos das inscrições vinculadas |

## 4. Regras de negócio

### 4.1 Eventos e atividades

| ID | Regra |
| --- | --- |
| RN-01 | Os eventos atendidos pelo escopo são acadêmicos, presenciais e gratuitos |
| RN-02 | Um evento pode abranger um ou mais dias e reunir diversas atividades |
| RN-03 | Cada atividade possui horário, local, responsável/ministrante e capacidade próprios |
| RN-04 | Somente o organizador responsável pode alterar, publicar, cancelar ou excluir seu evento e suas atividades |
| RN-05 | A consulta da programação publicada é pública |
| RN-06 | O início deve anteceder o término do evento e de cada atividade; as atividades devem estar contidas no período do evento |
| RN-07 | Rascunhos e registros excluídos ficam fora da consulta pública; uma atividade só fica pública quando ela e o evento estão publicados |
| RN-08 | A capacidade de uma atividade é um inteiro positivo e não pode ser reduzida abaixo do total de inscrições que ocupam vagas |

### 5.2 Inscrições, vagas e horários

| ID | Regra |
| --- | --- |
| RN-09 | A inscrição é feita pelo aluno em uma atividade; não é prevista inscrição independente no evento |
| RN-10 | Uma inscrição só pode ser confirmada se houver vaga disponível |
| RN-11 | O sistema deve impedir inscrição duplicada do mesmo aluno na mesma atividade |
| RN-12 | O sistema deve controlar os prazos de inscrição |
| RN-13 | O sistema deve impedir conflito de horário entre atividades nas quais o aluno está inscrito |
| RN-14 | A criação da inscrição deve validar vagas, duplicidade e conflito de forma consistente mesmo em requisições simultâneas |
| RN-15 | Proposta de cálculo: vagas disponíveis = capacidade - inscrições confirmadas; o resultado nunca pode ser negativo |
| RN-16 | Proposta de prazo: usar abertura e encerramento por atividade, com abertura <= agora < encerramento e encerramento <= início da atividade |
| RN-17 | Proposta de conflito: aplicar a verificação às inscrições confirmadas do aluno em todos os eventos, usando início A < fim B e início B < fim A |
| RN-18 | Atividades consecutivas, como 08h-09h e 09h-10h, não se sobrepõem pela regra RN-17 |
| RN-19 | Somente eventos e atividades publicados, não cancelados e não excluídos aceitam novas inscrições |
| RN-20 | Após cancelamento efetivo, a vaga é liberada; um pedido ainda pendente mantém a vaga ocupada até sua resolução |
| RN-21 | Uma nova inscrição após cancelamento pode ser aceita se todas as validações forem satisfeitas; o cancelamento anterior permanece no histórico |

### 4.3 Cancelamento, exclusão e relatórios

| ID | Regra | Origem |
| --- | --- | --- |
| RN-22 | O aluno pode consultar suas inscrições e solicitar cancelamento | Proposta |
| RN-23 | Cancelamento e exclusão de eventos ou atividades devem preservar o histórico de inscrições | Proposta |
| RN-24 | Proposta de exclusão: usar marcação lógica de exclusão, mantendo vínculos e dados necessários ao histórico | Sugerido |
| RN-25 | Cancelar um evento impede inscrições em todas as suas atividades; cancelar uma atividade impede inscrições nela | Derivado |
| RN-26 | Proposta de efeito: cancelar um evento cancela suas atividades e inscrições vigentes; cancelar uma atividade cancela suas inscrições vigentes, registrando motivo, autor e data | Sugerido |
| RN-27 | Proposta de exclusão com inscrições vigentes: efetuar os efeitos de cancelamento e a marcação de exclusão na mesma transação | Sugerido |
| RN-28 | O total de participantes distintos deve contar cada aluno uma única vez por evento | Proposta |
| RN-29 | Proposta de contagem operacional: considerar alunos com pelo menos uma inscrição confirmada em atividade vigente do evento; cancelamentos permanecem consultáveis no histórico | Sugerido |
| RN-30 | Alterar horário de atividade com inscrições confirmadas deve ser bloqueado até existir uma política de remanejamento que preserve a regra de conflito | Sugerido |

## 5. Estados e fluxos propostos

Esta seção é **sugerida**. O PDF não define os nomes dos estados nem todas as transições.

### 5.1 Evento e atividade

- `rascunho`: editável pelo responsável, sem visibilidade pública ou novas inscrições.
- `publicado`: visível conforme a publicação do evento pai; aceita inscrições quando as demais condições são satisfeitas.
- `cancelado`: bloqueia novas inscrições, mantém histórico e identifica o cancelamento para inscritos.
- `excluido_em`: marcação de exclusão lógica independente do estado; remove o registro das listagens normais, preservando a consulta histórica autorizada.

Fluxo inicial: `rascunho -> publicado -> cancelado`. Rascunhos também podem ser cancelados. Exclusão lógica pode ocorrer em rascunhos, publicados e cancelados, respeitando RN-27. Reabertura e restauração não estão definidas.

Para uma atividade já publicada, a edição continua sujeita às validações de período, capacidade e alteração de horário. O término cronológico bloqueia inscrições pelo prazo, sem exigir um estado persistido de encerramento nesta versão.

### 6.2 Inscrição

Fluxo básico sugerido: `confirmada -> cancelada`. A confirmação só ocorre após todas as validações, em transação.

Se houver aprovação de cancelamento, acrescentar `cancelamento_solicitado`, com transições para `cancelada` ou retorno a `confirmada` em caso de recusa. Enquanto aguarda decisão, a inscrição ocupa vaga e participa da verificação de conflitos. O responsável pela decisão, o prazo e o motivo da recusa precisam ser definidos antes de implementar esse fluxo.

### 6.3 Jornada principal

1. Organizador autorizado cria evento e atividades em rascunho.
2. Organizador informa dados, capacidade e períodos necessários e publica a programação.
3. Visitante consulta a programação; aluno autentica-se para se inscrever.
4. API valida acesso, publicação, prazo, vaga, duplicidade e conflito e confirma a inscrição em transação.
5. Aluno acompanha suas inscrições e solicita cancelamento quando necessário.
6. Organizador acompanha inscritos e participantes distintos; alterações e cancelamentos preservam o histórico.

## 7. Requisitos não funcionais propostos

Os requisitos abaixo são **sugeridos**, para orientar a implementação; não representam metas de desempenho já acordadas.

| ID | Requisito | Verificação esperada |
| --- | --- | --- |
| RNF-01 | Proteger credenciais | Senhas armazenadas com hash apropriado; segredos fora do código e do repositório |
| RNF-02 | Aplicar autorização na API | Rotas privadas rejeitam chamadas sem autenticação ou sem permissão sobre o recurso |
| RNF-03 | Garantir integridade no PostgreSQL | Chaves estrangeiras, restrições e transações evitam inscrições órfãs, duplicidades vigentes e excesso de vagas |
| RNF-04 | Validar entradas e consultas | Dados inválidos são rejeitados e consultas SQL usam parâmetros |
| RNF-05 | Oferecer interface responsiva e acessível | Fluxos principais utilizáveis em celular e computador, com teclado, rótulos e mensagens legíveis |
| RNF-06 | Padronizar respostas da API | Códigos HTTP adequados e mensagens em português sem expor pilhas de erro ou segredos |
| RNF-07 | Tratar datas consistentemente | Instantes persistidos com referência de fuso; proposta de exibição em America/Manaus, ainda a validar |
| RNF-08 | Permitir rastreabilidade | Registrar autoria e data de alterações relevantes, preservando histórico de cancelamento e exclusão |
| RNF-09 | Reproduzir instalação e banco | Documentar variáveis, scripts e migrações; manter package-lock.json versionado e node_modules fora do Git |

Metas de disponibilidade, volume de usuários, latência, backup e retenção de dados precisam ser acordadas antes da implantação.

## 8. Organização técnica conforme a imagem

**Stack da proposta:** HTML5, CSS3 e JavaScript na interface; Node.js com Express na API; PostgreSQL para persistência; Git e GitHub para versionamento e colaboração.

A imagem mostra as pastas `config`, `microservices`, `node_modules`, `public` e `utils`, além de arquivos chamados `config`, `db`, `package`, `package-lock` e `server`. Como as extensões não estão visíveis, a árvore abaixo assume `.js` e `.json` de acordo com seus ícones e com a stack. Os nomes internos são sugestões.

```text
central de eventos ICET/
├── config/                      # Configurações específicas e migrações
│   └── migrations/              # Evolução versionada do banco (sugerido)
├── microservices/               # Módulos de negócio da API
│   ├── auth/                    # Autenticação e autorização
│   ├── usuarios/                # Contas e perfis
│   ├── eventos/                 # Gestão e consulta de eventos
│   ├── atividades/              # Programação e capacidade
│   └── inscricoes/              # Inscrições, cancelamentos e contagens
├── node_modules/                # Gerado pela instalação; não versionar
├── public/                      # Interface servida pelo Express
│   ├── css/
│   ├── js/
│   └── index.html
├── utils/                       # Funções e tratamentos compartilhados
├── config.js                    # Entrada central de configuração
├── db.js                        # Pool de conexão e acesso a transações
├── package.json                 # Dependências e scripts
├── package-lock.json            # Versões resolvidas das dependências
├── server.js                    # Inicialização do Express e registro de rotas
├── .env.example                 # Modelo de variáveis sem segredos (sugerido)
├── .gitignore                   # Exclusões de versionamento (sugerido)
└── DOCUMENTACAO.md              # Este documento
```

### 8.1 Responsabilidades propostas

- `server.js`: inicializar a aplicação, registrar middleware, arquivos estáticos e rotas; não concentrar regras de negócio.
- `config.js`: carregar e validar variáveis de ambiente; importar configurações específicas de `config/`, sem duplicar valores.
- `db.js`: disponibilizar conexão PostgreSQL e transações; receber configuração de `config.js`.
- `microservices/<modulo>/`: separar rotas, controladores, serviços e repositórios conforme a complexidade; serviços aplicam regras, repositórios executam consultas parametrizadas.
- `public/`: conter páginas, estilos e JavaScript da interface; comunicar-se com a API sem acessar diretamente o banco.
- `utils/`: conter apenas elementos compartilhados, como validação e tratamento de erros; regras específicas ficam nos módulos.

O nome `microservices/` é preservado para seguir a imagem. A proposta inicial é executar os módulos em uma única aplicação Express, com um PostgreSQL. A pasta, por si só, não define serviços independentes; separação em processos, redes ou bancos distintos exigiria uma decisão arquitetural adicional.

## 9. Modelo de dados conceitual sugerido

O modelo relacional, as transações e o contrato HTTP estão em [Modelagem do banco de dados e especificação da API](MODELAGEM-E-API.md).

| Entidade | Campos iniciais | Relações e observações |
| --- | --- | --- |
| Usuário | id, nome, email, senha_hash, perfil, criado_em | E-mail normalizado e único é uma sugestão; dados institucionais e múltiplos perfis estão pendentes |
| Evento | id, organizador_id, titulo, descricao, inicio_em, fim_em, estado, criado_em, atualizado_em, excluido_em | Pertence a um organizador e reúne atividades |
| Atividade | id, evento_id, titulo, descricao, tipo, inicio_em, fim_em, local, ministrante, capacidade, inscricoes_abrem_em, inscricoes_encerram_em, estado, excluido_em | Pertence a um evento; ministrante não precisa ser usuário no modelo inicial |
| Inscrição | id, aluno_id, atividade_id, estado, criada_em, cancelada_em, motivo_cancelamento | Liga aluno a atividade; mantém histórico, mesmo após cancelamento |
| Histórico de alterações | id, entidade, entidade_id, acao, autor_id, registrada_em, detalhes | Sugestão para rastrear alterações relevantes; não expor informações sensíveis desnecessárias |

Relacionamentos: usuário organizador `1:N` eventos; evento `1:N` atividades; aluno `N:N` atividades por inscrições.

Aplicar unicidade de inscrição vigente por aluno e atividade, preservando registros cancelados. Se houver cancelamento pendente, ele também integra a restrição de vigência. A implementação deve serializar alterações concorrentes por aluno e controlar capacidade por atividade, em ordem consistente, para evitar que duas inscrições simultâneas burlem conflito ou disputem a última vaga. O mecanismo exato será definido no desenho das transações.

## 10. Cenários mínimos de aceite

Os cenários verificam os requisitos e, quando indicado, as escolhas sugeridas. Não representam testes já executados.

| Cenário | Resultado esperado |
| --- | --- |
| Visitante consulta evento publicado | Visualiza programação por dia e horário, local, ministrante e vagas; não acessa dados pessoais de inscritos |
| Aluno tenta gerenciar evento | Operação rejeitada por falta de permissão |
| Organizador tenta alterar evento de outro | Operação rejeitada sem alteração dos dados |
| Dois alunos disputam a última vaga | Apenas uma inscrição é confirmada; capacidade permanece respeitada |
| Mesmo aluno envia duas inscrições simultâneas na mesma atividade | Apenas uma inscrição vigente é criada |
| Mesmo aluno tenta inscrever-se simultaneamente em atividades sobrepostas | No máximo uma inscrição é confirmada, considerando RN-17 |
| Aluno tenta inscrever-se fora do período | Inscrição rejeitada sem ocupar vaga |
| Aluno escolhe atividades consecutivas | Inscrições permitidas pelas regras de horário sugeridas, se as demais condições forem válidas |
| Aluno consulta ou cancela inscrição de terceiro | Acesso rejeitado |
| Cancelamento é efetivado | Histórico preservado e vaga liberada uma única vez |
| Evento com inscrições é excluído | Sai da listagem normal; histórico continua disponível às pessoas autorizadas |
| Mesmo aluno está em três atividades do evento | Conta uma vez no total de participantes distintos, segundo a política validada |
| Organizador reduz capacidade abaixo da ocupação | Alteração rejeitada |

## 11. Decisões pendentes de validação

| ID | Questão | Encaminhamento inicial |
| --- | --- | --- |
| PD-01 | O nome oficial é ICET ou ICETI? | Usar ICET provisoriamente, seguindo o PDF |
| PD-02 | Quem pode cadastrar-se e quais dados são obrigatórios? | Não presumir obrigatoriedade de matrícula ou e-mail institucional |
| PD-03 | Como se concede o perfil de organizador? Uma conta acumula perfis? | Definir concessão controlada antes de implementar cadastro público |
| PD-04 | Cancelamento de inscrição é automático ou exige aprovação? Há limite de prazo? | Confirmar política, responsável e estados antes de implementar RF-08 |
| PD-05 | Prazos são por atividade ou por evento? | Sugestão: por atividade, com encerramento até seu início |
| PD-06 | Conflitos são verificados entre todos os eventos? | Sugestão: sim, usando somente inscrições que ocupam vagas |
| PD-07 | Como contar cancelados no total de participantes? | Sugestão: mostrar confirmados distintos e manter cancelados no histórico |
| PD-08 | Como alterar horários após inscrições e como avisar participantes? | Bloquear alteração de horário inicialmente; definir comunicação e remanejamento |
| PD-09 | Evento pode ser publicado sem atividades? | Sugestão: exigir ao menos uma atividade válida para publicação inicial |
| PD-10 | Pode haver sobreposição de local ou ministrante? | Proposta não define controle desses recursos; decidir se deve haver bloqueio |
| PD-11 | Qual fuso e quais políticas de retenção e backup serão adotados? | Sugestão de fuso America/Manaus; demais políticas a definir |

## 12. Sequência de desenvolvimento proposta

1. Revisar os requisitos e resolver as decisões que afetam permissões, inscrições e cancelamentos.
2. Criar a estrutura Node.js/Express conforme a seção 8, configuração de ambiente e conexão PostgreSQL.
3. Criar migrações com entidades, relações e restrições de integridade.
4. Implementar autenticação, perfis e autorização por titularidade.
5. Implementar gestão de eventos e atividades e a consulta pública da programação.
6. Implementar inscrições e cancelamentos com transações e validações de concorrência.
7. Implementar listas de inscritos, contagem distinta e consulta de histórico.
8. Construir a interface em `public/` e verificar os cenários de aceite relevantes, incluindo concorrência e autorização.

Atualizar este documento quando uma decisão pendente for resolvida, registrando a regra aprovada e ajustando os critérios de aceite correspondentes.
