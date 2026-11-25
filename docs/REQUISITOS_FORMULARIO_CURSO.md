
# Requisitos — Cadastro, Edição e Visualização de Cursos

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais das telas de Cadastro, Edição e Visualização de Cursos.

As três operações deverão compartilhar a mesma identidade visual, seguindo o padrão estabelecido nas telas correspondentes de Alunos e Professores.

A interface será organizada em um cartão de identificação, apresentando os três campos próprios da entidade Curso:

- Nome.
- Sigla.
- Código.

Não existem relacionamentos com outras entidades que precisem ser apresentados nessas telas.

## 2. Estrutura geral

As telas deverão apresentar:

- Cabeçalho global compartilhado.
- Conteúdo principal organizado em um cartão de identificação.
- Campos distribuídos de maneira clara, respeitando a hierarquia visual.
- Rodapé global compartilhado.

O cartão deverá utilizar a identificação:

**IDENTIFICAÇÃO DO CURSO**

O nome deverá possuir maior destaque visual. Sigla e código deverão ser apresentados abaixo do nome, mantendo a organização estabelecida na listagem.

A estrutura visual deverá ser preservada nas três operações, alterando somente o estado dos campos e as ações disponíveis.

## 3. Campos do curso

O modelo Curso possui os seguintes campos:

| Campo | Tipo | Restrição |
|---|---|---|
| `nome` | Texto | Máximo de 255 caracteres |
| `sigla` | Texto | Máximo de 10 caracteres; único |
| `codigo` | Texto | Máximo de 50 caracteres; único |

### 3.1. Nome

O nome identifica o curso e deverá ser o elemento de maior destaque no cartão.

- Obrigatório.
- Máximo de 255 caracteres.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

### 3.2. Sigla

A sigla identifica abreviadamente o curso.

- Obrigatória.
- Máximo de 10 caracteres.
- Deve ser única entre os cursos cadastrados.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

### 3.3. Código

O código identifica o curso por meio de seu código cadastral.

- Obrigatório.
- Máximo de 50 caracteres.
- Deve ser único entre os cursos cadastrados.
- Editável no cadastro e na edição.
- Somente leitura na visualização.

## 4. Tela de visualização

A tela de visualização deverá apresentar os três campos em modo somente leitura:

- Nome do curso.
- Sigla.
- Código.

A organização deverá manter o nome em destaque e apresentar sigla e código abaixo dele.

### 4.1. Ações disponíveis

Deverá existir o botão:

- **Editar Curso:** direciona o usuário à tela de edição do curso correspondente.

A navegação deverá preservar o identificador do curso.

Não deverão existir campos editáveis na visualização.

## 5. Tela de edição

A tela de edição deverá manter a mesma estrutura visual da visualização, apresentando os valores atuais nos respectivos campos.

Os três campos deverão permitir alteração:

- Nome.
- Sigla.
- Código.

As restrições de tamanho e unicidade deverão ser respeitadas.

### 5.1. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar alterações:** envia os dados atualizados à API.
- **Cancelar:** descarta as alterações realizadas e retorna à visualização do curso.

Após uma atualização bem-sucedida, deverá ser apresentada uma indicação de sucesso e o usuário deverá retornar à visualização do curso atualizado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os valores preenchidos sempre que possível.

## 6. Tela de cadastro

A tela de cadastro deverá utilizar a mesma estrutura visual das telas de edição e visualização.

Os campos deverão ser apresentados inicialmente vazios:

- Nome.
- Sigla.
- Código.

Todos os campos deverão ser obrigatórios.

### 6.1. Ações disponíveis

A tela deverá disponibilizar:

- **Salvar cadastro:** envia os dados preenchidos à API.
- **Cancelar:** descarta os dados informados e retorna à listagem de cursos.

Após o cadastro bem-sucedido, deverá ser apresentada uma indicação de sucesso e o usuário deverá ser direcionado à visualização do curso recém-criado.

Em caso de erro, a interface deverá apresentar uma mensagem adequada e preservar os valores preenchidos sempre que possível.

## 7. Regras de comportamento por operação

| Informação | Cadastro | Edição | Visualização |
|---|---|---|---|
| Nome | Preenchimento | Editável | Somente leitura |
| Sigla | Preenchimento | Editável | Somente leitura |
| Código | Preenchimento | Editável | Somente leitura |
| Botão principal | Salvar cadastro | Salvar alterações | Editar Curso |
| Cancelar | Retorna à listagem | Retorna à visualização | Não aplicável |

## 8. Integração com o backend

Endpoint base:

`/api/cursos/`

Operações necessárias:

- `GET /api/cursos/{id}/`: recuperar um curso específico.
- `POST /api/cursos/`: cadastrar um novo curso.
- `PATCH /api/cursos/{id}/`: atualizar um curso existente.

O `CursoSerializer` utiliza `fields = '__all__'`, disponibilizando os campos do modelo.

### 8.1. Estrutura dos dados enviados

**Cadastro — POST `/api/cursos/`:**

```json
{
  "nome": "Nome do Curso",
  "sigla": "ABC",
  "codigo": "12345"
}
```

**Edição — PATCH `/api/cursos/{id}/`:**

```json
{
  "nome": "Nome atualizado",
  "sigla": "XYZ",
  "codigo": "67890"
}
```

Os exemplos são ilustrativos. Os valores deverão ser fornecidos pelo usuário.

Não deverão ser enviados campos que não pertençam ao modelo Curso.

## 9. Validações e regras de negócio

O frontend deverá considerar:

- Nome obrigatório, com até 255 caracteres.
- Sigla obrigatória, com até 10 caracteres.
- Código obrigatório, com até 50 caracteres.
- Sigla única entre os cursos.
- Código único entre os cursos.

As validações locais deverão melhorar a experiência de preenchimento, mas não substituirão as validações do backend.

Em caso de conflito de unicidade, a mensagem retornada pela API deverá ser apresentada de forma compreensível.

## 10. Estados da interface

### 10.1. Carregamento

Durante a recuperação de um curso para edição ou visualização, deverá ser apresentado um indicador de carregamento.

### 10.2. Sucesso

Após cadastro ou atualização concluídos, deverá ser apresentada uma indicação de sucesso.

### 10.3. Erro

Falhas de comunicação ou validação deverão ser apresentadas de maneira clara.

Os valores preenchidos deverão ser preservados sempre que possível.

### 10.4. Curso inexistente

Caso o identificador informado não corresponda a um curso existente, a interface deverá informar que o registro não foi encontrado e disponibilizar uma forma de retornar à listagem.

## 11. Responsividade e acessibilidade

- O cartão deverá se adaptar à largura disponível.
- Os campos deverão permanecer legíveis em dispositivos menores.
- Todos os campos deverão possuir rótulos acessíveis.
- Os botões deverão oferecer indicação visual de foco.
- A interface deverá respeitar os temas claro e escuro.
- Mensagens de validação e erro deverão ser associadas aos respectivos campos sempre que possível.

## 12. Fora do escopo

Não fazem parte destas telas:

- Cadastro ou edição de alunos.
- Associação de alunos ao curso.
- Alteração de departamentos ou unidades acadêmicas.
- Exclusão de cursos.
- Inclusão de campos não existentes no modelo.
- Busca ou filtros adicionais.
- Gerenciamento de autenticação ou usuários.

## 13. Critérios de aceite

- [ ] As três operações deverão utilizar a identidade visual compartilhada com Alunos e Professores.
- [ ] A tela deverá apresentar a identificação "IDENTIFICAÇÃO DO CURSO".
- [ ] O nome deverá possuir maior destaque visual.
- [ ] Sigla e código deverão ser apresentados abaixo do nome.
- [ ] Cadastro deverá apresentar os três campos vazios.
- [ ] Edição deverá carregar os valores atuais.
- [ ] Visualização deverá apresentar todos os campos em modo somente leitura.
- [ ] Nome, sigla e código deverão respeitar seus limites de caracteres.
- [ ] A unicidade de sigla e código deverá ser tratada.
- [ ] Cadastro deverá utilizar `POST /api/cursos/`.
- [ ] Edição deverá utilizar `PATCH /api/cursos/{id}/`.
- [ ] Visualização deverá recuperar os dados do curso correspondente.
- [ ] Os botões Salvar e Cancelar deverão executar os comportamentos definidos.
- [ ] A visualização deverá disponibilizar o botão Editar Curso.
- [ ] Os estados de carregamento, sucesso, erro e registro inexistente deverão ser tratados.
- [ ] A interface deverá funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 14. Referências ao backend

- `core/models.py`: definição do modelo `Curso`.
- `core/serializers.py`: definição do `CursoSerializer`.
- `core/views.py`: implementação do `CursoViewSet`.
- `core/urls.py`: registro da rota de cursos.

### Observação final

O frontend deverá respeitar os campos e as restrições definidos no modelo Curso. A API será responsável pela validação definitiva dos dados e pela persistência dos registros.

A prioridade é manter uma experiência consistente com as telas de Alunos e Professores, sem acrescentar informações ou operações que não estejam previstas no backend.
