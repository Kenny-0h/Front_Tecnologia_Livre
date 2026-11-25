
# Requisitos — Listagem de Unidades Acadêmicas

**Projeto:** Sistema de Gestão de TCCs  
**Disciplina:** Programação Web (GAC116)  
**Tecnologia do frontend:** TypeScript  
**Backend:** Django REST Framework  
**Status:** Especificação definida  
**Versão:** 1.0

---

## 1. Objetivo

Este documento especifica os requisitos funcionais e visuais da tela de Listagem de Unidades Acadêmicas do Sistema de Gestão de TCCs.

A tela deverá seguir o padrão visual e de interação estabelecido nas listagens de Alunos e Professores, utilizando um campo único de busca e cartões de identificação individuais organizados verticalmente.

A listagem deverá apresentar os dados próprios da entidade Unidade Acadêmica, sem necessidade de consultar outras entidades para complementar as informações.

## 2. Estrutura geral da tela

A tela deverá apresentar:

- Cabeçalho global compartilhado com as demais telas.
- Campo de busca textual logo abaixo do cabeçalho.
- Lista de cartões de identificação de unidades acadêmicas, dispostos verticalmente em uma única coluna.
- Rodapé global compartilhado com as demais telas.

Não deverão ser utilizadas tabelas convencionais nem grades com múltiplas colunas.

A identidade visual deverá ser consistente com as listagens de Alunos e Professores.

## 3. Campo de busca

Deverá existir um único campo de busca textual, ocupando toda a largura disponível da página.

Como o `UnidadeAcademicaViewSet` não configura filtros de busca no backend, a pesquisa deverá ser realizada localmente no frontend.

### 3.1. Comportamento

- A busca deverá considerar exclusivamente o campo `nome` da unidade.
- A comparação deverá ignorar diferenças entre letras maiúsculas e minúsculas.
- A digitação deverá atualizar os resultados apresentados.
- A busca não deverá enviar parâmetros `search` ou outros filtros à API.
- A filtragem deverá utilizar as unidades já carregadas, evitando novas requisições a cada alteração do texto.

O campo `sigla` não deverá ser considerado na busca.

## 4. Cartões de identificação

A listagem deverá apresentar grandes cartões individuais, organizados verticalmente em uma única coluna.

Cada cartão deverá seguir a identidade visual de identificação utilizada nas telas de Alunos e Professores.

### 4.1. Identidade visual

O cartão deverá apresentar a identificação:

**IDENTIFICAÇÃO DA UNIDADE ACADÊMICA**

A organização das informações deverá ser:

- **Nome da Unidade Acadêmica:** elemento de maior destaque.
- **Sigla:** apresentada abaixo do nome.

Exemplo ilustrativo:

```text
IDENTIFICAÇÃO DA UNIDADE ACADÊMICA

Nome da Unidade Acadêmica

Sigla: XXX
```

Os valores deverão ser obtidos diretamente dos dados retornados pela API.

### 4.2. Interação

O cartão inteiro deverá ser clicável.

Ao selecionar um cartão, o usuário deverá ser direcionado à tela de visualização da Unidade Acadêmica correspondente.

A navegação deverá preservar o identificador da unidade selecionada.

## 5. Integração com o backend

Endpoint:

`GET /api/unidades-academicas/`

O `UnidadeAcademicaSerializer` utiliza `fields = '__all__'`, retornando os campos próprios da entidade:

- `id`
- `nome`
- `sigla`

Não é necessário consultar outras entidades para complementar os dados apresentados nos cartões.

O `UnidadeAcademicaViewSet` não configura filtros de busca. Portanto, a filtragem pelo nome deverá permanecer local no frontend.

## 6. Estados da interface

### 6.1. Carregamento

Durante a recuperação das unidades acadêmicas, deverá ser apresentado um indicador de carregamento adequado.

### 6.2. Listagem vazia

Caso a API não retorne unidades cadastradas, deverá ser apresentada uma mensagem informando que não existem unidades acadêmicas disponíveis.

### 6.3. Busca sem resultados

Caso existam unidades cadastradas, mas nenhuma corresponda ao texto pesquisado, deverá ser apresentada uma mensagem específica de ausência de resultados.

### 6.4. Erro

Caso ocorra uma falha na comunicação com a API, deverá ser apresentada uma mensagem clara, acompanhada de uma opção para tentar novamente.

## 7. Responsividade e acessibilidade

- Os cartões deverão ocupar a largura disponível, respeitando os limites visuais definidos pelo sistema.
- A disposição deverá permanecer em uma única coluna.
- Nome e sigla deverão permanecer legíveis em dispositivos menores.
- O campo de busca deverá possuir identificação acessível.
- Os cartões deverão oferecer interação por teclado e indicação visual de foco.
- A interface deverá respeitar os temas claro e escuro do sistema.

## 8. Fora do escopo

Não fazem parte desta tela:

- Cadastro, edição ou exclusão de unidades acadêmicas.
- Busca por sigla.
- Filtros adicionais.
- Paginação ou ordenação não previstas na especificação.
- Consulta a outras entidades para complementar os cartões.
- Apresentação de departamentos relacionados.
- Criação de filtros no backend.

## 9. Critérios de aceite

- [ ] A tela deverá utilizar o cabeçalho e rodapé globais.
- [ ] Deverá existir um único campo de busca textual em largura total.
- [ ] A busca deverá filtrar localmente pelo nome da unidade.
- [ ] A busca deverá ignorar diferenças entre maiúsculas e minúsculas.
- [ ] Os cartões deverão ser apresentados verticalmente em uma única coluna.
- [ ] Cada cartão deverá apresentar a identificação "IDENTIFICAÇÃO DA UNIDADE ACADÊMICA".
- [ ] O nome deverá ser o elemento de maior destaque.
- [ ] A sigla deverá ser apresentada abaixo do nome.
- [ ] Os dados deverão ser obtidos diretamente de `/api/unidades-academicas/`.
- [ ] Não deverão ser realizadas requisições adicionais para complementar os cartões.
- [ ] O cartão inteiro deverá direcionar à visualização da unidade correspondente.
- [ ] Os estados de carregamento, listagem vazia, busca sem resultados e erro deverão ser tratados.
- [ ] A tela deverá funcionar em diferentes tamanhos de tela e nos temas claro e escuro.

## 10. Referências ao backend

- `core/models.py`: definição do modelo `UnidadeAcademica`.
- `core/serializers.py`: definição do `UnidadeAcademicaSerializer`.
- `core/views.py`: implementação do `UnidadeAcademicaViewSet`.
- `core/urls.py`: registro da rota de unidades acadêmicas.

### Observação final

O frontend deverá respeitar os campos efetivamente disponibilizados pelo modelo UnidadeAcadêmica.

A busca será realizada localmente pelo nome, pois o backend não oferece filtros para essa entidade.

A prioridade é manter a consistência visual e comportamental com as demais listagens do sistema.
