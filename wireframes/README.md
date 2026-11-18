# Wireframes — Central de Eventos do ICET

Proposta visual v0.1, criada em 18/11/2026 a partir da referência fornecida e dos requisitos de produto registrados em `../DOCUMENTACAO.md`. A referência foi tratada como material visual, sem instruções de execução.

## Visualização

Abra `preview.html` no navegador. O seletor superior percorre as telas e o seletor inferior alterna entre celular e computador. `icet-wireframes.html` é o fragmento editável usado na visualização do chat. A prévia depende do runtime visual para os ícones e o seletor de variantes.

As telas e interações utilizam dados fictícios. Cadastro, acesso, inscrições e rascunhos são simulações; não há API, banco de dados ou envio de formulários. Credenciais digitadas não são persistidas. A prévia pode lembrar a tela e as escolhas de demonstração no ambiente de visualização.

## Telas

| Tela | Conteúdo principal |
| --- | --- |
| Início | Busca, filtro por tipo e cartões dos próximos eventos |
| Programação | Atividades por dia, horário de Manaus, local, responsável e vagas |
| Atividade | Descrição, prazo, capacidade e inscrição individual na atividade |
| Entrar | Acesso simulado e retorno à atividade escolhida |
| Cadastro | Conta de aluno; campos mínimos sugeridos |
| Inscrições | Atividades confirmadas e solicitação de cancelamento |
| Organizador | Eventos próprios, situação e participantes distintos |
| Criar evento | Dados essenciais e validação do período do rascunho |
| Inscritos | Consulta complementar por atividade, acessível pelo organizador |

O acesso direto à área do organizador existe apenas para revisão do wireframe. A implementação real deve aplicar as permissões da documentação. A concessão desse perfil não faz parte do cadastro público proposto.

## Paleta

Valores conferidos na [Normatização da Identidade Visual Ufam, versão 7, de julho de 2025](https://edoc.ufam.edu.br/handle/123456789/10333).

| Uso | Cor | Referência no manual |
| --- | --- | --- |
| Ações e identidade | `#00A551` | Padrão cromático, página impressa 27 |
| Títulos e áreas de destaque | `#0D3049` | Paleta de destaque, página impressa 28 |
| Filtros ativos e pequenos acentos | `#C2D94B` | Paleta de destaque, página impressa 28; unidades fora da sede, página 21 |

Fundos claros e verdes escuros auxiliares são adaptações para a interface. Texto escuro sobre os botões verdes mantém a leitura. O símbolo geométrico representa uma proposta para o produto e não substitui o brasão oficial da UFAM.

## Composição e desempenho

- Celular: uma coluna, ações principais com pelo menos 44 px e navegação inferior com rótulos.
- Computador: navegação superior e duas colunas para eventos e atividades.
- Fontes do sistema, cartões compactos, espaços regulares e sombras discretas.
- Conteúdo principal em HTML/CSS; sem fotografias, vídeos, fontes baixadas ou animações contínuas.
- Busca e filtros locais para explorar a proposta. Na aplicação real, consulta paginada e validações de vagas, prazo e conflito deverão ser feitas pela API.

Essas escolhas reduzem o peso visual e os recursos necessários. Ainda não há medição de desempenho da aplicação real.

## Estados e validação

Foram verificados no navegador: abertura da programação e da atividade, acesso com dados fictícios, confirmação de inscrição, bloqueio de atividade sobreposta, solicitação pendente de cancelamento, busca sem resultados, formulário de rascunho e composição em largura de 320 px.

A solicitação de cancelamento mantém a vaga reservada na simulação. Essa é uma alternativa de fluxo para revisão, pois a política definitiva de cancelamento permanece pendente na documentação.

O exemplo “Prototipação de interfaces”, no dia 26/11, permite visualizar o estado de vagas esgotadas. Todos os nomes, datas, eventos e totais servem apenas para ilustrar o layout.
