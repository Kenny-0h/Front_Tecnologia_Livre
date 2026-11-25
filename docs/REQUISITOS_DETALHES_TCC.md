
# REQUISITOS_DETALHES_TCC.md

**Projeto:** Sistema de Gestão de TCCs  
**Tela:** Visualização detalhada de TCC  
**Versão:** 1.0  
**Status:** Aprovado para implementação

## 1. Objetivo

Apresentar todos os dados cadastrados de um Trabalho de Conclusão de Curso em uma página de visualização somente leitura (*read-only*), reutilizando a identidade visual e a organização da tela de Cadastro/Edição de TCCs.

A tela deve transmitir a sensação de consultar um documento acadêmico já preenchido. Não deve introduzir uma nova linguagem visual ou uma estrutura de formulário diferente.

## 2. Estrutura global

- Reutilizar exatamente o header e o footer globais definidos para o Dashboard.
- Manter o conteúdo em uma coluna, com a apresentação visual de um documento acadêmico.
- Preservar a mesma ordem das informações definida na tela de Cadastro/Edição.
- Disponibilizar um botão **Editar TCC**, que abre a tela de Cadastro/Edição no modo de edição, com os dados atuais previamente carregados.

## 3. Apresentação das informações

Todos os campos devem ser apresentados sem possibilidade de alteração. Não utilizar inputs editáveis, textareas editáveis, radio buttons interativos ou modais de seleção.

A ordem de apresentação será:

### 3.1. Identificação e participantes

1. **Título:** título completo do TCC, com destaque e alinhamento coerentes com o documento acadêmico.
2. **Aluno:** nome do aluno associado ao trabalho.
3. **Orientador:** nome do professor orientador.
4. **Coorientador:** nome do coorientador, quando cadastrado. Na ausência de coorientador, apresentar indicação discreta como “Não informado”.
5. **Presidente da banca:** nome do professor cadastrado como presidente.
6. **Primeiro membro:** nome do primeiro membro da banca.
7. **Segundo membro:** nome do segundo membro da banca.

Os participantes devem ser apresentados como informações textuais, sem controles para pesquisa ou seleção.

### 3.2. Resumo

- Exibir integralmente o resumo cadastrado.
- Preservar quebras de linha e formatação textual simples, quando presentes.
- Não aplicar limite de caracteres ou truncamento próprio da tela de cadastro.

### 3.3. Palavras-chave

- Exibir as palavras-chave cadastradas.
- A apresentação pode separar visualmente os termos, desde que preserve o conteúdo armazenado.
- Não apresentar controles de edição ou inclusão.

### 3.4. Classificações

Apresentar os seguintes valores como informações somente leitura:

- **Status:** Em Elaboração, Enviado, Aprovado ou Reprovado.
- **Tipo:** Monografia, Relatório de Estágio, Relatório Técnico ou Artigo.
- **Idioma:** Português ou Inglês.
- **Semestre letivo de defesa:** valor cadastrado, quando informado.

Utilizar os rótulos legíveis na interface. O status deve preservar a diferenciação cromática definida para a listagem:

| Status | Cor |
|---|---|
| Em Elaboração | Cinza |
| Enviado | Azul |
| Aprovado | Verde |
| Reprovado | Vermelho |

As cores são complementares ao texto e não devem ser a única forma de identificar o status.

Quando o semestre não estiver informado, apresentar “Não informado” ou indicação textual equivalente.

### 3.5. Arquivo do trabalho

- Quando houver arquivo associado, apresentar seu nome e uma ação para abri-lo ou baixá-lo.
- Quando não houver arquivo, apresentar “Nenhum arquivo anexado”.
- Não exibir controles de upload, substituição ou remoção nesta tela.
- O acesso ao arquivo deve utilizar a URL disponibilizada pela API, respeitando a configuração de mídia do backend.

## 4. Botão de edição

A tela deve apresentar um botão claramente identificado como **Editar TCC**.

Ao acioná-lo:

1. Navegar para a rota de Cadastro/Edição correspondente ao TCC selecionado.
2. Abrir o formulário no modo de edição, e não no modo de criação.
3. Identificar o registro pelo ID do TCC.
4. Carregar os dados atuais do TCC pela API e preencher previamente os campos do formulário.
5. Resolver os relacionamentos de aluno e professores para que os nomes correspondentes apareçam como seleções atuais nos respectivos campos.
6. Permitir que o usuário altere os dados e salve as modificações conforme as regras da tela de Cadastro/Edição.

A tela de visualização não deve implementar uma segunda versão do formulário. O botão deve reutilizar a tela e o fluxo definidos em `REQUISITOS_FORMULARIO_TCC.md`.

## 5. Integração com a API

### 5.1. Carregamento do TCC

- Consultar o endpoint de detalhe do TCC correspondente ao identificador recebido pela rota.
- Utilizar os dados retornados pela API como fonte de verdade.
- Exibir os rótulos de apresentação disponibilizados pelo serializer:
  - `status_display`
  - `tipo_display`
  - `idioma_display`

