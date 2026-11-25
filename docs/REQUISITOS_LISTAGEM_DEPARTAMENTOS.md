
# Requisitos — Listagem de Departamentos

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais da tela de Listagem de Departamentos do Sistema de Gestão de TCCs.

A tela deverá seguir o padrão visual e de interação estabelecido nas listagens de Alunos e Professores, utilizando um campo único de busca e cartões de identificação individuais organizados verticalmente.

Cada cartão deverá apresentar o nome e a sigla do departamento, além do nome da Unidade Acadêmica à qual ele pertence.

Como Departamento possui uma chave estrangeira para Unidade Acadêmica, o frontend deverá resolver esse relacionamento utilizando os dados disponibilizados pela API.

## 2. Estrutura geral da tela

A tela deverá apresentar:

- Cabeçalho global compartilhado.
- Campo de busca textual logo abaixo do cabeçalho.
- Lista de cartões de identificação de departamentos, dispostos verticalmente em uma única coluna.
- Rodapé global compartilhado.

Não deverão ser utilizadas tabelas convencionais nem grades com múltiplas colunas.

A identidade visual deverá ser consistente com as listagens de Alunos e Professores.

## 3. Campo de busca

Deverá existir um único campo de busca textual, ocupando toda a largura disponível da página.

Como o `DepartamentoViewSet` não configura filtros de busca no backend, a pesquisa deverá ser realizada localmente no frontend.

### 3.1. Comportamento

- A busca deverá considerar exclusivamente o campo `nome` do departamento.
- A comparação deverá ignorar diferenças entre letras maiúsculas e minúsculas.
- A digitação deverá atualizar os resultados apresentados.
- A busca não deverá enviar parâmetros `search` ou outros filtros à API.
- A filtragem deverá utilizar os departamentos já carregados, evitando novas requisições a cada alteração do texto.

Os campos `sigla` e `unidade_academica` não deverão ser considerados na busca.

## 4. Cartões de identificação

A listagem deverá apresentar grandes cartões individuais, organizados verticalmente em uma única coluna.

Cada cartão deverá seguir a identidade visual de identificação utilizada nas telas de Alunos e Professores.

### 4.1. Identidade visual

O cartão deverá apresentar a identificação:

**IDENTIFICAÇÃO DO DEPARTAMENTO**

A organização das informações deverá ser:

- **Nome do departamento:** elemento de maior destaque.
- **Sigla:** apresentada abaixo do nome.
- **Unidade Acadêmica:** apresentada ao lado da sigla.

Exemplo ilustrativo:

```text
IDENTIFICAÇÃO DO DEPARTAMENTO

Nome do departamento

Sigla: XXX             Unidade: Nome da Unidade
```

O nome e a sigla deverão vir dos dados do departamento.

O nome da Unidade Acadêmica deverá ser obtido por meio da resolução da chave estrangeira `unidade_academica`.

### 4.2. Relacionamento com Unidade Acadêmica

O serializer de Departamento retorna o campo `unidade_academica` como identificador, não como objeto aninhado.

O frontend deverá consultar `/api/unidades-academicas/` e relacionar os registros utilizando seus IDs.

A implementação deverá evitar uma requisição individual para cada departamento.

Sempre que possível, a lista de unidades deverá ser carregada uma única vez e reutilizada ou armazenada em cache.

### 4.3. Interação

O cartão inteiro deverá ser clicável.

Ao selecionar um cartão, o usuário deverá ser direcionado à tela de visualização do departamento correspondente.

A navegação deverá preservar o identificador do departamento selecionado.

## 5. Integração com o backend

### 5.1. Departamentos

Endpoint:

`GET /api/departamentos/`

O `DepartamentoSerializer` utiliza `fields = '__all__'`, retornando:

- `id`
- `nome`
- `sigla`
- `unidade_academica`

O campo `unidade_academica` corresponde ao ID da unidade associada.

O `DepartamentoViewSet` não configura filtros de busca. Portanto, a filtragem pelo nome deverá ser local no frontend.

### 5.2. Unidades Acadêmicas

Endpoint:

`GET /api/unidades-academicas/`

O `UnidadeAcademicaSerializer` disponibiliza:

- `id`
- `nome`
- `sigla`

