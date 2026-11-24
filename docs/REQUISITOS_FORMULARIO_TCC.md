# Requisitos da Tela --- Cadastro e Edição de TCCs

**Projeto:** Sistema de Gestão de Trabalhos de Conclusão de Curso (TCC)\
**Disciplina:** GAC116 --- Programação Web\
**Tecnologia do frontend:** TypeScript\
**Status:** Especificação funcional\
**Versão:** 1.0

------------------------------------------------------------------------

## 1. Objetivo

A tela deve permitir cadastrar um novo TCC e editar um TCC existente,
mantendo a identidade visual de documento acadêmico definida para a
Listagem de TCCs.

A experiência deve transmitir a sensação de preenchimento progressivo de
um trabalho acadêmico. Os campos devem aparecer na ordem em que as
informações compõem um documento, evitando a aparência de um formulário
administrativo genérico.

O formulário deve respeitar os campos, relacionamentos, escolhas e
validações efetivamente definidos pelo backend.

## 2. Estrutura global compartilhada

O header e o footer devem ser exatamente os mesmos componentes globais
definidos para o Dashboard e utilizados na Listagem de TCCs.

-   Reutilizar o header global, incluindo navegação, rota ativa e
    controle de tema.
-   Reutilizar o footer global.
-   Não criar versões específicas desses componentes nesta tela.

A área principal deve conter um único formulário em coluna, com largura
confortável para leitura e preenchimento. A aparência deve remeter a uma
folha ou documento acadêmico, com hierarquia tipográfica clara,
separadores discretos e poucos elementos decorativos.

## 3. Ordem e organização dos campos

Os campos devem ser apresentados na seguinte sequência:

1.  Título do trabalho.
2.  Aluno.
3.  Orientador.
4.  Coorientador.
5.  Presidente da banca.
6.  Primeiro membro da banca.
7.  Segundo membro da banca.
8.  Resumo.
9.  Palavras-chave.
10. Classificações do trabalho: status, tipo, idioma e semestre letivo
    de defesa.
11. Arquivo do trabalho.

A ordem deve ser preservada tanto no cadastro quanto na edição.

## 4. Identificação do trabalho e participantes

### 4.1. Título

-   Primeiro campo do formulário.
-   Campo textual de linha única.
-   Obrigatório.
-   Limite máximo: 255 caracteres, conforme o modelo do backend.
-   Apresentar contador de caracteres ou indicação do limite.

Campo da API: `titulo`.

### 4.2. Seleção de aluno e professores

Os participantes devem ser selecionados por meio de modais
centralizadas, abertas a partir dos respectivos campos no formulário. O
formulário principal não deve conter campos de busca embutidos.

Cada campo deve apresentar claramente a função da pessoa selecionada e,
após a seleção, exibir uma identificação legível da entidade escolhida.
Deve haver uma ação para abrir novamente a modal e substituir a seleção.

#### Modal de seleção

Cada modal deve conter:

-   título que identifique o tipo de entidade procurada;
-   campo de busca textual;
-   lista de resultados retornados pela API;
-   indicação de carregamento;
-   mensagem para busca sem resultados;
-   mensagem de erro e possibilidade de tentar novamente;
-   ação de seleção de um resultado;
-   ação para fechar ou cancelar sem alterar a seleção atual.

A pesquisa deve utilizar os filtros existentes no backend:

  Entidade    Endpoint                  Campos pesquisáveis
  ----------- ------------------------- ---------------------
  Aluno       `GET /api/alunos/`        `nome`, `matricula`
  Professor   `GET /api/professores/`   `nome`

Enviar o termo pelo parâmetro `search`. Não implementar pesquisa por
campos que o backend não disponibiliza.

A modal deve permitir pesquisar sem carregar previamente listas
completas de alunos ou professores. Deve respeitar a paginação e o
formato de resposta efetivamente configurados na API.

