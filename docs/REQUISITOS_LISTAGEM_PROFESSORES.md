
# Requisitos da Tela --- Listagem de Professores

**Projeto:** Sistema de Gestão de Trabalhos de Conclusão de Curso (TCC)\
**Disciplina:** GAC116 --- Programação Web\
**Tecnologia do frontend:** TypeScript\
**Status do documento:** Especificação funcional aprovada para orientar
a implementação\
**Versão:** 1.0

------------------------------------------------------------------------

## 1. Objetivo da tela

A tela de Listagem de Professores deve permitir que o usuário localize
e identifique os professores cadastrados no Sistema de Gestão de TCCs
por meio de uma apresentação visual inspirada em cartões de
identificação docente.

A interface deve manter a mesma estrutura visual e de navegação
utilizada na tela de Listagem de Alunos, alterando apenas as
informações específicas da entidade Professor.

A tela deve priorizar:

- busca textual simples pelo nome do professor;
- apresentação dos professores em cartões verticais, um abaixo do
  outro;
- identificação rápida pelo nome do professor e pelos dados do
  departamento;
- navegação para a visualização individual de cada professor.

A interface deve funcionar como um espaço de consulta e identificação,
sem acrescentar informações que não sejam necessárias ao
reconhecimento do professor.

A implementação não deve criar regras de negócio ou filtros que não
sejam oferecidos pelo backend.

## 2. Estrutura global compartilhada

O header e o footer devem ser exatamente os mesmos definidos para o
Dashboard. Eles são componentes globais da aplicação e não devem receber
variações específicas nesta tela.

### 2.1. Header

Reutilizar integralmente o header global, incluindo:

- identificação do Sistema de Gestão de TCCs;
- navegação horizontal para Dashboard e listagens;
- ícones e labels já definidos;
- indicação da rota ativa (`/professores`);
- controle global de tema claro/escuro.

Não criar um segundo header nem duplicar sua implementação.

### 2.2. Footer

Reutilizar integralmente o footer global, incluindo:

- divisor superior discreto;
- identificação do Sistema de Gestão de TCCs;
- identificação da disciplina GAC116;
- ano de desenvolvimento.

Não criar um footer específico para a listagem.

## 3. Organização do conteúdo

A área principal da página deve apresentar, nesta ordem:

1. campo único de busca textual;
2. listagem vertical dos cartões de identificação docente.

O conteúdo deve aproveitar a largura horizontal disponível.

Não utilizar tabela convencional nem grade de cartões em múltiplas
colunas. A apresentação deve manter uma única coluna vertical,
independentemente da quantidade de professores retornados pela API.

A estrutura deve seguir o mesmo padrão adotado na tela de Listagem de
Alunos.

## 4. Campo de busca

### 4.1. Apresentação

- Exibir um único campo de busca textual logo abaixo do header.
- O campo deve ocupar toda a largura disponível da área de conteúdo.
- Não adicionar filtros separados por departamento ou outros critérios.
- O campo deve ter identificação ou placeholder que deixe clara sua
  finalidade.

### 4.2. Comportamento

A busca deve utilizar o mecanismo disponibilizado pelo backend:

- endpoint: `GET /api/professores/`;
- parâmetro: `search`;
- campo pesquisável: `nome`.

A atualização da busca deve ocorrer conforme o usuário digita. A
implementação deve utilizar debounce para evitar requisições
desnecessárias durante a digitação.

O `ProfessorViewSet` utiliza `SearchFilter` e configura exclusivamente
o campo `nome` para pesquisa.

Portanto, a pesquisa deve considerar somente o nome do professor.

O frontend não deve simular filtros por departamento ou outros
critérios que não estejam configurados no `ProfessorViewSet`.

### 4.3. Resultados

- Quando houver resultados, apresentar os cartões correspondentes aos
  professores retornados pela API.
- Quando a busca não encontrar resultados, apresentar uma mensagem
  clara informando que nenhum professor foi encontrado.
- Quando o campo estiver vazio, apresentar a listagem sem restringir
  os resultados por texto.
- Erros de comunicação com a API devem ser apresentados por meio de
  uma mensagem compreensível, sem substituir o erro por dados
  fictícios.

## 5. Listagem dos professores

### 5.1. Formato e disposição

- Apresentar os professores em uma única coluna.
- Cada professor deve ocupar praticamente toda a largura disponível.
- Os cartões devem ser dispostos verticalmente, um abaixo do outro.
- Não utilizar tabela nem grade de cartões com múltiplas colunas.
- A composição deve remeter a cartões de identificação, combinando
  essa referência visual com uma interface moderna e limpa.
- Os cartões devem possuir espaçamento suficiente para que cada
  professor seja visualmente distinguível.

A estrutura deve preservar o mesmo padrão utilizado na Listagem de
Alunos, sem efeitos de sobreposição ou desvanecimento.

### 5.2. Estrutura interna de cada cartão

Cada cartão deve conter os seguintes elementos:

