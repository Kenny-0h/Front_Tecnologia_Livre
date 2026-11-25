
# Requisitos — Cadastro, Edição e Visualização de Departamentos

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais das telas de Cadastro, Edição e Visualização de Departamentos do Sistema de Gestão de TCCs.

As três operações deverão compartilhar a mesma identidade visual, seguindo o padrão estabelecido nas telas correspondentes de Alunos e Professores.

A interface será organizada em duas seções principais:

1. Dados do departamento.
2. Dados da Unidade Acadêmica.

A primeira seção apresentará os campos próprios do departamento. A segunda apresentará a unidade à qual ele pertence, utilizando o relacionamento existente no backend.

## 2. Estrutura geral

As telas deverão apresentar:

- Cabeçalho global compartilhado.
- Conteúdo principal organizado em duas seções verticais.
- Cartões de identificação seguindo a identidade visual das demais entidades.
- Rodapé global compartilhado.

A primeira seção deverá utilizar a identificação:

**IDENTIFICAÇÃO DO DEPARTAMENTO**

A segunda seção deverá utilizar a identificação:

**UNIDADE ACADÊMICA**

A organização deverá ser mantida nas três operações, alterando somente o estado dos campos e os controles disponíveis.

## 3. Seção de dados do departamento

O modelo Departamento possui os seguintes campos próprios:

| Campo | Tipo | Restrição |
|---|---|---|
| `nome` | Texto | Máximo de 255 caracteres |
| `sigla` | Texto | Máximo de 10 caracteres; única |

### 3.1. Nome

O nome identifica o departamento e deverá ser o elemento de maior destaque na seção.

- Obrigatório.
- Máximo de 255 caracteres.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

### 3.2. Sigla

A sigla identifica abreviadamente o departamento.

- Obrigatória.
- Máximo de 10 caracteres.
- Deve ser única entre os departamentos cadastrados.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

## 4. Seção de dados da Unidade Acadêmica

A segunda seção deverá apresentar os dados da Unidade Acadêmica associada ao departamento.

As informações exibidas serão:

- **Nome da unidade:** elemento de maior destaque.
- **Sigla da unidade:** apresentada abaixo do nome.

O relacionamento é representado pelo campo `unidade_academica`, uma chave estrangeira obrigatória para o modelo UnidadeAcadêmica.

O serializer de Departamento retorna esse campo como ID. Portanto, o frontend deverá utilizar os dados recuperados de `/api/unidades-academicas/` para apresentar o nome e a sigla correspondentes.

### 4.1. Comportamento por operação

**Cadastro:**
- Apresentar um campo de seleção simples com as unidades acadêmicas existentes.
- Permitir a seleção de apenas uma unidade.
- Após a seleção, apresentar nome e sigla da unidade no cartão correspondente.
- Atualizar as informações exibidas quando a seleção for alterada.
- Exigir uma unidade válida antes de permitir o salvamento.

**Edição:**
- Recuperar a unidade associada ao departamento.
- Apresentar nome e sigla da unidade.
- Manter as informações em modo somente leitura.
- Não permitir alteração do relacionamento durante a edição.

**Visualização:**
- Recuperar a unidade associada ao departamento.
- Apresentar nome e sigla.
- Manter todas as informações em modo somente leitura.

## 5. Tela de visualização

A tela de visualização deverá apresentar duas seções:

### 5.1. Dados do departamento

Exibir em modo somente leitura:

- Nome do departamento.
- Sigla do departamento.

O nome deverá possuir maior destaque visual.

### 5.2. Dados da Unidade Acadêmica

Exibir em modo somente leitura:

- Nome da Unidade Acadêmica.
- Sigla da Unidade Acadêmica.

As informações deverão corresponder à unidade identificada pela FK do departamento.

### 5.3. Ações disponíveis

A seção de dados do departamento deverá disponibilizar o botão:

- **Editar Departamento:** direciona o usuário à tela de edição do departamento correspondente.

A navegação deverá preservar o identificador do departamento.

## 6. Tela de edição

A tela de edição deverá manter a mesma organização visual da visualização, permitindo a alteração dos campos próprios do departamento.

### 6.1. Dados do departamento

- Carregar o nome atual.
- Permitir a alteração do nome.
- Carregar a sigla atual.
- Permitir a alteração da sigla.
- Respeitar os limites de caracteres e a unicidade definidos pelo backend.

### 6.2. Dados da Unidade Acadêmica

- Carregar a unidade associada ao departamento.
- Apresentar nome e sigla da unidade.
- Manter os dados em modo somente leitura.
- Não disponibilizar controle para alteração da unidade.

