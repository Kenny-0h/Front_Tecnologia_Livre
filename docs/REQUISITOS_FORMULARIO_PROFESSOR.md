
# Requisitos — Cadastro, Edição e Visualização de Professores

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais das telas de Cadastro, Edição e Visualização de Professores do Sistema de Gestão de TCCs.

As três operações devem compartilhar a mesma identidade visual e organização estrutural, seguindo o padrão adotado nas telas correspondentes de alunos.

A interface será organizada em duas seções principais, apresentadas verticalmente:

1. Dados do professor.
2. Dados do departamento.

A principal adaptação em relação às telas de alunos está na quantidade de informações cadastrais: o modelo de Professor possui somente o campo `nome` e a associação obrigatória com um Departamento.

O frontend deverá respeitar a estrutura e as regras estabelecidas pelo backend, sem adicionar campos ou informações que não façam parte do modelo atual.

## 2. Estrutura geral das telas

As telas de Cadastro, Edição e Visualização de Professores deverão apresentar:

- Cabeçalho global compartilhado com as demais telas do sistema.
- Conteúdo principal organizado em cartões de identificação.
- Duas seções verticais: dados do professor e dados do departamento.
- Rodapé global compartilhado com as demais telas.

A identidade visual deverá ser consistente com as telas de Cadastro, Edição e Visualização de Alunos.

As duas seções deverão possuir hierarquia visual própria, permitindo identificar claramente as informações do professor e do departamento associado.

### 2.1. Organização das seções

A primeira seção será dedicada aos dados do professor e deverá apresentar o nome como informação principal.

A segunda seção será dedicada aos dados do departamento, apresentando seu nome e sua sigla.

A disposição deverá permanecer a mesma nas três operações, independentemente de os campos estarem editáveis ou somente para leitura.

## 3. Seção de dados do professor

A seção de dados do professor deverá seguir a identidade visual dos cartões de identificação utilizados no sistema.

O modelo atual de Professor possui somente uma informação cadastral armazenada diretamente:

- **Nome:** campo textual que identifica o professor.

O nome deverá ser apresentado como elemento de maior destaque na seção.

### 3.1. Comportamento por operação

**Cadastro:**
- Apresentar o campo de nome inicialmente vazio.
- Permitir que o usuário informe o nome do professor.
- O campo deverá ser obrigatório.
- Respeitar o limite de 255 caracteres definido no modelo do backend.

**Edição:**
- Apresentar o nome atual do professor.
- Permitir a alteração do nome.
- Respeitar o limite de 255 caracteres definido no modelo do backend.

**Visualização:**
- Apresentar o nome do professor em modo somente leitura.
- Não disponibilizar campos editáveis nessa operação.

Não deverão ser adicionados outros campos cadastrais à seção, pois eles não fazem parte do modelo atual da entidade Professor.

## 4. Seção de dados do departamento

A segunda seção deverá apresentar as informações do departamento ao qual o professor está associado.

A seção deverá seguir a proposta visual utilizada na identificação dos departamentos na listagem de professores.

As informações apresentadas serão:

- **Nome do departamento:** elemento de maior destaque.
- **Sigla do departamento:** apresentada abaixo do nome.

O modelo Professor possui uma chave estrangeira obrigatória para Departamento. O relacionamento deverá ser utilizado para recuperar as informações correspondentes.

### 4.1. Comportamento por operação

**Cadastro:**
- Apresentar um campo de seleção simples para escolha de um departamento existente.
- Permitir a seleção de apenas um departamento.
- Após a seleção, apresentar o nome e a sigla do departamento no cartão.
- Atualizar as informações exibidas sempre que o usuário selecionar outro departamento.
- Não permitir o salvamento enquanto nenhum departamento válido estiver selecionado.

**Edição:**
- Recuperar o departamento associado ao professor.
- Apresentar o nome e a sigla do departamento atual.
- Manter as informações em modo somente leitura.
- Não disponibilizar controle para alteração do departamento.

