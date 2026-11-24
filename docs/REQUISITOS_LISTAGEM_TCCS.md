# Requisitos da Tela --- Listagem de TCCs

**Projeto:** Sistema de Gestão de Trabalhos de Conclusão de Curso (TCC)\
**Disciplina:** GAC116 --- Programação Web\
**Tecnologia do frontend:** TypeScript\
**Status do documento:** Especificação funcional aprovada para orientar
a implementação\
**Versão:** 1.0

------------------------------------------------------------------------

## 1. Objetivo da tela

A tela de Listagem de TCCs deve permitir que o usuário localize e
reconheça os trabalhos cadastrados por meio de uma apresentação visual
diferenciada, inspirada em documentos acadêmicos organizados em
gaveteiros de bibliotecas.

A interface deve priorizar:

-   busca textual simples;
-   apresentação dos trabalhos em documentos verticais, um abaixo do
    outro;
-   identificação rápida por título, tipo, idioma e status;
-   navegação para a visualização individual de cada TCC.

A tela não deve implementar regras de negócio ou filtros que não sejam
oferecidos pelo backend.

## 2. Estrutura global compartilhada

O header e o footer devem ser exatamente os mesmos definidos para o
Dashboard. Eles são componentes globais da aplicação e não devem receber
variações específicas nesta tela.

### 2.1. Header

Reutilizar integralmente o header global, incluindo:

-   identificação do Sistema de Gestão de TCCs;
-   navegação horizontal para Dashboard e listagens;
-   ícones e labels já definidos;
-   indicação da rota ativa (`/tccs`);
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
2.  listagem vertical dos documentos de TCC.

O conteúdo deve aproveitar a largura horizontal disponível. Não utilizar
tabela convencional nem grade de cards em múltiplas colunas.

## 4. Campo de busca

### 4.1. Apresentação

-   Exibir um único campo de busca textual logo abaixo do header.
-   O campo deve ocupar toda a largura disponível da área de conteúdo.
-   Não adicionar filtros separados por tipo, idioma, status, curso ou
    outras dimensões.
-   O campo deve ter identificação ou placeholder que deixe clara sua
    finalidade.

### 4.2. Comportamento

A busca deve utilizar o mecanismo disponibilizado pelo backend:

-   endpoint: `GET /api/tccs/`;
-   parâmetro: `search`;
-   campos pesquisáveis: `titulo` e `resumo`.

A atualização da busca deve ocorrer conforme o usuário digita. A
implementação deve utilizar debounce para evitar requisições
desnecessárias durante a digitação.

O frontend não deve simular uma busca por palavras-chave, tipo, idioma
ou status. Embora essas informações sejam exibidas nos documentos, elas
não fazem parte dos campos de busca configurados no `TCCViewSet`.

### 4.3. Resultados

-   Quando houver resultados, apresentar os documentos retornados pela
    API.
-   Quando a busca não encontrar resultados, apresentar uma mensagem
    clara informando que nenhum TCC foi encontrado.
-   Quando o campo estiver vazio, apresentar a listagem sem restringir
    os resultados por texto.
-   Erros de comunicação com a API devem ser apresentados por meio de
    uma mensagem compreensível, sem substituir o erro por dados
    fictícios.

## 5. Listagem dos TCCs

### 5.1. Formato e disposição

-   Apresentar os TCCs em uma única coluna.
-   Cada TCC deve ocupar praticamente toda a largura disponível.
-   Os documentos devem ser dispostos verticalmente, um abaixo do outro.
-   Não utilizar tabela nem grade de cards com múltiplas colunas.
-   A composição deve remeter a documentos acadêmicos armazenados em
    gaveteiros de arquivo, combinando essa referência visual com uma
    interface moderna e limpa.

### 5.2. Estrutura interna de cada documento

Cada documento deve conter os seguintes elementos:

1.  **Labels de identificação:** posicionadas no canto superior direito,
    identificando tipo, idioma e status.
2.  **Título:** centralizado no cabeçalho e com destaque visual.
3.  **Separação visual:** divisor discreto entre cabeçalho e conteúdo.
4.  **Resumo:** apresentado na área principal do documento.
5.  **Palavras-chave:** posicionadas abaixo do resumo.

As labels devem permanecer legíveis e não podem ser afetadas pelo efeito
de desvanecimento do conteúdo.

### 5.3. Labels de identificação

Cada documento deve exibir três labels:

  Informação   Campo utilizado    Exemplo de apresentação
  ------------ ------------------ -------------------------
  Tipo         `tipo_display`     Monografia
  Idioma       `idioma_display`   Português
  Status       `status_display`   Em Elaboração

