# Divisão de Tarefas — Etapa 1 do Projeto

> **Objetivo:** dividir a Etapa 1 entre 3 pessoas, considerando que a análise de dados mais aprofundada ficará para uma etapa futura.

Nesta etapa, o foco é:

- entendimento das bases;
- seleção do recorte;
- pré-processamento;
- estatística descritiva básica;
- dataset final tratado;
- dicionário de dados;
- limitações;
- trabalho científico relacionado.

---

# Pessoa 1 — Entendimento das bases, CATSER e definição do recorte

## Notebook

Responsabilidades:

- explicar quais bases existem;
- definir qual base será realmente utilizada;
- explicar a relação entre `COMPRA`, `COMPRA_ITEM`, `ITEM_RESULTADO` e CATSER;
- explicar por que `COMPRA_ITEM` será a base principal;
- listar as colunas disponíveis;
- selecionar apenas as colunas importantes para o trabalho;
- identificar os grupos CATSER relevantes;
- filtrar os grupos escolhidos;
- acrescentar o código e o nome das quatro divisões oficiais do CATSER;
- manter também o código e o nome oficial do grupo CATSER;
- conferir se os registros ficaram corretamente classificados;
- mostrar o número inicial de registros e colunas.

## Divisões oficiais adotadas

| Código da divisão | Nome oficial da divisão | Grupos selecionados no trabalho |
|---|---|---|
| **11** | Serviços de desenvolvimento, manutenção e sustentação de software | 111, 112, 113, 114, 115, 116, 117 |
| **13** | Serviços de computação em nuvem | 131 |
| **16** | Serviços para a infraestrutura de Tecnologia da Informação e Comunicação (TIC) | 161, 162, 163, 164, 165 |
| **17** | Serviços de pesquisa, análise de dados e indicadores, consultoria e projetos de TIC | 172, 173 |

As quatro divisões pertencem à Seção 1 — Serviços de TIC. O recorte permanece nos 15 grupos selecionados, sem incluir automaticamente os demais grupos dessas divisões. Hospedagem (163) e gerenciamento de TIC (162) ficam na divisão 16; computação em nuvem (131) fica na divisão 13.

O CSV CATSER local não contém os campos de divisão. A Pessoa 1 deve acrescentar `codigo_divisao_catser` e `nome_divisao_catser` usando a correspondência oficial da tabela acima e conferir os grupos mapeados. Preserve os códigos e nomes de grupo e divisão.

## Relatório

Responsabilidades:

- título do projeto;
- problema investigado;
- questões de pesquisa;
- fonte dos dados;
- explicação das bases utilizadas;
- explicação do CATSER;
- explicação da escolha dos grupos;
- explicação do uso das quatro divisões oficiais na análise;
- deixar claro que Divisão e Grupo são níveis oficiais do CATSER e que a equipe definiu os grupos incluídos no recorte.

## Apresentação

Essa pessoa deve explicar principalmente:

> De onde vieram os dados, o que cada registro representa, quais colunas foram escolhidas e como foi definido o recorte de serviços de software/TIC.

---

# Pessoa 2 — Limpeza estrutural dos dados

## Notebook

Responsabilidades:

### 1. Conferir tipos das colunas

Exemplo:

```python
df.dtypes
```

Converter colunas quando necessário.

Exemplo:

```python
df["quantidade"] = pd.to_numeric(
    df["quantidade"],
    errors="coerce"
)
```

### 2. Identificar e remover duplicatas exatas

```python
df.duplicated().sum()
```

```python
df = df.drop_duplicates()
```

Justificativa:

> Registros exatamente repetidos foram removidos para evitar contagem duplicada da mesma observação.

### 3. Verificar valores ausentes

```python
df[colunas_importantes].isna().sum()
```

Também pode ser calculado o percentual de ausência por coluna.

A pessoa deve identificar:

- quais colunas possuem valores ausentes;
- quais delas são essenciais;
- quantos registros seriam perdidos caso esses valores fossem removidos.

### 4. Tratar apenas ausentes essenciais

Não preencher automaticamente com:

- média;
- mediana;
- regressão;
- valores inventados.

Quando uma informação essencial estiver ausente e não houver forma segura de recuperá-la, o registro pode ser removido da análise correspondente.

### 5. Fazer pequenas padronizações textuais

Exemplo:

```python
df["nome_grupo_catser"] = (
    df["nome_grupo_catser"]
    .str.strip()
)
```

Apenas correções simples e fáceis de justificar.

## Relatório

Responsabilidades:

- início da seção de pré-processamento;
- número de registros antes do tratamento;
- quantidade de duplicatas encontradas;
- quantidade de duplicatas removidas;
- valores ausentes encontrados;
- critérios utilizados para exclusão;
- alterações de tipo realizadas;
- padronizações simples aplicadas.

## Apresentação

Essa pessoa deve explicar principalmente:

> Como foi feita a limpeza estrutural da base: duplicatas, tipos incorretos, valores ausentes e pequenas padronizações.

---

# Pessoa 3 — Validação dos valores, dataset final, estatística descritiva e limitações

## Notebook

Responsabilidades:

### 1. Verificar valores claramente inválidos

Exemplos:

```python
df[df["quantidade"] < 0]
```

```python
df[df["valor_unitario_estimado"] < 0]
```

Caso existam valores claramente impossíveis, removê-los e registrar a decisão.

### 2. Investigar valores iguais a zero

Valores iguais a zero não devem ser removidos automaticamente.

Primeiro verificar:

- quantos existem;
- em quais colunas;
- se podem representar um caso válido;
- se podem representar ausência ou inconsistência.

Se não houver uma interpretação segura, registrar como limitação.

### 3. Identificar valores extremos

