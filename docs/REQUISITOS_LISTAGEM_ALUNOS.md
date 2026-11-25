
# Requisitos da Tela --- Listagem de Alunos

**Projeto:** Sistema de Gestão de Trabalhos de Conclusão de Curso (TCC)\
**Disciplina:** GAC116 --- Programação Web\
**Tecnologia do frontend:** TypeScript\
**Status do documento:** Especificação funcional aprovada para orientar
a implementação\
**Versão:** 1.0

------------------------------------------------------------------------

## 1. Objetivo da tela

A tela de Listagem de Alunos deve permitir que o usuário localize e
identifique os alunos cadastrados no Sistema de Gestão de TCCs por meio
de uma apresentação visual inspirada em cartões de identificação
estudantil.

A interface deve priorizar:

-   busca textual simples por nome ou matrícula;
-   apresentação dos alunos em cartões verticais, um abaixo do outro;
-   identificação rápida pelo nome, matrícula e curso;
-   navegação para a visualização individual de cada aluno.

A tela deve funcionar como um espaço de consulta e identificação,
evitando informações adicionais que não sejam necessárias ao
reconhecimento do aluno.

A implementação não deve criar regras de negócio ou filtros que não
sejam oferecidos pelo backend.

## 2. Estrutura global compartilhada

O header e o footer devem ser exatamente os mesmos definidos para o
Dashboard. Eles são componentes globais da aplicação e não devem receber
variações específicas nesta tela.

### 2.1. Header

Reutilizar integralmente o header global, incluindo:

-   identificação do Sistema de Gestão de TCCs;
-   navegação horizontal para Dashboard e listagens;
-   ícones e labels já definidos;
-   indicação da rota ativa (`/alunos`);
-   controle global de tema claro/escuro.

Não criar um segundo header nem duplicar sua implementação.

### 2.2. Footer

Reutilizar integralmente o footer global, incluindo:

-   divisor superior discreto;
-   identificação do Sistema de Gestão de TCCs;
-   identificação da disciplina GAC116;
-   ano de desenvolvimento.

Não criar um footer específico para a listagem.

## 3. Organização do conteúdo

A área principal da página deve apresentar, nesta ordem:

1.  campo único de busca textual;
2.  listagem vertical dos cartões de identificação estudantil.

O conteúdo deve aproveitar a largura horizontal disponível.

Não utilizar tabela convencional nem grade de cartões em múltiplas
colunas. A apresentação deve manter uma única coluna vertical,
independentemente da quantidade de alunos retornados pela API.

## 4. Campo de busca

### 4.1. Apresentação

-   Exibir um único campo de busca textual logo abaixo do header.
-   O campo deve ocupar toda a largura disponível da área de conteúdo.
-   Não adicionar filtros separados por curso ou outros critérios.
-   O campo deve ter identificação ou placeholder que deixe clara sua
    finalidade.

### 4.2. Comportamento

A busca deve utilizar o mecanismo disponibilizado pelo backend:

-   endpoint: `GET /api/alunos/`;
-   parâmetro: `search`;
-   campos pesquisáveis: `nome` e `matricula`.

A atualização da busca deve ocorrer conforme o usuário digita. A
implementação deve utilizar debounce para evitar requisições
desnecessárias durante a digitação.

O backend configura o `SearchFilter` para pesquisar nos campos `nome` e
`matricula`. Assim, um único termo de pesquisa pode localizar alunos
por qualquer uma dessas informações.

O frontend não deve simular filtros por curso ou outros critérios que
não estejam configurados no `AlunoViewSet`.

### 4.3. Resultados

-   Quando houver resultados, apresentar os cartões correspondentes aos
    alunos retornados pela API.
-   Quando a busca não encontrar resultados, apresentar uma mensagem
    clara informando que nenhum aluno foi encontrado.
-   Quando o campo estiver vazio, apresentar a listagem sem restringir
    os resultados por texto.
-   Erros de comunicação com a API devem ser apresentados por meio de
    uma mensagem compreensível, sem substituir o erro por dados
    fictícios.

## 5. Listagem dos alunos

### 5.1. Formato e disposição

-   Apresentar os alunos em uma única coluna.
-   Cada aluno deve ocupar praticamente toda a largura disponível.
-   Os cartões devem ser dispostos verticalmente, um abaixo do outro.
-   Não utilizar tabela nem grade de cartões com múltiplas colunas.
-   A composição deve remeter a cartões de identificação estudantil,
    combinando essa referência visual com uma interface moderna e
    limpa.
-   Os cartões devem possuir espaçamento suficiente para que cada aluno
    seja visualmente distinguível.

Diferentemente da listagem de TCCs, os cartões de alunos não devem
simular documentos sobrepostos nem utilizar efeitos de desvanecimento.

### 5.2. Estrutura interna de cada cartão

Cada cartão deve conter os seguintes elementos:

1.  **Identificação estudantil:** pequeno texto ou indicação visual que
    caracterize o cartão como uma identificação de aluno.
2.  **Nome:** apresentado com maior destaque visual, utilizando
    tamanho e peso tipográfico superiores aos demais dados.
3.  **Matrícula:** apresentada abaixo do nome.
4.  **Curso:** apresentado abaixo do nome, alinhado horizontalmente com
    a matrícula.

A composição deve estabelecer uma hierarquia clara: o nome é a
informação principal, enquanto matrícula e curso são informações
complementares de identificação.

### 5.3. Identificação estudantil

-   Apresentar uma indicação visual discreta de que o cartão representa
    uma identificação estudantil.
-   Essa indicação deve funcionar como elemento de identidade visual,
    sem competir com o nome do aluno.
-   Não utilizar números, códigos ou informações de identificação
    fictícias.
-   A indicação deve ser consistente em todos os cartões.

### 5.4. Nome do aluno

-   Apresentar o nome como elemento de maior destaque do cartão.
-   Manter contraste e legibilidade.
-   Permitir nomes longos sem sobreposição com matrícula ou curso.
-   Evitar truncamento que impeça a identificação do aluno.

### 5.5. Matrícula e curso

A matrícula e o curso devem ser apresentados na mesma região do cartão,
logo abaixo do nome, com alinhamento horizontal.

-   A matrícula deve utilizar o valor retornado no campo `matricula`.
-   O curso deve apresentar seu nome, e não apenas seu identificador
    numérico.
-   Os dois elementos devem possuir identificação textual clara.
-   A disposição deve se adaptar a telas menores sem causar
    sobreposição ou perda de legibilidade.

O nome do curso deverá ser obtido por meio da resolução do
relacionamento com a entidade Curso, conforme descrito na seção de
integração com a API.

## 6. Interação e navegação

-   A superfície inteira de cada cartão deve ser clicável.
-   Ao selecionar um cartão, navegar para a tela de visualização do
    aluno correspondente.
-   A navegação deve utilizar o identificador (`id`) retornado pela API.
-   A rota de visualização deve ser definida e integrada à arquitetura
    de rotas do frontend.
-   O cartão deve apresentar feedback visual discreto de interação,
    como alteração de fundo, sombra ou elevação no hover.
-   O comportamento deve ser acessível por teclado e apresentar
    indicação visual de foco.
-   Não adicionar botões internos que concorram com a ação principal de
    clicar no cartão.

O conteúdo e as funcionalidades da tela de visualização individual do
aluno não fazem parte desta especificação e serão definidos em
documento próprio.

## 7. Integração com a API

### 7.1. Endpoint e busca

Operação          Endpoint                              Parâmetro
  ----------------- ------------------------------------- -----------
Listar alunos     `GET /api/alunos/`                    Nenhum
Buscar alunos     `GET /api/alunos/?search={termo}`     `search`

O `AlunoViewSet` utiliza `SearchFilter` e configura os campos `nome` e
`matricula` para pesquisa.

A busca deve ser realizada pela API, respeitando a configuração
existente no backend.

### 7.2. Campos necessários

O `AlunoSerializer` utiliza `fields = '__all__'`, disponibilizando os
campos do modelo Aluno.

Informação necessária na interface   Campo da resposta
  ------------------------------------ -------------------
Identificador para navegação         `id`
Nome do aluno                        `nome`
Matrícula                            `matricula`
Identificador do curso               `curso`

O campo `curso` representa uma chave estrangeira para o modelo Curso.
Na resposta do serializer atual, ele contém o identificador do curso,
não um objeto com os dados completos da entidade relacionada.

Portanto, o frontend não deve presumir que a resposta de um aluno
contenha diretamente o nome do curso.

### 7.3. Resolução do nome do curso

Para apresentar o nome do curso nos cartões, será necessário consultar
os dados disponibilizados pelo endpoint de cursos:

`GET /api/cursos/`

O `CursoSerializer` também utiliza `fields = '__all__'`, disponibilizando
os campos do modelo Curso:

-   `id`;
-   `nome`;
-   `sigla`;
-   `codigo`.

O frontend deverá relacionar o identificador recebido no campo
`curso` do aluno ao campo `id` do respectivo curso, utilizando o campo
`nome` para apresentação no cartão.

A implementação deve considerar:

-   reaproveitamento dos dados de cursos já carregados;
-   armazenamento em memória ou cache durante a utilização da
    aplicação, quando adequado;
-   evitar uma requisição individual de cursos para cada aluno;
-   tratamento de situações em que o relacionamento não possa ser
    resolvido.

Não realizar requisições repetidas e desnecessárias para obter o mesmo
conjunto de cursos.

### 7.4. Responsabilidades do frontend

-   Consumir a listagem e a busca oferecidas pela API.
-   Apresentar os dados recebidos, sem inventar ou completar
    informações ausentes.
-   Resolver o relacionamento entre aluno e curso utilizando os
    identificadores fornecidos pelo backend.