**Visualização:**
- Recuperar o departamento associado ao professor.
- Apresentar o nome e a sigla do departamento.
- Manter todas as informações em modo somente leitura.

A seção deverá manter a mesma aparência nas três operações, alterando apenas o comportamento do campo de seleção no cadastro.

## 5. Tela de visualização de professores

A tela de visualização deverá apresentar os dados completos do professor em duas seções:

1. Dados do professor.
2. Dados do departamento.

Todas as informações deverão ser exibidas em modo somente leitura.

### 5.1. Informações apresentadas

**Dados do professor:**
- Nome do professor.

**Dados do departamento:**
- Nome do departamento.
- Sigla do departamento.

O nome e as informações do departamento deverão ser recuperados a partir dos dados retornados pela API e da associação existente entre as entidades.

### 5.2. Ações disponíveis

A seção de dados do professor deverá disponibilizar o botão:

- **Editar Professor:** direciona o usuário para a tela de edição do professor correspondente.

A navegação deverá preservar o identificador do professor selecionado.

Não deverão existir campos editáveis ou ações de alteração direta na tela de visualização.

## 6. Tela de edição de professores

A tela de edição deverá manter a mesma organização visual da tela de visualização, mas permitindo a alteração do nome do professor.

### 6.1. Campos e comportamentos

**Dados do professor:**
- Carregar o nome atual do professor.
- Permitir a edição do nome.
- Respeitar o limite de 255 caracteres.

**Dados do departamento:**
- Carregar o departamento associado ao professor.
- Apresentar o nome e a sigla do departamento.
- Manter os dados em modo somente leitura.
- Não permitir a alteração do relacionamento durante a edição.

### 6.2. Ações disponíveis

A tela deverá disponibilizar os seguintes botões:

- **Salvar alterações:** envia a atualização do nome do professor para a API.
- **Cancelar:** descarta as alterações realizadas e retorna à tela de visualização do professor.

O salvamento deverá atualizar somente o campo `nome`, preservando o departamento associado.

Após uma atualização bem-sucedida, o usuário deverá receber uma indicação de sucesso e ser direcionado à tela de visualização do professor atualizado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os dados preenchidos para que o usuário possa corrigir ou tentar novamente.

## 7. Tela de cadastro de professores

A tela de cadastro deverá utilizar a mesma organização visual das telas de edição e visualização, mantendo as duas seções de identificação.

### 7.1. Dados do professor

A primeira seção deverá apresentar:

- Campo de nome inicialmente vazio.
- Possibilidade de preenchimento pelo usuário.

O campo deverá ser obrigatório e respeitar o limite de 255 caracteres.

### 7.2. Dados do departamento

A segunda seção deverá apresentar um campo de seleção simples contendo os departamentos existentes.

O campo deverá:

- Permitir a seleção de um único departamento.
- Ser preenchido a partir dos dados recuperados da API de departamentos.
- Ser obrigatório.
- Apresentar as opções de forma identificável, preferencialmente pelo nome do departamento e sua sigla.

Após a seleção, o frontend deverá apresentar um cartão com:

- Nome do departamento selecionado.
- Sigla do departamento selecionado.

Caso o usuário altere a seleção, o cartão deverá ser atualizado automaticamente para refletir o novo departamento.

### 7.3. Ações disponíveis

A tela deverá disponibilizar os seguintes botões:

- **Salvar cadastro:** envia os dados preenchidos para a API.
- **Cancelar:** descarta os dados informados e retorna à listagem de professores.

O cadastro deverá enviar o nome do professor e o identificador do departamento selecionado.

Após o cadastro bem-sucedido, o usuário deverá receber uma indicação de sucesso e ser direcionado à tela de visualização do professor recém-criado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os dados preenchidos sempre que possível.

## 8. Regras de comportamento por operação