#### Campos e obrigatoriedade

  Participação               Campo da API        Obrigatoriedade
  -------------------------- ------------------- -----------------
  Aluno                      `aluno`             Obrigatório
  Orientador                 `orientador`        Obrigatório
  Coorientador               `coorientador`      Opcional
  Presidente da banca        `presidente`        Obrigatório
  Primeiro membro da banca   `primeiro_membro`   Obrigatório
  Segundo membro da banca    `segundo_membro`    Obrigatório

Cada campo deve abrir a modal correspondente: seleção de aluno para
`aluno` e seleção de professor para os demais.

O coorientador deve permitir permanecer sem seleção. Os outros cinco
relacionamentos devem ser preenchidos antes do envio, conforme a
obrigatoriedade do modelo.

A interface não deve presumir regras adicionais sobre a repetição de um
mesmo professor em diferentes funções, a menos que o backend as imponha.

## 5. Resumo

-   Apresentar uma única área `textarea`.
-   Posicionar depois da identificação e dos participantes.
-   Utilizar o campo `resumo` do modelo.
-   Disponibilizar contador de caracteres para informar o tamanho atual
    do texto.
-   Não impor limite máximo artificial: o campo é um `TextField` e o
    modelo não define limite de caracteres.
-   Manter o conteúdo digitado durante a interação com o restante do
    formulário.

O contador é informativo. Não deve bloquear a digitação com base em um
limite que não existe no contrato do backend.

## 6. Palavras-chave

O campo de palavras-chave deve proporcionar uma interação contínua, sem
exigir que o usuário pressione repetidamente um botão de adicionar.

### 6.1. Interação por vírgula

-   Disponibilizar uma área de entrada para digitação contínua.
-   Quando o usuário inserir uma vírgula, considerar concluída a
    palavra-chave anterior e iniciar a próxima.
-   A interface pode representar os termos já concluídos como pequenas
    etiquetas dentro do próprio campo ou em uma região imediatamente
    associada a ele.
-   Permitir corrigir e remover termos já inseridos.
-   Tratar espaços excedentes ao redor dos termos.
-   Evitar criar palavras-chave vazias quando houver vírgulas
    consecutivas ou espaços sem conteúdo.
-   Ao sair do campo ou enviar o formulário, considerar também o último
    termo digitado, mesmo que não tenha sido finalizado por vírgula.

### 6.2. Persistência

O backend armazena `palavras_chave` em um campo de texto, com limite de
255 caracteres. Portanto, o frontend deve serializar os termos em uma
única string, separados por vírgulas, antes de enviar à API.

O limite de 255 caracteres deve considerar a string completa armazenada,
incluindo vírgulas e espaços. A interface deve informar o limite e
impedir que a composição ultrapasse o tamanho aceito.

Na edição, o valor existente deve ser interpretado para reconstruir os
termos editáveis. Ao salvar, deve ser convertido novamente para o
formato textual esperado pelo backend.

## 7. Classificações do trabalho

Após as palavras-chave, apresentar uma seção única de classificação.
Nela devem ficar agrupados quatro conjuntos de seletores do tipo radio:

1.  Status.
2.  Tipo.
3.  Idioma.
4.  Semestre letivo de defesa.

Cada conjunto deve ter um rótulo próprio e opções visualmente
organizadas. Os grupos podem ser apresentados em sequência dentro da
mesma seção, com espaçamento e separação suficientes para facilitar a
leitura.

Os valores enviados devem ser os códigos aceitos pelo backend. Os
rótulos apresentados ao usuário devem corresponder às opções definidas
no modelo.

### 7.1. Status

Campo: `status`.

Opções:

-   Em Elaboração.
-   Enviado.
-   Aprovado.
-   Reprovado.

### 7.2. Tipo

Campo: `tipo`.

Opções:

-   Monografia.
-   Relatório de Estágio.
-   Relatório Técnico.
-   Artigo.

### 7.3. Idioma

Campo: `idioma`.

Opções:

-   Português.
-   Inglês.

### 7.4. Semestre letivo de defesa