1. **Identificação docente:** pequeno texto ou indicação visual que
   caracterize o cartão como uma identificação de professor.
2. **Nome do professor:** apresentado com maior destaque visual,
   utilizando tamanho e peso tipográfico superiores aos demais dados.
3. **Nome do departamento:** apresentado abaixo do nome do professor.
4. **Sigla do departamento:** apresentada ao lado do nome do
   departamento, mantendo ambas as informações alinhadas
   horizontalmente.

A composição deve estabelecer uma hierarquia clara: o nome do professor
é a informação principal, enquanto nome e sigla do departamento são
informações complementares.

### 5.3. Identificação docente

- Apresentar a indicação visual **"IDENTIFICAÇÃO DOCENTE"** em cada
  cartão.
- Essa indicação deve funcionar como elemento de identidade visual,
  sem competir com o nome do professor.
- Não utilizar números, códigos ou informações de identificação
  fictícias.
- A indicação deve ser consistente em todos os cartões.

### 5.4. Nome do professor

- Apresentar o nome como elemento de maior destaque do cartão.
- Manter contraste e legibilidade.
- Permitir nomes longos sem sobreposição com os dados do departamento.
- Evitar truncamento que impeça a identificação do professor.

O valor apresentado deve ser obtido diretamente do campo `nome`
retornado pela API.

### 5.5. Departamento

O departamento deve ser apresentado logo abaixo do nome do professor,
com duas informações:

- nome do departamento;
- sigla do departamento.

As duas informações devem permanecer alinhadas horizontalmente quando
houver espaço disponível.

O nome e a sigla devem ser obtidos a partir da entidade Departamento
relacionada ao professor, utilizando o identificador retornado no campo
`departamento`.

Não apresentar somente o identificador numérico da chave estrangeira.

## 6. Interação e navegação

- A superfície inteira de cada cartão deve ser clicável.
- Ao selecionar um cartão, navegar para a tela de visualização do
  professor correspondente.
- A navegação deve utilizar o identificador (`id`) retornado pela API.
- A rota de visualização deve ser definida e integrada à arquitetura
  de rotas do frontend.
- O cartão deve apresentar feedback visual discreto de interação,
  como alteração de fundo, sombra ou elevação no hover.
- O comportamento deve ser acessível por teclado e apresentar
  indicação visual de foco.
- Não adicionar botões internos que concorram com a ação principal de
  clicar no cartão.

O conteúdo e as funcionalidades da tela de visualização individual do
professor não fazem parte desta especificação e serão definidos em
documento próprio.

## 7. Integração com a API

### 7.1. Endpoint e busca

| Operação | Endpoint | Parâmetro |
|---|---|---|
| Listar professores | `GET /api/professores/` | Nenhum |
| Buscar professores | `GET /api/professores/?search={termo}` | `search` |

O `ProfessorViewSet` utiliza `SearchFilter` e configura exclusivamente
o campo `nome` para pesquisa.

A busca deve ser realizada pela API, respeitando a configuração
existente no backend.

### 7.2. Campos necessários

O `ProfessorSerializer` utiliza `fields = '__all__'`, disponibilizando
os campos do modelo Professor.

| Informação necessária na interface | Campo da resposta |
|---|---|
| Identificador para navegação | `id` |
| Nome do professor | `nome` |
| Identificador do departamento | `departamento` |

O campo `departamento` representa uma chave estrangeira para o modelo
Departamento.

Na resposta do serializer atual, esse campo contém o identificador do
departamento, e não um objeto com os dados completos da entidade
relacionada.

Portanto, o frontend não deve presumir que a resposta de um professor
contenha diretamente o nome e a sigla do departamento.

### 7.3. Resolução do nome e da sigla do departamento

Para apresentar os dados completos do departamento nos cartões, será
necessário consultar os dados disponibilizados pelo endpoint de
departamentos:

`GET /api/departamentos/`

O `DepartamentoSerializer` também utiliza `fields = '__all__'`,
disponibilizando os campos do modelo Departamento:

- `id`;
- `nome`;
- `sigla`;
- `unidade_academica`.

O frontend deverá relacionar o identificador recebido no campo
`departamento` do professor ao campo `id` do respectivo departamento.

Os campos utilizados na apresentação serão:

- `nome`: nome do departamento;
- `sigla`: sigla do departamento.

O campo `unidade_academica` não precisa ser apresentado nesta tela.

A implementação deve considerar:

- reaproveitamento dos dados de departamentos já carregados;
- armazenamento em memória ou cache durante a utilização da
  aplicação, quando adequado;
- evitar uma requisição individual de departamento para cada professor;
- tratamento de situações em que o relacionamento não possa ser
  resolvido.

Não realizar requisições repetidas e desnecessárias para obter o mesmo
conjunto de departamentos.

### 7.4. Responsabilidades do frontend

- Consumir a listagem e a busca oferecidas pela API.
- Apresentar os dados recebidos, sem inventar ou completar
  informações ausentes.
- Resolver o relacionamento entre professor e departamento utilizando
  os identificadores fornecidos pelo backend.