-   Não implementar filtros locais por curso ou outros critérios como
    se fossem filtros oficiais da API.
-   Não realizar uma requisição individual de curso para cada cartão.
-   Tratar adequadamente os estados de carregamento, resultado vazio e
    erro.

A configuração de paginação deve respeitar o contrato efetivamente
utilizado pela API. A interface não deve presumir um formato de resposta
diferente daquele configurado no backend.

## 8. Estados da interface

A tela deve contemplar:

-   **Carregamento:** apresentar feedback visual enquanto a listagem ou
    uma nova busca estiver sendo processada.
-   **Sucesso:** apresentar os cartões dos alunos retornados.
-   **Lista vazia:** informar que ainda não há alunos cadastrados.
-   **Busca sem resultados:** informar que nenhum aluno corresponde ao
    termo pesquisado.
-   **Erro:** apresentar mensagem clara e permitir nova tentativa.
-   **Relacionamento não resolvido:** caso não seja possível obter o
    nome do curso associado a um aluno, não inventar um nome. A
    interface deverá tratar a ausência de maneira controlada, sem
    comprometer a apresentação dos demais dados.

Não utilizar dados fictícios em nenhum desses estados.

## 9. Responsividade e acessibilidade

-   Manter a listagem em uma única coluna em todos os tamanhos de tela.
-   Aproveitar a largura disponível sem criar colunas paralelas.
-   Adaptar espaçamentos, tipografia e alinhamento dos dados conforme a
    largura.
-   Em telas menores, permitir que matrícula e curso sejam reorganizados
    quando necessário para evitar colisões.
-   Garantir contraste adequado nos temas claro e escuro.
-   Garantir que cada cartão possa ser acionado por teclado, com foco
    visível.
-   Preservar uma área de interação confortável em dispositivos com
    tela sensível ao toque.
-   Não depender exclusivamente de elementos visuais para identificar
    matrícula e curso; utilizar também indicações textuais.

## 10. Fora do escopo desta tela

Não fazem parte desta especificação:

-   criação, edição ou exclusão de alunos;
-   alteração dos dados cadastrais;
-   visualização detalhada do aluno;
-   filtros por curso ou outros critérios não disponibilizados pelo
    backend;
-   exibição de informações pessoais adicionais;
-   exibição de telefone, e-mail, endereço ou outros dados de contato;
-   gerenciamento de cursos;
-   gerenciamento de professores, departamentos ou unidades
    acadêmicas;
-   autenticação ou gerenciamento de usuários.

Essas funcionalidades, quando necessárias, serão especificadas em telas
ou documentos próprios.

## 11. Critérios de aceite

A tela será considerada implementada quando:

1.  Reutilizar exatamente o header e o footer globais definidos no
    Dashboard.
2.  Apresentar um único campo de busca textual em toda a largura
    disponível.
3.  Pesquisar utilizando o parâmetro `search` do endpoint de alunos,
    respeitando os campos pesquisáveis do backend.
4.  Apresentar os alunos em uma única coluna, sem tabela ou grade de
    múltiplas colunas.
5.  Utilizar cartões com identidade visual inspirada em identificações
    estudantis.
6.  Apresentar o nome do aluno como elemento de maior destaque.
7.  Exibir matrícula e curso abaixo do nome, alinhados horizontalmente
    quando houver espaço disponível.
8.  Apresentar o nome do curso por meio da resolução do relacionamento
    com a entidade Curso.
9.  Evitar requisições individuais e repetidas para obter os dados dos
    cursos.
10. Tornar cada cartão inteiramente clicável e direcioná-lo à
    visualização do aluno correspondente pelo seu identificador.
11. Apresentar feedback visual de interação e foco acessível por
    teclado.
12. Tratar carregamento, lista vazia, busca sem resultados e erro.
13. Adaptar a apresentação a telas menores e aos temas claro e escuro.
14. Não apresentar filtros ou funcionalidades que a API não oferece.
15. Não utilizar dados fictícios nem informações pessoais adicionais.

## 12. Referências ao backend

Especificação conferida com base nos arquivos do backend fornecido:

-   `core/models.py`: modelo `Aluno`, com os campos `nome`,
    `matricula` e relacionamento com `Curso`; modelo `Curso`, com os
    campos `nome`, `sigla` e `codigo`.
-   `core/serializers.py`: `AlunoSerializer` e `CursoSerializer`,
    ambos utilizando `fields = '__all__'`.
-   `core/views.py`: `AlunoViewSet`, configuração de `SearchFilter` e
    campos pesquisáveis `nome` e `matricula`.
-   `core/urls.py`: registro dos recursos `alunos` e `cursos` no
    roteador da API.

------------------------------------------------------------------------

**Observação:** esta especificação descreve exclusivamente a tela de
Listagem de Alunos. As telas de cadastro, edição e visualização
individual serão documentadas separadamente.