| Informação | Cadastro | Edição | Visualização |
|---|---|---|---|
| Nome do professor | Preenchimento | Editável | Somente leitura |
| Seleção de departamento | Selecionável | Não disponível | Não disponível |
| Nome do departamento | Carregado após seleção | Carregado pela FK | Carregado pela FK |
| Sigla do departamento | Carregada após seleção | Carregada pela FK | Carregada pela FK |
| Dados do departamento | Exibidos após seleção | Somente leitura | Somente leitura |
| Botão principal | Salvar cadastro | Salvar alterações | Editar Professor |
| Cancelar | Retorna à listagem | Retorna à visualização | Não aplicável |

## 9. Integração com o backend

A implementação deverá utilizar os endpoints disponibilizados pelo Django REST Framework.

### 9.1. Professores

Endpoint base:

`/api/professores/`

Operações necessárias:

- `GET /api/professores/{id}/`: recuperar os dados de um professor específico.
- `POST /api/professores/`: cadastrar um novo professor.
- `PATCH /api/professores/{id}/`: atualizar os dados de um professor existente.

O serializer de Professor utiliza `fields = '__all__'`.

Portanto, os dados retornados incluem o identificador do professor, o nome e o identificador do departamento associado.

O campo `departamento` é retornado como ID, e não como objeto contendo nome e sigla.

### 9.2. Departamentos

Endpoint base:

`/api/departamentos/`

Operação necessária:

- `GET /api/departamentos/`: recuperar os departamentos existentes para preencher o campo de seleção e resolver as informações do relacionamento.

O serializer de Departamento também utiliza `fields = '__all__'`.

Os dados disponibilizados incluem:

- `id`
- `nome`
- `sigla`
- `unidade_academica`

Para esta funcionalidade, serão utilizados somente `id`, `nome` e `sigla`.

### 9.3. Relacionamento entre professor e departamento

O relacionamento entre Professor e Departamento é representado por uma chave estrangeira obrigatória.

Como o serializer de Professor retorna o departamento como identificador, o frontend deverá utilizar os dados recuperados do endpoint de departamentos para localizar e apresentar o nome e a sigla correspondentes.

A implementação deverá evitar a realização de uma requisição individual para cada professor ou departamento quando os dados já estiverem disponíveis em memória.

Sempre que possível, a lista de departamentos deverá ser reutilizada ou armazenada em cache durante a navegação.

### 9.4. Estrutura dos dados enviados

**Cadastro — POST `/api/professores/`:**

```json
{
  "nome": "Nome do Professor",
  "departamento": 1
}
```

O valor de `departamento` deverá corresponder ao ID do departamento selecionado.

**Edição — PATCH `/api/professores/{id}/`:**

```json
{
  "nome": "Nome Atualizado"
}
```

A edição deverá atualizar somente o nome, preservando o relacionamento existente com o departamento.

O frontend não deverá enviar o nome ou a sigla do departamento como campos adicionais do professor.

## 10. Validações e regras de negócio

As seguintes regras deverão ser respeitadas:

- O nome do professor é obrigatório.
- O nome deverá respeitar o limite de 255 caracteres.
- O departamento é obrigatório no cadastro.
- O departamento selecionado deverá corresponder a um registro existente.
- A edição deverá preservar o departamento atualmente associado.
- A visualização deverá apresentar os dados recuperados do backend.
- As validações realizadas no frontend não substituem as validações do backend.

A API deverá permanecer como fonte de verdade para as regras de persistência e integridade dos relacionamentos.

## 11. Estados da interface

As telas deverão contemplar os seguintes estados:

### 11.1. Carregamento

Durante a recuperação dos dados do professor ou dos departamentos, a interface deverá apresentar um indicador de carregamento adequado.

No cadastro, o campo de seleção deverá permanecer indisponível até que os departamentos sejam carregados.

### 11.2. Sucesso

Após uma operação concluída com sucesso, deverá ser apresentada uma indicação visual correspondente.

No cadastro e na edição, o sucesso deverá ser seguido pelo redirecionamento definido para cada operação.

### 11.3. Erro

Caso ocorra uma falha na comunicação com a API, a interface deverá apresentar uma mensagem clara.

Os dados já preenchidos pelo usuário deverão ser preservados sempre que possível.

### 11.4. Professor inexistente