O `TCCSerializer` utiliza `ModelSerializer` com `fields = '__all__'`. Portanto, os relacionamentos são serializados como identificadores de suas entidades relacionadas, e não como objetos aninhados com nomes.

Para apresentar os nomes dos participantes, a implementação deverá resolver esses identificadores consultando os endpoints de Alunos e Professores ou utilizar dados já disponíveis em memória/cache de forma confiável. Não presumir que o detalhe do TCC retorna diretamente os nomes.

### 5.2. Campos relevantes do TCC

| Informação | Campo da API |
|---|---|
| Identificador | `id` |
| Título | `titulo` |
| Resumo | `resumo` |
| Palavras-chave | `palavras_chave` |
| Tipo | `tipo` / `tipo_display` |
| Idioma | `idioma` / `idioma_display` |
| Status | `status` / `status_display` |
| Aluno | `aluno` |
| Orientador | `orientador` |
| Coorientador | `coorientador` |
| Presidente | `presidente` |
| Primeiro membro | `primeiro_membro` |
| Segundo membro | `segundo_membro` |
| Semestre de defesa | `semestre_letivo_defesa` |
| Arquivo | `arquivo` |

### 5.3. Arquivo

O campo `arquivo` pode ser nulo. Quando houver valor, utilizar a URL retornada pela API para oferecer acesso ao documento.

A disponibilidade efetiva depende da configuração de armazenamento e disponibilização de mídia do backend.

## 6. Estados da interface

- **Carregamento:** apresentar indicação visual enquanto os dados do TCC e os nomes dos participantes são obtidos.
- **Sucesso:** apresentar o documento completo em modo somente leitura.
- **TCC não encontrado:** informar que o registro não foi localizado e oferecer retorno à listagem.
- **Erro de comunicação:** apresentar mensagem clara e ação para tentar novamente.
- **Relacionamento indisponível:** caso algum participante não possa ser resolvido, informar que o dado não está disponível, sem inventar nomes.
- **Sem coorientador ou semestre:** apresentar indicação discreta de que a informação não foi cadastrada.
- **Sem arquivo:** apresentar a mensagem definida na seção 3.5.

Não utilizar dados fictícios como substitutos em estados de erro ou carregamento.

## 7. Navegação

- A listagem de TCCs direciona para a visualização do registro selecionado.
- A visualização disponibiliza o botão **Editar TCC**, que direciona ao formulário em modo de edição.
- O formulário deve identificar o TCC pelo ID da rota.
- A navegação de retorno à listagem deve permanecer disponível por meio da navegação global ou de um link discreto de retorno, sem competir com o conteúdo principal.

## 8. Responsividade e acessibilidade

- Preservar a leitura confortável do documento em telas grandes e pequenas.
- Permitir quebra de títulos e nomes extensos sem sobreposição.
- Garantir contraste adequado entre textos, labels e fundo nos temas claro e escuro.
- O botão de edição e a ação de acesso ao arquivo devem possuir rótulos claros e foco visível.
- Não depender exclusivamente de cores para transmitir o status.

## 9. Fora do escopo

- Alterar dados diretamente nesta tela.
- Alterar o status do TCC nesta tela.
- Selecionar ou substituir participantes.
- Enviar, substituir ou remover arquivos.
- Implementar uma nova estrutura de formulário.
- Criar filtros ou busca nesta tela.
- Exibir dados que não sejam fornecidos pela API ou resolvidos por endpoints existentes.

## 10. Critérios de aceite

- [ ] A tela utiliza a identidade visual de documento acadêmico definida para Cadastro/Edição.
- [ ] Os dados do TCC são carregados pelo identificador da rota.
- [ ] Todas as informações cadastradas são apresentadas na ordem definida.
- [ ] Os nomes dos participantes são resolvidos corretamente a partir dos IDs retornados pela API.
- [ ] Status, tipo e idioma são apresentados com rótulos legíveis.
- [ ] O semestre é apresentado quando informado.
- [ ] O arquivo pode ser aberto ou baixado quando disponível.
- [ ] Campos e informações permanecem somente leitura.
- [ ] O botão “Editar TCC” abre o formulário existente em modo de edição.
- [ ] O formulário de edição é carregado com os dados atuais do TCC.
- [ ] Estados de carregamento, erro, registro inexistente e ausência de dados opcionais são tratados.
- [ ] A página funciona em diferentes tamanhos de tela e nos temas claro e escuro.

## 11. Referências

- `REQUISITOS_FORMULARIO_TCC.md` — estrutura, ordem dos campos e comportamento do formulário reutilizado para edição.
- `REQUISITOS_LISTAGEM_TCCS.md` — identidade visual dos documentos e cores de status.
- Backend Django REST Framework:
  - `core/models.py`
  - `core/serializers.py`
  - `core/views.py`
  - `core/urls.py`
