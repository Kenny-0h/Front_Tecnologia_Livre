# Requisitos da Tela --- Dashboard

**Projeto:** Sistema de Gestão de Trabalhos de Conclusão de Curso (TCC)\
**Disciplina:** GAC116 --- Programação Web\
**Tecnologia do frontend:** TypeScript\
**Status do documento:** Especificação funcional aprovada para orientar
a implementação\
**Versão:** 1.0

------------------------------------------------------------------------

## 1. Objetivo da tela

O Dashboard é a página inicial do Sistema de Gestão de TCCs. Seu
objetivo é oferecer uma visão consolidada dos dados gerais cadastrados
no sistema e permitir acesso rápido às principais áreas da aplicação.

A tela deve priorizar: - leitura rápida dos indicadores; - visualização
das distribuições estatísticas dos TCCs; - acesso direto às páginas de
listagem das entidades; - simplicidade de interação e organização
visual.

O Dashboard não deve duplicar regras de negócio nem executar agregações
que já são responsabilidade do backend.

## 2. Estrutura global compartilhada

O Dashboard utilizará a estrutura global da aplicação, compartilhada
pelas demais páginas.

### 2.1. Header horizontal

O header deve: - apresentar a identificação visual do sistema à
esquerda; - disponibilizar navegação horizontal por links de rota
(*router-links*); - representar cada destino por um ícone acompanhado de
uma label discreta; - indicar visualmente a rota atualmente
selecionada; - apresentar, à direita, um controle para alternar entre os
temas disponíveis; - não apresentar avatar, perfil, identificação de
usuário logado ou botão de logout, pois o sistema não possui
autenticação ou gerenciamento de perfis.

Os sete destinos principais e seus ícones definidos são:

  Destino               Rota prevista            Ícone
  --------------------- ------------------------ --------------------
  Dashboard             `/`                      `layout-dashboard`
  TCCs                  `/tccs`                  `book-open`
  Alunos                `/alunos`                `graduation-cap`
  Professores           `/professores`           `users`
  Cursos                `/cursos`                `book`
  Departamentos         `/departamentos`         `building-2`
  Unidades Acadêmicas   `/unidades-academicas`   `landmark`

Os ícones devem ser consistentes entre o header e os cards de navegação
rápida do Dashboard.

### 2.2. Tema

O header deve disponibilizar um controle de alternância entre tema claro
e tema escuro.

Requisitos: - a troca de tema deve afetar toda a aplicação, não apenas o
Dashboard; - a implementação deve centralizar o estado do tema para que
as demais telas utilizem o mesmo comportamento; - a estrutura deve
permitir a inclusão de outros temas no futuro, caso isso seja
decidido; - a escolha de tema não deve depender de autenticação.

A persistência da preferência entre sessões poderá ser definida durante
a implementação da infraestrutura global do frontend.

### 2.3. Footer

O footer deve ser simples e compartilhado por todas as páginas. Sua
principal função é delimitar visualmente o final do conteúdo.

Elementos previstos: - separador superior discreto; - identificação do
Sistema de Gestão de TCCs; - identificação da disciplina GAC116; - ano
de desenvolvimento.

Não incluir elementos de perfil, logout, redes sociais ou informações
sem função no contexto do sistema.

## 3. Organização do conteúdo do Dashboard

A área principal deve apresentar os seguintes blocos, nesta ordem:

1.  título e breve identificação da página;
2.  cards de navegação rápida;
3.  indicadores gerais;
4.  seção do gráfico estatístico.

O layout deve aproveitar o espaço horizontal disponível, manter
alinhamentos e espaçamentos consistentes e adaptar a disposição dos
componentes a telas menores.

## 4. Cards de navegação rápida

### 4.1. Objetivo

Oferecer uma segunda forma de acesso às páginas de listagem das
entidades, além do header.

### 4.2. Quantidade e destinos

Devem existir seis cards:

  Card                  Destino
  --------------------- ------------------------
  TCCs                  `/tccs`
  Alunos                `/alunos`
  Professores           `/professores`
  Cursos                `/cursos`
  Departamentos         `/departamentos`
  Unidades Acadêmicas   `/unidades-academicas`