A atualização deverá preservar o relacionamento existente.

### 6.3. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar alterações:** envia os dados próprios atualizados à API.
- **Cancelar:** descarta as alterações e retorna à visualização do departamento.

O salvamento deverá atualizar somente `nome` e `sigla`, preservando `unidade_academica`.

Após uma atualização bem-sucedida, deverá ser apresentada uma indicação de sucesso e o usuário deverá retornar à visualização do departamento atualizado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os dados preenchidos sempre que possível.

## 7. Tela de cadastro

A tela de cadastro deverá utilizar a mesma estrutura visual das telas de edição e visualização.

### 7.1. Dados do departamento

Apresentar os campos inicialmente vazios:

- Nome.
- Sigla.

Ambos deverão ser obrigatórios e respeitar as restrições definidas pelo backend.

### 7.2. Dados da Unidade Acadêmica

Apresentar um campo de seleção simples contendo as unidades acadêmicas existentes.

O campo deverá:

- Permitir a seleção de uma única unidade.
- Ser carregado a partir de `/api/unidades-academicas/`.
- Ser obrigatório.
- Apresentar as opções de forma identificável, preferencialmente pelo nome e pela sigla.

Após a seleção, o frontend deverá apresentar um cartão com:

- Nome da unidade selecionada.
- Sigla da unidade selecionada.

Caso o usuário altere a seleção, o cartão deverá ser atualizado automaticamente.

### 7.3. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar cadastro:** envia os dados preenchidos à API.
- **Cancelar:** descarta os dados informados e retorna à listagem de departamentos.

Após o cadastro bem-sucedido, deverá ser apresentada uma indicação de sucesso e o usuário deverá ser direcionado à visualização do departamento recém-criado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os valores preenchidos sempre que possível.

## 8. Regras de comportamento por operação

| Informação | Cadastro | Edição | Visualização |
|---|---|---|---|
| Nome do departamento | Preenchimento | Editável | Somente leitura |
| Sigla do departamento | Preenchimento | Editável | Somente leitura |
| Seleção de Unidade Acadêmica | Selecionável | Não disponível | Não disponível |
| Nome da unidade | Carregado após seleção | Carregado pela FK | Carregado pela FK |
| Sigla da unidade | Carregada após seleção | Carregada pela FK | Carregada pela FK |
| Dados da unidade | Exibidos após seleção | Somente leitura | Somente leitura |
| Botão principal | Salvar cadastro | Salvar alterações | Editar Departamento |
| Cancelar | Retorna à listagem | Retorna à visualização | Não aplicável |

## 9. Integração com o backend

### 9.1. Departamentos

Endpoint base:

`/api/departamentos/`

Operações necessárias:

- `GET /api/departamentos/{id}/`: recuperar um departamento específico.
- `POST /api/departamentos/`: cadastrar um departamento.
- `PATCH /api/departamentos/{id}/`: atualizar um departamento existente.

O `DepartamentoSerializer` utiliza `fields = '__all__'`.

Os campos retornados incluem:

- `id`
- `nome`
- `sigla`
- `unidade_academica`

O campo `unidade_academica` é retornado como ID.

### 9.2. Unidades Acadêmicas

Endpoint:

`GET /api/unidades-academicas/`

O `UnidadeAcademicaSerializer` disponibiliza:

- `id`
- `nome`
- `sigla`

Esses dados deverão ser utilizados para preencher o campo de seleção e resolver o relacionamento na visualização e na edição.

Sempre que possível, os dados das unidades deverão ser reutilizados ou armazenados em cache, evitando requisições repetidas desnecessárias.

### 9.3. Estrutura dos dados enviados

**Cadastro — POST `/api/departamentos/`:**

```json
{
  "nome": "Nome do Departamento",
  "sigla": "ABC",
  "unidade_academica": 1
}
```

O valor de `unidade_academica` deverá corresponder ao ID da unidade selecionada.

**Edição — PATCH `/api/departamentos/{id}/`:**

```json
{
  "nome": "Nome atualizado",
  "sigla": "XYZ"
}
```

A edição deverá atualizar somente os campos próprios do departamento, preservando o relacionamento com a Unidade Acadêmica.

Não deverão ser enviados nomes ou siglas da unidade como campos adicionais do departamento.

## 10. Validações e regras de negócio

O frontend deverá considerar:

- Nome obrigatório, com até 255 caracteres.
- Sigla obrigatória, com até 10 caracteres.
- Sigla única entre os departamentos.
- Unidade Acadêmica obrigatória no cadastro.
- Unidade selecionada deverá corresponder a um registro existente.
- Edição deverá preservar a unidade atualmente associada.
- Visualização deverá apresentar os dados correspondentes ao relacionamento retornado pela API.