Campo: `semestre_letivo_defesa`.

-   Apresentar as opções em radio buttons.
-   Utilizar exatamente as opções disponibilizadas pelo modelo do
    backend: 2020/1 até 2026/1.
-   O campo é opcional no backend.
-   Permitir que o usuário deixe o campo sem seleção.
-   Não criar opções de semestres futuros ou valores não definidos pelo
    backend.

A seleção do semestre deve permanecer na mesma seção visual das
classificações, conforme definido para esta tela.

## 8. Arquivo do trabalho

-   Apresentar o campo de upload por último.
-   Utilizar o campo `arquivo` da API.
-   Permitir selecionar um arquivo e exibir seu nome após a seleção.
-   Oferecer uma ação para substituir a seleção antes de salvar.
-   No cadastro, permitir que o campo permaneça vazio, pois ele é
    opcional no modelo.
-   Na edição, apresentar a informação do arquivo já associado, quando
    disponível, e permitir selecionar outro arquivo.

O modelo não define limite de tamanho nem validação de extensão.
Portanto, não impor PDF obrigatório, tamanho máximo ou outras restrições
não previstas no backend. Se houver validações adicionais no endpoint ou
na infraestrutura de armazenamento, elas deverão ser respeitadas
conforme o contrato real.

O envio de arquivo deve utilizar o formato de requisição aceito pelo
backend para upload, normalmente `multipart/form-data`, sem definir
manualmente um cabeçalho incompatível com a geração automática do
boundary pelo cliente HTTP.

## 9. Cadastro e edição

A mesma estrutura visual deve atender aos dois modos.

### 9.1. Cadastro

-   Apresentar o formulário vazio, sem dados fictícios.
-   Permitir preencher os campos na ordem especificada.
-   Validar os campos obrigatórios antes do envio.
-   Enviar os dados para a operação de criação disponibilizada pelo
    endpoint de TCCs.
-   Após sucesso, apresentar confirmação e encaminhar o usuário conforme
    o fluxo de navegação definido pela aplicação.

### 9.2. Edição

-   Carregar o TCC existente pelo identificador.
-   Preencher os campos com os valores retornados pela API.
-   Reconstruir a seleção dos participantes e as palavras-chave.
-   Apresentar o arquivo atual, quando disponível.
-   Permitir alterar os dados e substituir o arquivo.
-   Não apagar o arquivo existente apenas porque nenhum novo arquivo foi
    selecionado.
-   Enviar a atualização utilizando a operação e o formato de dados
    aceitos pelo backend.

A tela deve deixar claro se o usuário está cadastrando um novo TCC ou
editando um existente, sem alterar a identidade visual principal.

## 10. Validação e feedback

-   Validar a presença do título, aluno, orientador, presidente,
    primeiro membro e segundo membro.
-   Respeitar o limite de 255 caracteres do título.
-   Respeitar o limite de 255 caracteres da string de palavras-chave.
-   Não criar limite artificial para o resumo.
-   Permitir ausência de coorientador, semestre de defesa e arquivo.
-   Apresentar mensagens de validação próximas aos campos
    correspondentes.
-   Exibir estado de envio enquanto a operação estiver em andamento.
-   Evitar envios duplicados durante uma operação em andamento.
-   Apresentar erros retornados pela API de forma compreensível,
    associando-os aos campos quando possível.
-   Preservar os dados preenchidos caso a API retorne erro, para que o
    usuário possa corrigir e tentar novamente.

## 11. Ações do formulário

A tela deve disponibilizar ações claras ao final do formulário:

-   **Salvar:** valida e envia os dados para cadastro ou atualização.
-   **Cancelar/Voltar:** abandona a operação e retorna à tela anterior
    ou à listagem, conforme o fluxo de navegação definido.

A ação principal deve ter destaque visual. A ação secundária deve ser
discreta. Não adicionar ações administrativas que não façam parte do
escopo.

## 12. Responsividade e acessibilidade