Utilizar os campos de apresentação retornados pelo serializer. O
frontend não deve duplicar os mapeamentos de escolhas definidos no
modelo.

#### Diferenciação visual do status

A label de status deve receber uma cor própria para facilitar o
reconhecimento visual:

  Status retornado pela API   Cor sugerida
  --------------------------- --------------
  Em Elaboração               Cinza
  Enviado                     Azul
  Aprovado                    Verde
  Reprovado                   Vermelho

As cores devem ser aplicadas de maneira discreta, coerente com os temas
claro e escuro e sem comprometer o contraste ou a legibilidade. O texto
do status deve permanecer explícito; a cor não pode ser o único meio de
transmitir a informação.

As labels de tipo e idioma devem utilizar tratamento visual mais neutro,
evitando competir com o status e com o título.

### 5.4. Título

-   Apresentar o título centralizado no cabeçalho do documento.
-   Manter contraste e legibilidade.
-   Permitir títulos longos sem sobreposição com as labels.
-   O título não deve ser cortado pelo efeito de desvanecimento.

### 5.5. Resumo e palavras-chave

-   Exibir o resumo logo abaixo do cabeçalho.
-   Exibir as palavras-chave abaixo do resumo.
-   Preservar a separação e a hierarquia visual entre os dois conteúdos.
-   Utilizar os valores retornados pela API, sem criar textos
    substitutos ou demonstrativos.

## 6. Efeito visual de desvanecimento e sobreposição

O efeito de documentos parcialmente sobrepostos é um elemento central da
identidade visual da tela.

### 6.1. Desvanecimento

-   Aplicar um efeito gradual de fade-out na região inferior do conteúdo
    do documento.
-   O efeito deve atingir somente o resumo e as palavras-chave.
-   O conteúdo deve perder opacidade gradualmente até desaparecer na
    parte inferior.
-   O título e as labels de identificação devem permanecer totalmente
    legíveis.
-   O efeito deve funcionar nos temas claro e escuro, utilizando cores
    de transição compatíveis com o fundo da interface.

### 6.2. Relação entre documentos

-   O início do documento seguinte deve ocorrer logo após a região de
    desvanecimento do documento anterior.
-   Utilizar espaçamento negativo ou outra técnica visual equivalente,
    de forma controlada, para sugerir sobreposição.
-   A sobreposição deve ser moderada: cada documento precisa continuar
    distinguível e sua área de título e labels deve permanecer
    acessível.
-   Evitar que o efeito prejudique a leitura, a interação ou a
    identificação dos documentos.

### 6.3. Adaptação a telas menores

Em larguras menores, a disposição interna deve se adaptar para evitar
colisões entre título e labels. O efeito de sobreposição deve continuar
presente, mas não pode comprometer a legibilidade nem a área clicável
dos documentos.

## 7. Interação e navegação

-   A superfície inteira de cada documento deve ser clicável.
-   Ao selecionar um documento, navegar para a tela de visualização do
    TCC correspondente.
-   A navegação deve utilizar o identificador (`id`) retornado pela API.
-   A rota de detalhes deve ser definida e integrada à arquitetura de
    rotas do frontend.
-   O documento deve apresentar feedback visual discreto de interação,
    como alteração de fundo, sombra ou elevação no hover.
-   O comportamento deve ser acessível por teclado e apresentar
    indicação visual de foco.
-   Não adicionar botões internos que concorram com a ação principal de
    clicar no documento.

O conteúdo e as funcionalidades da tela de visualização individual não
fazem parte desta especificação e serão definidos em documento próprio.

## 8. Integração com a API

### 8.1. Endpoint e busca

  Operação      Endpoint                          Parâmetro
  ------------- --------------------------------- -----------
  Listar TCCs   `GET /api/tccs/`                  Nenhum
  Buscar TCCs   `GET /api/tccs/?search={termo}`   `search`

O `TCCViewSet` utiliza `SearchFilter` e configura os campos `titulo` e
`resumo` para pesquisa.

### 8.2. Campos necessários

O `TCCSerializer` utiliza `fields = '__all__'` e acrescenta os campos
somente de leitura `status_display`, `tipo_display` e `idioma_display`.

  Informação necessária na interface   Campo da resposta
  ------------------------------------ -------------------
  Identificador para navegação         `id`
  Título                               `titulo`
  Resumo                               `resumo`
  Palavras-chave                       `palavras_chave`
  Tipo legível                         `tipo_display`
  Idioma legível                       `idioma_display`
  Status legível                       `status_display`

