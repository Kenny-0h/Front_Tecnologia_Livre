
# Requisitos — Cadastro, Edição e Visualização de Unidades Acadêmicas

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais das telas de Cadastro, Edição e Visualização de Unidades Acadêmicas do Sistema de Gestão de TCCs.

As três operações deverão compartilhar a mesma identidade visual, seguindo o padrão estabelecido nas telas correspondentes de Alunos e Professores.

A interface será organizada em um único cartão de identificação, apresentando os campos próprios da entidade Unidade Acadêmica:

- Nome.
- Sigla.

Não existem relacionamentos que precisem ser apresentados nessas telas.

## 2. Estrutura geral

As telas deverão apresentar:

- Cabeçalho global compartilhado.
- Conteúdo principal organizado em um cartão de identificação.
- Campos distribuídos de maneira clara, respeitando a hierarquia visual.
- Rodapé global compartilhado.

O cartão deverá utilizar a identificação:

**IDENTIFICAÇÃO DA UNIDADE ACADÊMICA**

O nome deverá possuir maior destaque visual. A sigla deverá ser apresentada abaixo do nome.

A estrutura visual deverá ser preservada nas três operações, alterando somente o estado dos campos e as ações disponíveis.

## 3. Campos da Unidade Acadêmica

O modelo UnidadeAcademica possui os seguintes campos:

| Campo | Tipo | Restrição |
|---|---|---|
| `nome` | Texto | Máximo de 255 caracteres |
| `sigla` | Texto | Máximo de 10 caracteres; única |

### 3.1. Nome

O nome identifica a Unidade Acadêmica e deverá ser o elemento de maior destaque no cartão.

- Obrigatório.
- Máximo de 255 caracteres.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

### 3.2. Sigla

A sigla identifica abreviadamente a Unidade Acadêmica.

- Obrigatória.
- Máximo de 10 caracteres.
- Deve ser única entre as unidades acadêmicas cadastradas.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

## 4. Tela de visualização

A tela de visualização deverá apresentar os campos em modo somente leitura:

- Nome da Unidade Acadêmica.
- Sigla.

O nome deverá possuir maior destaque visual, com a sigla apresentada abaixo.

### 4.1. Ações disponíveis

Deverá existir o botão:

- **Editar Unidade Acadêmica:** direciona o usuário à tela de edição da unidade correspondente.

A navegação deverá preservar o identificador da unidade.

Não deverão existir campos editáveis na visualização.

## 5. Tela de edição

A tela de edição deverá manter a mesma estrutura visual da visualização, apresentando os valores atuais nos respectivos campos.

Os campos deverão permitir alteração:

- Nome.
- Sigla.

As restrições de tamanho e unicidade deverão ser respeitadas.

### 5.1. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar alterações:** envia os dados atualizados à API.
- **Cancelar:** descarta as alterações realizadas e retorna à visualização da Unidade Acadêmica.

Após uma atualização bem-sucedida, deverá ser apresentada uma indicação de sucesso e o usuário deverá retornar à visualização da unidade atualizada.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os valores preenchidos sempre que possível.

## 6. Tela de cadastro

A tela de cadastro deverá utilizar a mesma estrutura visual das telas de edição e visualização.

Os campos deverão ser apresentados inicialmente vazios:

- Nome.
- Sigla.

Ambos deverão ser obrigatórios e respeitar as restrições definidas pelo backend.

### 6.1. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar cadastro:** envia os dados preenchidos à API.
- **Cancelar:** descarta os dados informados e retorna à listagem de unidades acadêmicas.

Após o cadastro bem-sucedido, deverá ser apresentada uma indicação de sucesso e o usuário deverá ser direcionado à visualização da Unidade Acadêmica recém-criada.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os valores preenchidos sempre que possível.

## 7. Regras de comportamento por operação

| Informação | Cadastro | Edição | Visualização |
|---|---|---|---|
| Nome | Preenchimento | Editável | Somente leitura |
| Sigla | Preenchimento | Editável | Somente leitura |
| Botão principal | Salvar cadastro | Salvar alterações | Editar Unidade Acadêmica |
| Cancelar | Retorna à listagem | Retorna à visualização | Não aplicável |

## 8. Integração com o backend

Endpoint base:

`/api/unidades-academicas/`

Operações necessárias:

- `GET /api/unidades-academicas/{id}/`: recuperar uma unidade específica.
- `POST /api/unidades-academicas/`: cadastrar uma nova unidade.
- `PATCH /api/unidades-academicas/{id}/`: atualizar uma unidade existente.