O Dashboard não deve possuir um card de navegação para si próprio.

### 4.3. Conteúdo de cada card

Cada card deve apresentar: - o mesmo ícone utilizado no header para
representar a entidade; - a label com o nome da entidade.

### 4.4. Comportamento e apresentação

-   A superfície inteira do card deve ser clicável.
-   O card deve navegar para a respectiva rota de listagem.
-   Deve apresentar feedback visual discreto ao passar o cursor
    (*hover*).
-   Os seis cards devem ser organizados em uma grade de três colunas e
    duas linhas em telas com espaço suficiente.
-   Em telas menores, a grade deve se adaptar para duas ou uma coluna,
    preservando legibilidade e área confortável de interação.
-   Não exibir contadores nos cards: o endpoint de estatísticas não
    fornece totais das entidades Aluno, Professor, Curso, Departamento e
    Unidade Acadêmica.
-   Não incluir botões de cadastro nos cards. Sua finalidade é
    exclusivamente a navegação.

## 5. Indicadores gerais

### 5.1. Objetivo

Apresentar uma leitura imediata do volume total de TCCs e da
distribuição dos trabalhos por status.

### 5.2. Indicadores definidos

Devem ser apresentados cinco indicadores:

  Indicador       Origem dos dados
  --------------- ----------------------------------------
  Total de TCCs   `total_geral`
  Em elaboração   Entrada correspondente em `por_status`
  Enviados        Entrada correspondente em `por_status`
  Aprovados       Entrada correspondente em `por_status`
  Reprovados      Entrada correspondente em `por_status`

Os rótulos de status devem corresponder aos valores legíveis retornados
pelo backend: `Em Elaboração`, `Enviado`, `Aprovado` e `Reprovado`.

### 5.3. Regras

-   O total geral deve ser apresentado em destaque.
-   Os quatro indicadores de status devem ser visualmente subordinados
    ao total, mas permanecer fáceis de localizar.
-   Os números devem ser derivados exclusivamente da resposta da API.
-   Não calcular ou estimar totais de alunos, professores, cursos,
    departamentos ou unidades acadêmicas.
-   Se a API retornar zero TCCs, os indicadores devem apresentar zero,
    sem dados demonstrativos.
-   Se algum rótulo de status não estiver presente na resposta, a
    interface deve tratá-lo como zero para a apresentação, sem alterar
    os dados recebidos.

## 6. Gráfico de distribuição dos TCCs

### 6.1. Objetivo

Apresentar a distribuição dos TCCs por diferentes dimensões,
reutilizando um único componente de gráfico.

### 6.2. Tipo de gráfico

-   Utilizar gráfico de barras horizontais.
-   Manter o mesmo componente visual para todas as dimensões.
-   Apresentar categorias e respectivos totais.
-   Utilizar uma legenda que identifique a série como quantidade de
    TCCs.
-   Preservar a legibilidade de nomes longos, especialmente em dimensões
    como orientador, curso, departamento e unidade acadêmica.

### 6.3. Seletor de dimensão estatística

Disponibilizar um seletor compacto (preferencialmente um dropdown) com
todas as nove dimensões retornadas pelo endpoint:

  Label apresentada   Propriedade da resposta
  ------------------- -------------------------
  Status              `por_status`
  Tipo de TCC         `por_tipo`
  Idioma              `por_idioma`
  Semestre            `por_semestre`
  Orientador          `por_orientador`
  Coorientador        `por_coorientador`
  Curso               `por_curso`
  Departamento        `por_departamento`
  Unidade Acadêmica   `por_unidade_academica`

A dimensão inicialmente selecionada deve ser **Departamento**.

### 6.4. Comportamento do seletor

-   A seleção de uma dimensão deve atualizar o título contextual e os
    dados do gráfico.
-   A troca de dimensão deve ocorrer localmente, sem realizar nova
    requisição à API, pois todas as agregações são retornadas pelo mesmo
    endpoint.