- Não implementar filtros locais por departamento ou outros critérios
  como se fossem filtros oficiais da API.
- Não realizar uma requisição individual de departamento para cada
  cartão.
- Tratar adequadamente os estados de carregamento, resultado vazio e
  erro.

A configuração de paginação deve respeitar o contrato efetivamente
utilizado pela API. A interface não deve presumir um formato de resposta
diferente daquele configurado no backend.

## 8. Estados da interface

A tela deve contemplar:

- **Carregamento:** apresentar feedback visual enquanto a listagem ou
  uma nova busca estiver sendo processada.
- **Sucesso:** apresentar os cartões dos professores retornados.
- **Lista vazia:** informar que ainda não há professores cadastrados.
- **Busca sem resultados:** informar que nenhum professor corresponde
  ao termo pesquisado.
- **Erro:** apresentar mensagem clara e permitir nova tentativa.
- **Relacionamento não resolvido:** caso não seja possível obter o nome
  e a sigla do departamento associado a um professor, não inventar
  esses valores. A interface deverá tratar a ausência de maneira
  controlada, sem comprometer a apresentação dos demais dados.

Não utilizar dados fictícios em nenhum desses estados.

## 9. Responsividade e acessibilidade

- Manter a listagem em uma única coluna em todos os tamanhos de tela.
- Aproveitar a largura disponível sem criar colunas paralelas.
- Adaptar espaçamentos, tipografia e alinhamento dos dados conforme a
  largura.
- Em telas menores, permitir que nome e sigla do departamento sejam
  reorganizados quando necessário para evitar colisões.
- Garantir contraste adequado nos temas claro e escuro.
- Identificar textualmente o departamento e sua sigla.
- Garantir que cada cartão possa ser acionado por teclado, com foco
  visível.
- Preservar uma área de interação confortável em dispositivos com
  tela sensível ao toque.

## 10. Fora do escopo desta tela

Não fazem parte desta especificação:

- criação, edição ou exclusão de professores;
- alteração dos dados cadastrais;
- visualização detalhada do professor;
- filtros por departamento ou outros critérios não disponibilizados
  pelo backend;
- apresentação de informações de contato;
- apresentação de dados da unidade acadêmica;
- apresentação de TCCs relacionados ao professor;
- gerenciamento de departamentos;
- gerenciamento de cursos, alunos ou unidades acadêmicas;
- autenticação ou gerenciamento de usuários.

Essas funcionalidades, quando necessárias, serão especificadas em
telas ou documentos próprios.

## 11. Critérios de aceite

A tela será considerada implementada quando:

1. Reutilizar exatamente o header e o footer globais definidos no
   Dashboard.
2. Apresentar um único campo de busca textual em toda a largura
   disponível.
3. Pesquisar utilizando o parâmetro `search` do endpoint de
   professores, respeitando o campo pesquisável do backend.
4. Apresentar os professores em uma única coluna, sem tabela ou grade
   de múltiplas colunas.
5. Utilizar cartões com identidade visual inspirada em identificações
   docentes.
6. Apresentar a indicação **"IDENTIFICAÇÃO DOCENTE"** em cada cartão.
7. Apresentar o nome do professor como elemento de maior destaque.
8. Exibir o nome e a sigla do departamento abaixo do nome do professor,
   alinhados horizontalmente quando houver espaço disponível.
9. Apresentar os dados do departamento por meio da resolução do
   relacionamento com a entidade Departamento.
10. Evitar requisições individuais e repetidas para obter os dados dos
    departamentos.
11. Tornar cada cartão inteiramente clicável e direcioná-lo à
    visualização do professor correspondente pelo seu identificador.
12. Apresentar feedback visual de interação e foco acessível por
    teclado.
13. Tratar carregamento, lista vazia, busca sem resultados e erro.
14. Adaptar a apresentação a telas menores e aos temas claro e escuro.
15. Não apresentar filtros ou funcionalidades que a API não oferece.
16. Não utilizar dados fictícios nem informações adicionais que não
    façam parte do objetivo da tela.

## 12. Referências ao backend

Especificação conferida com base nos arquivos do backend fornecido:

- `core/models.py`: modelo `Professor`, com os campos `nome` e
  relacionamento obrigatório com `Departamento`; modelo
  `Departamento`, com os campos `nome`, `sigla` e relacionamento com
  `UnidadeAcademica`.
- `core/serializers.py`: `ProfessorSerializer` e
  `DepartamentoSerializer`, ambos utilizando `fields = '__all__'`.
- `core/views.py`: `ProfessorViewSet`, configuração de `SearchFilter`
  e campo pesquisável `nome`; `DepartamentoViewSet`, baseado em
  `ModelViewSet`.
- `core/urls.py`: registro dos recursos `professores` e
  `departamentos` no roteador da API.

------------------------------------------------------------------------

**Observação:** esta especificação descreve exclusivamente a tela de
Listagem de Professores. As telas de cadastro, edição e visualização
individual serão documentadas separadamente.