O `UnidadeAcademicaSerializer` utiliza `fields = '__all__'`, disponibilizando os campos do modelo:

- `id`
- `nome`
- `sigla`

### 8.1. Estrutura dos dados enviados

**Cadastro — POST `/api/unidades-academicas/`:**

```json
{
  "nome": "Nome da Unidade Acadêmica",
  "sigla": "ABC"
}
```

**Edição — PATCH `/api/unidades-academicas/{id}/`:**

```json
{
  "nome": "Nome atualizado",
  "sigla": "XYZ"
}
```

Os exemplos são ilustrativos. Os valores deverão ser fornecidos pelo usuário.

Não deverão ser enviados campos que não pertençam ao modelo UnidadeAcademica.

## 9. Validações e regras de negócio

O frontend deverá considerar:

- Nome obrigatório, com até 255 caracteres.
- Sigla obrigatória, com até 10 caracteres.
- Sigla única entre as unidades acadêmicas cadastradas.

As validações locais deverão melhorar a experiência de preenchimento, mas não substituirão as validações do backend.

Em caso de conflito de unicidade, a mensagem retornada pela API deverá ser apresentada de forma compreensível.

## 10. Estados da interface

### 10.1. Carregamento

Durante a recuperação de uma Unidade Acadêmica para edição ou visualização, deverá ser apresentado um indicador de carregamento.

### 10.2. Sucesso

Após cadastro ou atualização concluídos, deverá ser apresentada uma indicação de sucesso.

### 10.3. Erro

Falhas de comunicação ou validação deverão ser apresentadas de maneira clara.

Os valores preenchidos deverão ser preservados sempre que possível.

### 10.4. Unidade inexistente

Caso o identificador informado não corresponda a uma unidade existente, a interface deverá informar que o registro não foi encontrado e disponibilizar uma forma de retornar à listagem.

## 11. Responsividade e acessibilidade

- O cartão deverá se adaptar à largura disponível.
- Os campos deverão permanecer legíveis em dispositivos menores.
- Todos os campos deverão possuir rótulos acessíveis.
- Os botões deverão oferecer indicação visual de foco.
- A interface deverá respeitar os temas claro e escuro.
- Mensagens de validação e erro deverão ser associadas aos respectivos campos sempre que possível.

## 12. Fora do escopo

Não fazem parte destas telas:

- Cadastro ou edição de departamentos.
- Associação ou gerenciamento de departamentos.
- Exclusão de unidades acadêmicas.
- Inclusão de campos não existentes no modelo.
- Busca ou filtros adicionais.
- Gerenciamento de autenticação ou usuários.

## 13. Critérios de aceite

- [ ] As três operações deverão utilizar a identidade visual compartilhada com Alunos e Professores.
- [ ] A tela deverá apresentar a identificação "IDENTIFICAÇÃO DA UNIDADE ACADÊMICA".
- [ ] O nome deverá possuir maior destaque visual.
- [ ] A sigla deverá ser apresentada abaixo do nome.
- [ ] Cadastro deverá apresentar os dois campos vazios.
- [ ] Edição deverá carregar os valores atuais.
- [ ] Visualização deverá apresentar os campos em modo somente leitura.
- [ ] Nome e sigla deverão respeitar seus limites de caracteres.
- [ ] A unicidade da sigla deverá ser tratada.
- [ ] Cadastro deverá utilizar `POST /api/unidades-academicas/`.
- [ ] Edição deverá utilizar `PATCH /api/unidades-academicas/{id}/`.
- [ ] Visualização deverá recuperar os dados da unidade correspondente.
- [ ] Os botões Salvar e Cancelar deverão executar os comportamentos definidos.
- [ ] A visualização deverá disponibilizar o botão Editar Unidade Acadêmica.
- [ ] Os estados de carregamento, sucesso, erro e registro inexistente deverão ser tratados.
- [ ] A interface deverá funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 14. Referências ao backend

- `core/models.py`: definição do modelo `UnidadeAcademica`.
- `core/serializers.py`: definição do `UnidadeAcademicaSerializer`.
- `core/views.py`: implementação do `UnidadeAcademicaViewSet`.
- `core/urls.py`: registro da rota de unidades acadêmicas.

### Observação final

O frontend deverá respeitar os campos e as restrições definidos no modelo UnidadeAcademica.

A API será responsável pela validação definitiva dos dados e pela persistência dos registros.

A prioridade é manter uma experiência consistente com as telas de Alunos e Professores, sem acrescentar informações ou operações que não estejam previstas no backend.