Esses dados deverão ser utilizados para resolver o relacionamento e apresentar o nome da unidade no cartão do departamento.

Não deverão ser realizadas requisições individuais por departamento quando os dados das unidades já estiverem carregados.

## 6. Estados da interface

### 6.1. Carregamento

Durante a recuperação dos departamentos e das unidades acadêmicas necessárias para resolver os relacionamentos, deverá ser apresentado um indicador de carregamento.

A listagem não deverá apresentar informações incompletas como se estivessem carregadas corretamente.

### 6.2. Listagem vazia

Caso a API não retorne departamentos cadastrados, deverá ser apresentada uma mensagem informando que não existem departamentos disponíveis.

### 6.3. Busca sem resultados

Caso existam departamentos cadastrados, mas nenhum corresponda ao texto pesquisado, deverá ser apresentada uma mensagem específica de ausência de resultados.

### 6.4. Erro

Caso ocorra uma falha na comunicação com qualquer uma das APIs necessárias, deverá ser apresentada uma mensagem clara e uma opção para tentar novamente.

### 6.5. Relacionamento não resolvido

Caso um departamento possua um ID de Unidade Acadêmica que não possa ser localizado nos dados carregados, a interface deverá indicar que a informação da unidade não está disponível.

O frontend não deverá inventar nomes, siglas ou associações.

## 7. Responsividade e acessibilidade

- Os cartões deverão ocupar a largura disponível, respeitando os limites visuais definidos pelo sistema.
- A disposição deverá permanecer em uma única coluna.
- Nome, sigla e unidade deverão permanecer legíveis em dispositivos menores.
- O campo de busca deverá possuir identificação acessível.
- Os cartões deverão oferecer interação por teclado e indicação visual de foco.
- A interface deverá respeitar os temas claro e escuro.

## 8. Fora do escopo

Não fazem parte desta tela:

- Cadastro, edição ou exclusão de departamentos.
- Busca por sigla ou Unidade Acadêmica.
- Filtros adicionais.
- Paginação ou ordenação não previstas na especificação.
- Cadastro ou alteração de Unidades Acadêmicas.
- Apresentação de professores ou TCCs relacionados.
- Criação de filtros no backend.

## 9. Critérios de aceite

- [ ] A tela deverá utilizar o cabeçalho e rodapé globais.
- [ ] Deverá existir um único campo de busca textual em largura total.
- [ ] A busca deverá filtrar localmente pelo nome do departamento.
- [ ] A busca deverá ignorar diferenças entre maiúsculas e minúsculas.
- [ ] Os cartões deverão ser apresentados verticalmente em uma única coluna.
- [ ] Cada cartão deverá apresentar a identificação "IDENTIFICAÇÃO DO DEPARTAMENTO".
- [ ] O nome deverá ser o elemento de maior destaque.
- [ ] A sigla deverá ser apresentada abaixo do nome.
- [ ] O nome da Unidade Acadêmica deverá aparecer ao lado da sigla.
- [ ] Os dados dos departamentos deverão ser obtidos de `/api/departamentos/`.
- [ ] Os dados das unidades deverão ser obtidos de `/api/unidades-academicas/`.
- [ ] O relacionamento deverá ser resolvido utilizando os IDs retornados pelas APIs.
- [ ] Não deverão ser realizadas requisições individuais para cada departamento.
- [ ] O cartão inteiro deverá direcionar à visualização do departamento correspondente.
- [ ] Os estados de carregamento, listagem vazia, busca sem resultados, erro e relacionamento não resolvido deverão ser tratados.
- [ ] A tela deverá funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 10. Referências ao backend

- `core/models.py`: definição dos modelos `Departamento` e `UnidadeAcademica`.
- `core/serializers.py`: definição dos serializers correspondentes.
- `core/views.py`: implementação dos ViewSets de Departamento e Unidade Acadêmica.
- `core/urls.py`: registro das rotas das duas entidades.

### Observação final

O frontend deverá respeitar o relacionamento existente entre Departamento e Unidade Acadêmica. Como a API retorna o relacionamento por ID, será necessário resolver essa associação utilizando os dados do endpoint de unidades acadêmicas.

A busca será realizada localmente pelo nome do departamento, pois o backend não oferece filtros para essa entidade.