Os campos `tipo`, `idioma` e `status` também existem no modelo, mas a
apresentação deve priorizar os respectivos campos `*_display` fornecidos
pelo serializer.

### 8.3. Responsabilidades do frontend

-   Consumir a listagem e a busca oferecidas pela API.
-   Apresentar os dados recebidos, sem inventar ou completar informações
    ausentes.
-   Não implementar filtros locais por status, tipo ou idioma como se
    fossem filtros oficiais da API.
-   Não buscar outras entidades apenas para compor os documentos, pois
    os campos necessários à listagem já estão presentes no serializer de
    TCC.
-   Tratar adequadamente os estados de carregamento, resultado vazio e
    erro.

A configuração de paginação deve respeitar o contrato efetivamente
utilizado pela API. A interface não deve presumir um formato de resposta
diferente daquele configurado no backend.

## 9. Estados da interface

A tela deve contemplar:

-   **Carregamento:** apresentar feedback visual enquanto a listagem ou
    uma nova busca estiver sendo processada.
-   **Sucesso:** apresentar os documentos retornados.
-   **Lista vazia:** informar que ainda não há TCCs cadastrados.
-   **Busca sem resultados:** informar que nenhum trabalho corresponde
    ao termo pesquisado.
-   **Erro:** apresentar mensagem clara e permitir nova tentativa.

Não utilizar dados fictícios em nenhum desses estados.

## 10. Responsividade e acessibilidade

-   Manter a listagem em uma única coluna em todos os tamanhos de tela.
-   Aproveitar a largura disponível sem criar colunas paralelas.
-   Adaptar espaçamentos, tipografia e posicionamento das labels
    conforme a largura.
-   Garantir contraste adequado nos dois temas.
-   Não depender exclusivamente de cores para comunicar o status.
-   Garantir que cada documento possa ser acionado por teclado, com foco
    visível.
-   Preservar uma área de interação confortável em dispositivos com tela
    sensível ao toque.

## 11. Fora do escopo desta tela

Não fazem parte desta especificação:

-   criação, edição ou exclusão de TCCs;
-   alteração de status;
-   filtros por status, tipo, idioma, curso, semestre ou orientador;
-   busca por palavras-chave;
-   visualização detalhada do TCC;
-   upload ou download de arquivos;
-   gerenciamento de alunos, professores, cursos, departamentos ou
    unidades acadêmicas;
-   autenticação ou gerenciamento de usuários.

Essas funcionalidades, quando necessárias, serão especificadas em telas
ou documentos próprios.

## 12. Critérios de aceite

A tela será considerada implementada quando:

1.  Reutilizar exatamente o header e o footer globais definidos no
    Dashboard.
2.  Apresentar um único campo de busca textual em toda a largura
    disponível.
3.  Pesquisar utilizando o parâmetro `search` do endpoint de TCCs,
    respeitando os campos pesquisáveis do backend.
4.  Apresentar os trabalhos em uma única coluna, sem tabela ou grade de
    múltiplas colunas.
5.  Exibir título, resumo, palavras-chave, tipo, idioma e status com a
    hierarquia definida.
6.  Utilizar os campos de apresentação `tipo_display`, `idioma_display`
    e `status_display`.
7.  Diferenciar visualmente os quatro status com as cores aprovadas,
    mantendo o texto explícito e legível.
8.  Aplicar fade-out somente ao resumo e às palavras-chave.
9.  Criar a sensação de documentos parcialmente sobrepostos sem
    prejudicar a leitura ou a interação.
10. Tornar cada documento inteiramente clicável e direcioná-lo à
    visualização do TCC correspondente pelo seu identificador.
11. Tratar carregamento, lista vazia, busca sem resultados e erro.
12. Adaptar a apresentação a telas menores e aos temas claro e escuro.
13. Não apresentar filtros ou funcionalidades que a API não oferece.
14. Não utilizar dados fictícios.

## 13. Referências ao backend

Especificação conferida com base nos arquivos do backend fornecido:

-   `core/models.py`: modelo `TCC`, campos de conteúdo e escolhas de
    status, tipo e idioma.
-   `core/serializers.py`: `TCCSerializer`, incluindo `status_display`,
    `tipo_display` e `idioma_display`.
-   `core/views.py`: `TCCViewSet`, configuração de `SearchFilter` e
    campos pesquisáveis `titulo` e `resumo`.
-   `core/urls.py`: registro do recurso `tccs` no roteador da API.

------------------------------------------------------------------------

**Observação:** esta especificação descreve exclusivamente a tela de
Listagem de TCCs. A tela de visualização individual será documentada
separadamente.