-   O componente de gráfico deve ser reutilizado; não devem ser
    implementados nove gráficos independentes.
-   A seleção deve ser acessível por teclado e apresentar claramente a
    opção atual.

### 6.5. Interpretação correta das agregações

As agregações devem ser apresentadas respeitando a regra implementada
pelo backend:

-   **Status:** quantidade de TCCs agrupada pelo status, com rótulos
    legíveis.
-   **Tipo:** quantidade agrupada pelo tipo de TCC, com rótulos
    legíveis.
-   **Idioma:** quantidade agrupada pelo idioma, com rótulos legíveis.
-   **Semestre:** quantidade agrupada pelo semestre letivo de defesa.
    Registros sem semestre de defesa são omitidos pelo backend.
-   **Orientador:** quantidade agrupada pelo nome do orientador.
-   **Coorientador:** quantidade agrupada pelo nome do coorientador.
    TCCs sem coorientador são excluídos dessa agregação.
-   **Curso:** quantidade agrupada pelo nome do curso do aluno associado
    ao TCC.
-   **Departamento:** quantidade agrupada pelo nome do departamento do
    orientador.
-   **Unidade Acadêmica:** quantidade agrupada pelo nome da unidade
    acadêmica do departamento do orientador.

A interface não deve sugerir que a distribuição por departamento ou
unidade acadêmica considera todos os participantes da banca. Essas
dimensões são calculadas a partir do departamento do orientador e da
unidade acadêmica desse departamento.

A distribuição por coorientador também não representa necessariamente
todos os TCCs, pois os trabalhos sem coorientador não entram nessa
agregação.

### 6.6. Ordenação e limites

O frontend não deve inventar filtros, limites de quantidade ou regras de
agregação que não estejam presentes no backend.

A apresentação deve utilizar os dados recebidos. Qualquer ordenação
visual adicional ou limitação de categorias deverá ser definida
explicitamente antes da implementação, sem alterar o significado
estatístico dos dados.

## 7. Integração com a API

### 7.1. Endpoint

  ---------------------------------------------------------------------------
  Método                  Rota                        Finalidade
  ----------------------- --------------------------- -----------------------
  GET                     `/api/tccs/estatisticas/`   Obter o total geral e
                                                      todas as agregações
                                                      estatísticas do
                                                      Dashboard

  ---------------------------------------------------------------------------

### 7.2. Contrato de resposta esperado

A resposta documentada pelo backend possui a seguinte estrutura:

``` typescript
interface DashboardStatistics {
  total_geral: number;
  por_status: Record<string, number>;
  por_tipo: Record<string, number>;
  por_idioma: Record<string, number>;
  por_semestre: Record<string, number>;
  por_orientador: Record<string, number>;
  por_coorientador: Record<string, number>;
  por_curso: Record<string, number>;
  por_departamento: Record<string, number>;
  por_unidade_academica: Record<string, number>;
}
```

As chaves dos objetos de agregação são os rótulos ou nomes das
categorias; os valores são as quantidades de TCCs.

### 7.3. Estratégia de consumo

-   Realizar uma requisição ao endpoint ao carregar o Dashboard.
-   Armazenar a resposta em estado da página ou em uma camada de dados
    apropriada da aplicação.
-   Derivar os cinco indicadores e os dados do gráfico a partir dessa
    resposta.
-   Ao trocar a dimensão do gráfico, selecionar a propriedade
    correspondente no estado já carregado.
-   Não realizar uma requisição por indicador ou por dimensão.
-   Não buscar listagens completas de alunos, professores, cursos,
    departamentos ou unidades apenas para compor o Dashboard.
-   Não recalcular no frontend as agregações que o backend já fornece.

### 7.4. Atualização dos dados

A atualização dos dados deve ocorrer quando a tela for carregada
conforme a estratégia de ciclo de vida e cache adotada pelo frontend. A
troca de dimensão do gráfico não deve disparar uma nova consulta.

## 8. Estados da interface