Caso o identificador informado não corresponda a um professor existente, a interface deverá apresentar uma mensagem adequada e disponibilizar uma forma de retornar à listagem.

### 11.5. Departamentos indisponíveis

Caso não seja possível recuperar os departamentos ou não existam departamentos cadastrados, o cadastro de professores deverá informar a situação ao usuário e impedir o envio de um cadastro sem departamento válido.

### 11.6. Relacionamento não resolvido

Caso o professor seja carregado, mas o departamento associado não possa ser localizado na lista recuperada, a interface deverá apresentar uma indicação de que os dados do departamento não estão disponíveis, sem inventar informações ou substituir o relacionamento automaticamente.

## 12. Responsividade e acessibilidade

As telas deverão funcionar adequadamente em diferentes tamanhos de tela.

Os cartões de identificação deverão se adaptar à largura disponível, preservando a hierarquia visual e a legibilidade das informações.

Os campos e botões deverão possuir identificação adequada, permitindo sua utilização por tecnologias assistivas.

Os estados de carregamento, sucesso e erro deverão ser apresentados de maneira perceptível e compreensível.

A interface também deverá respeitar os temas claro e escuro disponibilizados pelo sistema.

## 13. Fora do escopo

Não fazem parte desta funcionalidade:

- Cadastro ou edição de departamentos.
- Alteração do departamento durante a edição de um professor.
- Cadastro de informações adicionais do professor.
- Inclusão de matrícula, e-mail, telefone ou outros dados não existentes no modelo.
- Gerenciamento de usuários ou autenticação.
- Exclusão de professores.
- Alteração de dados da unidade acadêmica a partir da tela de professores.
- Implementação de filtros ou mecanismos de busca não disponibilizados pelo backend.

## 14. Critérios de aceite

A funcionalidade será considerada implementada quando:

- [ ] As telas de Cadastro, Edição e Visualização apresentarem a mesma identidade visual das telas correspondentes de alunos.
- [ ] As informações estiverem organizadas em duas seções: dados do professor e dados do departamento.
- [ ] A seção do professor apresentar somente o nome.
- [ ] O nome do professor puder ser preenchido no cadastro.
- [ ] O nome do professor puder ser alterado na edição.
- [ ] O nome do professor for apresentado em modo somente leitura na visualização.
- [ ] O cadastro disponibilizar um campo de seleção simples para departamentos.
- [ ] A seleção de departamento apresentar nome e sigla no cartão correspondente.
- [ ] A alteração da seleção atualizar automaticamente os dados exibidos.
- [ ] A edição apresentar o departamento atual em modo somente leitura.
- [ ] A visualização apresentar o departamento associado em modo somente leitura.
- [ ] O relacionamento entre professor e departamento for resolvido corretamente utilizando os IDs retornados pela API.
- [ ] O cadastro enviar o nome e o ID do departamento selecionado.
- [ ] A edição atualizar somente o nome, preservando o departamento.
- [ ] Os botões Salvar e Cancelar executarem os comportamentos definidos.
- [ ] A navegação entre listagem, cadastro, edição e visualização funcionar corretamente.
- [ ] Os estados de carregamento, sucesso e erro forem tratados adequadamente.
- [ ] A interface funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 15. Referências ao backend

A implementação deverá considerar os seguintes arquivos do projeto backend:

- `core/models.py`: definição dos modelos Professor e Departamento e do relacionamento entre eles.
- `core/serializers.py`: definição dos serializers utilizados nas operações da API.
- `core/views.py`: implementação dos ViewSets responsáveis pelas operações de professores e departamentos.
- `core/urls.py`: registro das rotas disponibilizadas pela API.

### Observação final

O frontend deverá respeitar a estrutura atual do backend e utilizar os relacionamentos existentes para apresentar as informações necessárias.

A interface não deverá criar campos, associações ou regras de negócio que não estejam previstas nos modelos e endpoints disponibilizados.

A prioridade é manter a consistência visual e comportamental com as telas de alunos, adaptando a apresentação e as operações às informações efetivamente disponíveis para a entidade Professor.