As validações locais não substituem as validações do backend.

Em caso de conflito de unicidade, a mensagem retornada pela API deverá ser apresentada de forma compreensível.

## 11. Estados da interface

### 11.1. Carregamento

Durante a recuperação do departamento ou das unidades acadêmicas, deverá ser apresentado um indicador de carregamento.

No cadastro, o campo de seleção deverá permanecer indisponível até que as unidades sejam carregadas.

### 11.2. Sucesso

Após cadastro ou atualização concluídos, deverá ser apresentada uma indicação de sucesso.

### 11.3. Erro

Falhas de comunicação ou validação deverão ser apresentadas claramente.

Os valores preenchidos deverão ser preservados sempre que possível.

### 11.4. Departamento inexistente

Caso o identificador informado não corresponda a um departamento existente, a interface deverá informar que o registro não foi encontrado e disponibilizar uma forma de retornar à listagem.

### 11.5. Unidades indisponíveis

Caso não seja possível recuperar as unidades ou não existam unidades cadastradas, o cadastro deverá informar a situação e impedir o envio sem uma unidade válida.

### 11.6. Relacionamento não resolvido

Caso o departamento seja carregado, mas a unidade associada não possa ser localizada, a interface deverá indicar que os dados da unidade não estão disponíveis.

O frontend não deverá inventar informações ou substituir automaticamente o relacionamento.

## 12. Responsividade e acessibilidade

- As duas seções deverão se adaptar à largura disponível.
- Os cartões deverão manter hierarquia visual e legibilidade.
- Os campos deverão possuir rótulos acessíveis.
- Os botões deverão oferecer indicação visual de foco.
- Mensagens de erro deverão ser associadas aos campos correspondentes sempre que possível.
- A interface deverá respeitar os temas claro e escuro.

## 13. Fora do escopo

Não fazem parte destas telas:

- Cadastro ou edição de Unidades Acadêmicas.
- Alteração da unidade durante a edição de um departamento.
- Exclusão de departamentos.
- Cadastro ou edição de professores.
- Inclusão de campos não existentes no modelo.
- Busca ou filtros adicionais.
- Gerenciamento de autenticação ou usuários.

## 14. Critérios de aceite

- [ ] As três operações deverão utilizar a identidade visual compartilhada com Alunos e Professores.
- [ ] A interface deverá possuir duas seções: dados do departamento e dados da Unidade Acadêmica.
- [ ] A seção do departamento deverá apresentar nome e sigla.
- [ ] O nome deverá possuir maior destaque visual.
- [ ] A seção da unidade deverá apresentar nome e sigla.
- [ ] Cadastro deverá apresentar os campos próprios vazios.
- [ ] Cadastro deverá disponibilizar seleção simples e obrigatória de unidade.
- [ ] A seleção deverá atualizar automaticamente o cartão da unidade.
- [ ] Edição deverá carregar nome, sigla e unidade atual.
- [ ] Edição deverá permitir alterar somente nome e sigla.
- [ ] A unidade deverá permanecer somente leitura durante a edição.
- [ ] Visualização deverá apresentar as duas seções em modo somente leitura.
- [ ] O relacionamento deverá ser resolvido utilizando os IDs retornados pelas APIs.
- [ ] Cadastro deverá enviar nome, sigla e ID da unidade selecionada.
- [ ] Edição deverá preservar o relacionamento existente.
- [ ] A unicidade da sigla deverá ser tratada.
- [ ] Os botões Salvar e Cancelar deverão executar os comportamentos definidos.
- [ ] A visualização deverá disponibilizar o botão Editar Departamento.
- [ ] Os estados de carregamento, sucesso, erro e relacionamento não resolvido deverão ser tratados.
- [ ] A interface deverá funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 15. Referências ao backend

- `core/models.py`: definição dos modelos `Departamento` e `UnidadeAcademica`.
- `core/serializers.py`: definição dos serializers correspondentes.
- `core/views.py`: implementação dos ViewSets de Departamento e Unidade Acadêmica.
- `core/urls.py`: registro das rotas das duas entidades.

### Observação final

O frontend deverá respeitar a chave estrangeira obrigatória entre Departamento e Unidade Acadêmica.

Como o serializer retorna o relacionamento por ID, o frontend deverá utilizar os dados do endpoint de unidades acadêmicas para apresentar nome e sigla.

A edição deverá preservar a associação existente, enquanto o cadastro permitirá selecionar uma unidade válida.