A tela deve tratar explicitamente os seguintes estados:

  -----------------------------------------------------------------------
  Estado                              Comportamento esperado
  ----------------------------------- -----------------------------------
  Carregamento inicial                Exibir skeletons ou placeholders
                                      nos indicadores e na área do
                                      gráfico.

  Sucesso com dados                   Exibir os indicadores e a dimensão
                                      selecionada no gráfico.

  Sucesso sem TCCs                    Exibir indicadores zerados e
                                      mensagem informando que ainda não
                                      existem TCCs cadastrados.

  Dimensão sem dados                  Manter a estrutura do gráfico e
                                      apresentar uma mensagem de ausência
                                      de dados para a dimensão
                                      selecionada.

  Erro na requisição                  Exibir mensagem clara de erro e
                                      ação para tentar novamente.
  -----------------------------------------------------------------------

Não utilizar números fictícios ou dados de demonstração na aplicação
final, inclusive durante estados de erro ou carregamento.

## 9. Requisitos de interface e experiência

-   Interface limpa, com hierarquia visual clara.
-   Aproveitamento adequado da largura disponível.
-   Espaçamentos, bordas, tipografia e ícones consistentes.
-   Cards de navegação com áreas clicáveis confortáveis.
-   Gráfico com nomes de categorias legíveis.
-   Seletor de dimensão compacto e fácil de identificar.
-   Suporte aos temas claro e escuro.
-   Layout responsivo para desktop, tablet e dispositivos móveis.
-   Estados de carregamento, vazio e erro visualmente consistentes com o
    restante da aplicação.
-   Evitar elementos decorativos que não contribuam para leitura,
    navegação ou compreensão dos dados.

## 10. Fora do escopo desta tela

Não fazem parte dos requisitos do Dashboard:

-   cadastro, edição ou exclusão de entidades;
-   listagem detalhada de TCCs;
-   filtros que não sejam fornecidos pelo backend;
-   busca de alunos, professores ou outras entidades;
-   carregamento de registros completos para produzir estatísticas no
    frontend;
-   autenticação, perfil ou logout;
-   gráficos temporais ou comparações históricas, pois o endpoint atual
    não fornece séries temporais;
-   indicadores de quantidade de alunos, professores, cursos,
    departamentos ou unidades acadêmicas.

## 11. Critérios de aceite

A implementação do Dashboard será considerada aderente a esta
especificação quando:

-   [ ] O header horizontal apresentar os sete destinos definidos e
    destacar a rota atual.
-   [ ] O controle de tema alternar entre claro e escuro globalmente.
-   [ ] O footer delimitar visualmente o final da página.
-   [ ] Os seis cards de navegação estiverem presentes, com os mesmos
    ícones do header.
-   [ ] Os cards estiverem organizados em três colunas por duas linhas
    em telas adequadas e se adaptarem a telas menores.
-   [ ] Os cinco indicadores apresentarem os dados correspondentes à
    resposta da API.
-   [ ] O gráfico utilizar barras horizontais e um componente
    reutilizável.
-   [ ] O seletor disponibilizar as nove dimensões estatísticas.
-   [ ] A troca de dimensão atualizar o gráfico sem nova requisição.
-   [ ] A tela consumir somente o endpoint de estatísticas para
    preencher seus indicadores e gráfico.
-   [ ] A interface tratar carregamento, erro, ausência de TCCs e
    dimensão sem dados.
-   [ ] Nenhum dado fictício for apresentado como dado real.

## 12. Referências de implementação

A especificação foi baseada no backend fornecido pelo professor,
especialmente:

-   `core/views.py`: implementação da ação `TCCViewSet.estatisticas`,
    responsável pelo total e pelas nove agregações.
-   `core/models.py`: escolhas de status, tipo e idioma, além dos
    relacionamentos utilizados nas agregações.
-   `core/urls.py`: roteamento da API.
-   `ESTRUTURA_BACKEND_GESTAO_TCCS.md`: documentação de referência da
    estrutura do backend e do endpoint de estatísticas.

**Observação:** este documento especifica somente a tela Dashboard e os
elementos globais necessários para contextualizá-la. A arquitetura geral
de navegação e a relação de todas as telas do sistema devem permanecer
em um documento geral separado.