-   Manter o formulário em uma coluna principal.
-   Adaptar espaçamentos e largura dos campos a telas menores.
-   Garantir que os grupos de radio possam ser utilizados por teclado.
-   Identificar cada campo com rótulo visível.
-   Garantir foco visível nos campos, botões e resultados das modais.
-   As modais devem permitir navegação por teclado, fechamento acessível
    e foco controlado.
-   Não depender exclusivamente de cores para indicar seleção,
    obrigatoriedade ou erro.
-   Preservar contraste adequado nos temas claro e escuro.

## 13. Integração com a API

O frontend deve utilizar os endpoints e operações disponibilizados pelo
backend do projeto:

  ----------------------------------------------------------------------------
  Finalidade                          Recurso
  ----------------------------------- ----------------------------------------
  Criar TCC                           `POST /api/tccs/`

  Consultar TCC para edição           `GET /api/tccs/{id}/`

  Atualizar TCC                       `PUT` ou `PATCH /api/tccs/{id}/`,
                                      conforme operação escolhida

  Pesquisar alunos                    `GET /api/alunos/?search={termo}`

  Pesquisar professores               `GET /api/professores/?search={termo}`
  ----------------------------------------------------------------------------

O formato de envio deve respeitar a aceitação de arquivos pelo backend.
Confirmar no serializer e na view a compatibilidade entre JSON e
multipart para as operações de criação e atualização antes de
implementar o cliente HTTP.

Não carregar previamente todos os alunos ou professores. As modais devem
realizar consultas sob demanda e respeitar paginação.

## 14. Fora do escopo

Não fazem parte desta tela:

-   gerenciamento de alunos e professores;
-   cadastro de alunos ou professores dentro das modais;
-   criação de filtros adicionais para seleção de participantes;
-   alteração das regras de negócio do backend;
-   visualização detalhada do TCC;
-   gerenciamento de cursos, departamentos ou unidades acadêmicas;
-   geração automática de resumo ou palavras-chave;
-   validações de arquivo que não estejam previstas no backend;
-   autenticação e gerenciamento de usuários.

## 15. Critérios de aceite

A tela será considerada implementada quando:

1.  Reutilizar exatamente o header e o footer globais.
2.  Apresentar o formulário com identidade visual de documento
    acadêmico.
3.  Respeitar a ordem de preenchimento definida.
4.  Utilizar modais de busca para selecionar aluno e professores.
5.  Pesquisar alunos por nome e matrícula e professores por nome, usando
    os parâmetros de busca da API.
6.  Respeitar a obrigatoriedade dos participantes e a opcionalidade do
    coorientador.
7.  Disponibilizar uma única `textarea` para o resumo, sem limite
    artificial.
8.  Permitir digitar palavras-chave continuamente, separando os termos
    pela vírgula.
9.  Respeitar o limite de 255 caracteres de `palavras_chave`,
    considerando a string completa enviada.
10. Apresentar status, tipo, idioma e semestre letivo de defesa na mesma
    seção, usando radio buttons.
11. Utilizar exatamente as opções definidas no backend.
12. Apresentar o upload de arquivo por último e respeitar sua
    opcionalidade.
13. Permitir cadastro e edição com preenchimento dos dados existentes.
14. Tratar validações, carregamento, sucesso e erros da API sem perder
    os dados preenchidos.
15. Adaptar-se a telas menores e aos temas claro e escuro.

## 16. Referências ao backend

Especificação baseada nos arquivos do backend fornecido:

-   `core/models.py`: modelo `TCC`, campos, escolhas, relacionamentos e
    limites.
-   `core/serializers.py`: `TCCSerializer` e campos expostos pela API.
-   `core/views.py`: `TCCViewSet` e operações de criação, consulta e
    atualização.
-   `core/urls.py`: registro dos recursos `tccs`, `alunos` e
    `professores`.

------------------------------------------------------------------------

**Observação:** esta especificação cobre cadastro e edição de TCCs. A
visualização detalhada do trabalho será documentada separadamente.