Não remover outliers automaticamente.

Usar estatísticas básicas para observar a distribuição.

Exemplo:

```python
df["quantidade"].describe()
```

```python
df["valor_unitario_estimado"].describe()
```

### 4. Fazer estatística descritiva básica

Nesta etapa, a estatística serve para descrever o dataset final, e não para realizar ainda a análise aprofundada do problema.

Pode incluir:

- quantidade de registros;
- média;
- mediana;
- mínimo;
- máximo;
- quartis;
- quantidade por divisão CATSER;
- quantidade por ano, se necessário para descrever a base.

Evitar nesta etapa:

- análise causal;
- comparação aprofundada antes/depois de 2022;
- K-Means;
- análise de clusters;
- análise detalhada de fornecedores;
- conclusões sobre impacto da IA.

Esses pontos ficam para uma etapa futura de análise de dados.

### 5. Produzir o dataset final

Responsabilidades:

- conferir número final de linhas;
- conferir número final de colunas;
- validar se as colunas importantes permanecem presentes;
- exportar o dataset tratado;
- garantir que os códigos e nomes oficiais de grupo e divisão CATSER estejam presentes.

### 6. Criar o dicionário de dados final

Exemplo de estrutura:

| Coluna | Tipo | Descrição |
|---|---|---|
| `id_compra_item` | texto | Identificador do item da contratação |
| `ano_compra` | inteiro | Ano relacionado à contratação |
| `codigo_grupo_catser` | inteiro | Código oficial do grupo CATSER |
| `nome_grupo_catser` | texto | Nome oficial do grupo CATSER |
| `codigo_divisao_catser` | texto | Código oficial da divisão CATSER: 11, 13, 16 ou 17 |
| `nome_divisao_catser` | texto | Nome oficial da divisão CATSER à qual pertence o grupo selecionado |
| `quantidade` | numérico | Quantidade associada ao item |
| `valor_unitario_estimado` | numérico | Valor unitário estimado |
| `valor_total` | numérico | Valor total associado ao item |

Os nomes e descrições devem ser ajustados conforme o dataset final real.

## Relatório

Responsabilidades:

- continuação/finalização da seção de pré-processamento;
- validações feitas nos valores;
- estatística descritiva básica;
- apresentação do dataset final;
- dicionário de dados;
- limitações do pré-processamento;
- limitações do dataset produzido.

## Limitações que podem ser mencionadas

- tratamento conservador;
- possíveis inconsistências remanescentes na base pública;
- versões diferentes de um mesmo registro podem exigir tratamento mais sofisticado;
- ausência de validação manual de todos os registros;
- valores extremos foram mantidos quando não havia evidência suficiente de erro;
- algumas inconsistências exigiriam consulta individual à fonte;
- o recorte inclui somente os 15 grupos selecionados, não todos os serviços das quatro divisões CATSER;
- 2026 pode representar um período incompleto, dependendo da data de coleta.

## Apresentação

Essa pessoa deve explicar principalmente:

> Como os valores foram validados, quais estatísticas básicas descrevem a base, qual foi o dataset final produzido e quais limitações ainda permanecem.

---

# Divisão do trabalho científico

O trabalho científico relacionado pode ser dividido entre os três.

## Pessoa 1

Responsável por:

- encontrar um artigo científico relacionado;
- registrar referência;
- identificar título;
- identificar relação com o projeto.

## Pessoa 2

Responsável por resumir:

- objetivo;
- problema de pesquisa;
- metodologia utilizada.

## Pessoa 3

Responsável por resumir:

- principais resultados;
- conclusão;
- crítica da equipe ao trabalho.

## Revisão

Os três devem revisar juntos a parte do artigo para garantir que todos saibam explicar:

- título;
- objetivo;
- problema;
- resultado;
- crítica da equipe.

---

# Fluxo de trabalho

```text
PESSOA 1
Entendimento das bases
+
seleção de colunas
+
CATSER
+
4 divisões oficiais do CATSER
        ↓
PESSOA 2
Tipos
+
duplicatas
+
valores ausentes
+
padronizações simples
        ↓
PESSOA 3
Valores inválidos
+
valores extremos
+
estatística descritiva básica
+
dataset final
+
dicionário
+
limitações
```

---

# Resumo da divisão

| Pessoa | Principal responsabilidade |
|---|---|
| Pessoa 1 | Bases + CATSER + recorte + divisões oficiais + problema e questões |
| Pessoa 2 | Tipos + duplicatas + valores ausentes + padronização |
| Pessoa 3 | Validação de valores + estatística descritiva básica + dataset final + dicionário + limitações |

---

# O que fica para a etapa futura de análise de dados

Não faz parte desta divisão atual:

- K-Means;
- definição e interpretação de clusters;
- comparação aprofundada das categorias ao longo dos anos;
- análise de crescimento ou redução após 2022;
- análise aprofundada de preços;
- análise detalhada de fornecedores;
- análise de sensibilidade;
- transformação logarítmica para modelagem;
- comparação de scalers;
- tentativa de relacionar mudanças diretamente ao avanço da IA.

A Etapa 1 deve produzir uma base tratada, compreensível e documentada para que essas análises possam ser feitas depois.

---

# Regra geral para os três

Cada integrante é responsável por desenvolver sua parte, mas todos devem saber explicar superficialmente o fluxo completo.

Antes de adicionar qualquer tratamento novo, perguntar:

1. O problema realmente existe na nossa base?
2. Esse tratamento é necessário nesta etapa?
3. Conseguimos explicar claramente o que ele faz?
4. Conseguimos justificar por que escolhemos essa regra?

Se a resposta for não, é preferível:

- simplificar;
- não aplicar o tratamento;
- registrar o problema como limitação.
