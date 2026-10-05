# Contexto do Projeto — Tópicos / ComprasGov, CATSER e Clusterização

> **Objetivo deste arquivo:** servir como referência pessoal e como contexto para novos chats sobre o projeto.  
> A prioridade definida nesta conversa é **simplificar o trabalho para que todo o código, tratamento e análise sejam compreensíveis e explicáveis na apresentação**, mesmo que isso signifique deixar tratamentos avançados como limitações do trabalho.

---

## 1. Situação atual do projeto

O trabalho analisa contratações públicas federais relacionadas a serviços de software/TIC utilizando dados do ComprasGov e a classificação CATSER.

### Título atual do trabalho

> **Como a administração pública federal gasta com serviços de software dada a crescente melhora de ferramentas de inteligência artificial (2021–2026)**

Foi identificado que o título atual pode não combinar perfeitamente com o recorte dos dados, porque os grupos selecionados incluem não apenas desenvolvimento de software, mas também:

- computação em nuvem;
- hospedagem;
- infraestrutura de TIC;
- gerenciamento de TIC;
- integração de sistemas;
- pesquisa e consultoria em TIC.

### Possível título mais coerente

> **Análise das contratações de serviços de software e TIC pela administração pública federal entre 2021 e 2026**

Também foi sugerido preferir **“contrata”** a **“gasta”**, pois os dados utilizados podem representar valores estimados, resultados/homologações e contratações, não necessariamente a execução orçamentária efetivamente paga.

---

## 2. Problema principal identificado

O notebook atual atende a muitos requisitos e possui um tratamento relativamente completo, porém ficou **complexo demais para ser explicado com segurança na apresentação**.

A prioridade daqui para frente não é fazer o pipeline mais sofisticado possível.

A prioridade é:

1. entender as bases;
2. entender as colunas;
3. decidir quais informações são realmente importantes;
4. organizar os grupos selecionados nas quatro divisões oficiais do CATSER;
5. aplicar apenas tratamentos de dados óbvios e justificáveis;
6. fazer uma análise exploratória clara;
7. só depois aplicar clusterização;
8. conseguir explicar cada decisão tomada.

### Princípio central

> **É melhor entregar uma análise mais simples que eu consiga explicar por completo do que uma análise muito sofisticada cujo código e decisões eu não consiga defender.**

O uso de IA para auxiliar na escrita de código não deve resultar em um notebook que o grupo não consiga compreender.

---

# 3. Entendimento das bases

No recorte atual do ComprasGov existem três níveis principais:

```text
COMPRA
   1
   |
   N
COMPRA_ITEM
   1
   |
   N
ITEM_RESULTADO
```

Em termos simples:

- uma **COMPRA** pode possuir vários itens;
- cada **COMPRA_ITEM** representa um item daquela contratação;
- um item pode possuir um ou mais registros de **ITEM_RESULTADO**, por exemplo resultados relacionados a fornecedores.

Além dessas bases existe o **CATSER**, usado para entender e classificar os serviços.

---

## 3.1 COMPRA

É a base de nível mais geral.

Ela contém informações sobre a contratação como um todo, como:

- identificação da compra;
- órgão;
- modalidade;
- objeto;
- datas;
- valor total estimado;
- valor total homologado;
- situação da contratação.

Ela pode ser útil para perguntas específicas, mas **não precisa ser integrada inteira ao início da análise**.

---

## 3.2 COMPRA_ITEM

É a base mais importante para o trabalho simplificado.

Ela contém os itens individuais das contratações e já possui grande parte das informações necessárias.

Exemplos de campos relevantes:

- `id_compra`
- `id_compra_item`
- `descricao_resumida`
- `descricao_detalhada`
- `material_ou_servico`
- `codigo_grupo`
- `codigo_classe`
- `cod_item_catalogo`
- `unidade_medida`
- `quantidade`
- `valor_unitario_estimado`
- `valor_total`
- informações de resultado/fornecedor presentes no recorte utilizado.

### Estratégia preferida

Começar principalmente com:

```text
COMPRA_ITEM + CATSER
```

e somente buscar dados da tabela `COMPRA` ou `ITEM_RESULTADO` se alguma pergunta da análise realmente exigir.

Isso evita merges e regras de integração desnecessárias logo no começo.

---

## 3.3 CATSER

O CATSER é o catálogo oficial utilizado para classificar serviços.

A hierarquia contém níveis como:

```text
Seção
  ↓
Divisão
  ↓
Grupo
  ↓
Classe
  ↓
Subclasse
  ↓
Serviço/item
```

### Observação importante

No notebook e em algumas explicações anteriores foi usado o termo **“classes CATSER”**, mas o recorte principal atualmente está sendo feito por **grupos CATSER**.

Portanto, durante a apresentação, evitar dizer que os códigos `111`, `112`, `131` etc. são classes.

Eles são **grupos**.

---

# 4. Grupos CATSER atualmente selecionados

O notebook atual utiliza 15 grupos dentro da seção de TIC.

## Desenvolvimento, manutenção e sustentação de software

- **111** — Serviços de desenvolvimento e manutenção de software
- **112** — Serviços de manutenção e sustentação de software
- **113** — Serviços de documentação de software
- **114** — Serviços de engenharia de requisitos de software
- **115** — Serviços de mensuração de software
- **116** — Serviços de qualidade de software
- **117** — Serviço de implementação ágil de software

## Computação em nuvem

- **131** — Serviços de computação em nuvem

## Infraestrutura de TIC

- **161** — Serviços especializados de instalação, transição, configuração/customização de software
- **162** — Serviços de gerenciamento em TIC
- **163** — Serviços de hospedagem em TIC
- **164** — Serviços de integração de sistemas em TIC
- **165** — Serviços para infraestrutura de TIC não classificados em outros tópicos

## Pesquisa e consultoria

- **172** — Serviços de pesquisa, análise e desenvolvimento em TIC
- **173** — Serviços de consultoria em TIC

---

# 5. Problema de trabalhar diretamente com os 15 grupos

Usar todos os 15 grupos separadamente deixa a análise:

- mais difícil de visualizar;
- mais difícil de explicar;
- mais fragmentada;
- sujeita a categorias com poucos registros;
- mais complexa para gráficos e comparação temporal.

Além disso, vários desses grupos representam atividades muito próximas do ponto de vista da pergunta do trabalho.

Por isso, a análise será organizada pelas **quatro divisões oficiais do CATSER** às quais pertencem os grupos selecionados.

---

# 6. Quatro divisões oficiais do CATSER

**Decisão adotada:** utilizar o nível **Divisão**, que fica acima de Grupo na hierarquia oficial, para organizar as categorias da análise. Todas as divisões selecionadas pertencem à Seção 1 — Serviços de Tecnologia da Informação e Comunicação (TIC).

| Código da divisão | Nome oficial da divisão | Grupos selecionados no trabalho |
|---|---|---|
| **11** | Serviços de desenvolvimento, manutenção e sustentação de software | 111, 112, 113, 114, 115, 116, 117 |
| **13** | Serviços de computação em nuvem | 131 |
| **16** | Serviços para a infraestrutura de Tecnologia da Informação e Comunicação (TIC) | 161, 162, 163, 164, 165 |
| **17** | Serviços de pesquisa, análise de dados e indicadores, consultoria e projetos de TIC | 172, 173 |

Preserve o código e o nome oficial do grupo junto do código e do nome da divisão. O grupo 163 (hospedagem) e o grupo 162 (gerenciamento de TIC) pertencem à divisão 16; o grupo 131 pertence à divisão 13.

O recorte continua limitado aos **15 grupos já selecionados**. Organizar a análise pelas quatro divisões não inclui automaticamente todos os outros grupos dessas divisões.

## Campos da divisão na base

A [documentação da API](7-api-docs.json) apresenta os campos `codigoDivisao` e `nomeDivisao` no catálogo de serviços. O CSV CATSER local contém grupo, classe e serviço, mas não contém os campos da divisão. Para o recorte selecionado, acrescente a correspondência grupo → divisão conforme a tabela acima.

Na base do projeto, os nomes propostos são `codigo_divisao_catser` e `nome_divisao_catser`. Eles representam a classificação oficial e devem ser documentados no dicionário de dados.

### Explicação para apresentação

Caso o professor pergunte de onde vieram as categorias usadas na análise:

> “Usamos o nível Divisão da hierarquia oficial do CATSER para organizar os 15 grupos selecionados em quatro categorias. Mantivemos os códigos e nomes oficiais de grupo e divisão na base.”

O nível de classificação e os grupos incluídos no recorte devem ficar explícitos no relatório e na apresentação.

---

# 7. Colunas que realmente importam

A nova versão do trabalho deve evitar carregar dezenas de colunas só porque elas existem.

## 7.1 Colunas principais para a análise

Uma proposta enxuta é manter principalmente:

```text
id_compra
id_compra_item

ano_compra

cod_item_catalogo
codigo_grupo_catser
nome_grupo_catser
codigo_divisao_catser
nome_divisao_catser

quantidade
valor_unitario_estimado
valor_total

cod_fornecedor
nome_fornecedor
valor_total_resultado
```

Os nomes exatos devem ser conferidos na base final utilizada.

---

## 7.2 Colunas úteis apenas para consulta/contexto

Também pode ser útil manter, sem necessariamente usá-las no modelo:

```text
descricao_resumida
descricao_detalhada
unidade_medida
orgao_entidade_cnpj
```

Essas colunas podem ajudar a investigar manualmente registros estranhos.

Exemplo:

> “Por que este item possui valor tão elevado?”

Nesse caso, consultar `descricao_resumida` ou `descricao_detalhada` ajuda a entender o registro.

---

# 8. Filosofia do tratamento de dados

O notebook atual possui tratamento relativamente sofisticado, incluindo preocupações com:

- versões diferentes do mesmo item;
- registros atualizados em datas diferentes;
- conflitos entre atualizações;
- integração entre bases;
- tratamentos específicos de valores;
- `Decimal`;
- regras para resultados;
- outliers;
- transformações logarítmicas;
- comparação entre diferentes escalonadores.

Isso pode ser tecnicamente válido, mas exige muitas decisões difíceis de defender.

A nova estratégia será um **tratamento conservador e simples**.

---

# 9. Tratamentos que devem ser mantidos

## 9.1 Remover duplicatas exatas

Exemplo:

```python
df = df.drop_duplicates()
```

### Justificativa

> “Removemos registros completamente repetidos para evitar contar exatamente a mesma observação mais de uma vez.”

Importante: isso resolve apenas **duplicatas exatas**.

Não significa que todas as possíveis versões ou duplicidades lógicas da base foram resolvidas.

---

## 9.2 Verificar valores ausentes

Primeiro apenas observar:

```python
df[colunas_importantes].isna().sum()
```

Quando uma análise exigir determinadas colunas, registros ausentes nessas colunas podem ser excluídos daquela análise.

Exemplo:

```python
df = df.dropna(
    subset=[
        "quantidade",
        "valor_unitario_estimado",
        "nome_divisao_catser"
    ]
)
```

### Justificativa

> “Como essas informações eram necessárias para a análise e não havia uma forma segura de inferi-las, optamos por não preencher valores artificialmente.”

---

## 9.3 Não fazer imputação sofisticada

Evitar preencher valores faltantes com:

- média;
- mediana;
- regressão;
- valores calculados artificialmente;

a menos que exista uma justificativa muito forte.

Isso criaria uma decisão adicional difícil de defender.

---

## 9.4 Converter tipos de dados quando necessário

Exemplo:

```python
df["ano_compra"] = pd.to_numeric(
    df["ano_compra"],
    errors="coerce"
)

df["quantidade"] = pd.to_numeric(
    df["quantidade"],
    errors="coerce"
)

df["valor_unitario_estimado"] = pd.to_numeric(
    df["valor_unitario_estimado"],
    errors="coerce"
)
```

### Justificativa

> “As colunas foram convertidas para tipos numéricos porque seriam utilizadas em cálculos e análises estatísticas.”

---

## 9.5 Remover valores claramente impossíveis

Exemplo básico:

```python
df = df[
    (df["quantidade"] >= 0) &
    (df["valor_unitario_estimado"] >= 0)
]
```

### Justificativa

> “Quantidades ou preços negativos não fazem sentido dentro da interpretação adotada para essas contratações.”

### Atenção com zero

Não remover automaticamente valores iguais a zero.

Um zero pode:

- representar um caso real;
- representar ausência codificada;
- representar alguma particularidade do processo.

Antes de removê-lo, deve-se entender o significado.

Se não for possível entender, registrar como limitação.

---

## 9.6 Padronizações textuais óbvias

Se necessário:

```python
df["nome_grupo_catser"] = (
    df["nome_grupo_catser"]
    .str.strip()
)
```

Isso remove espaços extras e é simples de explicar.

---

# 10. O que NÃO precisa ser tratado nesta versão

Não é necessário resolver automaticamente todos os problemas existentes na base.

Problemas complexos podem ser reconhecidos e registrados como limitações.

Exemplos:

- versões diferentes do mesmo registro;
- conflitos entre datas de atualização;
- diferentes registros para o mesmo item;
- inconsistências entre quantidade, preço unitário e valor total;
- correções manuais de contratos;
- conflitos entre resultados;
- situações de cancelamento muito específicas;
- regras sofisticadas de consolidação;
- tratamento individual de todas as anomalias.

### Como explicar

> “Durante a exploração foram identificadas inconsistências mais complexas que exigiriam regras específicas ou validação individual na fonte. Para evitar introduzir correções arbitrárias, optamos por não corrigi-las automaticamente e registrá-las como limitação do trabalho.”

Essa postura é preferível a aplicar uma regra que o grupo não compreende completamente.

---

# 11. Outliers

Outliers não devem ser removidos automaticamente.

Em compras públicas, valores muito elevados podem representar contratações reais de grande porte.

## Estratégia inicial

Observar estatísticas:

```python
df["valor_unitario_estimado"].describe()
```

Também podem ser utilizados:

- mediana;
- quartis;
- IQR;
- boxplot;
- histogramas.

### Explicação possível

> “Foram encontrados valores extremos, mas um valor elevado não necessariamente representa erro em uma base de contratações públicas. Por isso, não removemos esses registros automaticamente.”

---

# 12. Como a análise de dados pode mitigar um tratamento simples

A etapa de análise pode ajudar a avaliar se problemas restantes estão realmente afetando as conclusões.

Ela não substitui totalmente a limpeza, mas pode aumentar a robustez da interpretação.

---

## 12.1 Média versus mediana

Se a distribuição possui valores extremos, comparar:

- média;
- mediana.

Se a média estiver muito acima da mediana, existe forte assimetria.

Nesse caso, a mediana pode representar melhor um valor típico.

---

## 12.2 Quartis e IQR

Quartis permitem mostrar a dispersão dos valores sem depender tanto dos extremos.

Podem ser usados para descrever a distribuição, mesmo que os outliers permaneçam na base.

---

## 12.3 Análise de sensibilidade

A ideia é repetir uma análise sob cenários simples e verificar se a conclusão muda muito.

Exemplo conceitual:

```text
Cenário A:
todos os registros válidos

Cenário B:
sem os registros extremamente altos

Resultado:
a tendência geral continua semelhante
```

Nesse caso, pode-se dizer:

> “A tendência observada permaneceu semelhante mesmo quando os casos extremos foram retirados em uma análise de sensibilidade.”

Isso aumenta a confiança no resultado.

---

## 12.4 Informar o tamanho da amostra utilizada

Quando registros com valores ausentes forem excluídos, mostrar quantos registros permaneceram.

Exemplo:

> “A análise utilizou 92% dos registros disponíveis para essas variáveis.”

Não usar esse número sem recalculá-lo na base real.

---

## 12.5 Transformação logarítmica

O notebook atual utiliza `log1p`.

A transformação logarítmica **não deve entrar automaticamente na nova versão**.

Ela pode ser usada futuramente somente se ficar evidente que:

- os dados são extremamente assimétricos;
- valores muito grandes estão dominando os gráficos ou o K-Means;
- o grupo conseguir explicar por que a transformação foi utilizada.

### Explicação simples, se for necessária

> “Aplicamos log para reduzir a diferença de escala entre valores muito pequenos e muito grandes sem simplesmente excluir as contratações de maior valor.”

Se não houver necessidade clara, não usar.

---

# 13. O que a análise NÃO consegue corrigir

Análise exploratória não resolve erros estruturais automaticamente.

Exemplos:

- se um mesmo registro estiver duplicado logicamente várias vezes, ele continuará influenciando as contagens;
- se uma contratação estiver classificada incorretamente na fonte, o gráfico não descobrirá sozinho;
- se valores estiverem errados na origem, estatística não transforma esses valores em corretos.

Portanto, a estratégia é:

```text
Problemas óbvios e seguros de corrigir
→ tratar

Problemas ambíguos
→ investigar quando necessário

Problemas complexos sem solução segura
→ documentar como limitação
```

---

# 14. Tratamento mínimo definido para o novo notebook

A ideia pode ser resumida em três perguntas principais.

## Pergunta 1 — Existem linhas exatamente repetidas?

Se sim:

```python
drop_duplicates()
```

---

## Pergunta 2 — Faltam dados essenciais para a análise?

Se sim:

- contar os ausentes;
- excluir apenas quando necessário;
- informar quantos registros foram retirados.

---

## Pergunta 3 — Existem valores claramente impossíveis?

Exemplos:

- quantidade negativa;
- preço negativo.

Se sim:

- remover;
- registrar o critério.

---

# 15. K-Means: simplificação proposta

O notebook atual trabalha com:

```text
quantidade
valor_unitario_estimado
valor_total
```

Porém:

```text
valor_total ≈ quantidade × valor_unitario
```

Portanto, utilizar as três variáveis pode introduzir redundância.

## Proposta inicial

Começar o K-Means somente com:

```text
quantidade
valor_unitario_estimado
```

Isso facilita muito a explicação.

### Interpretação

> “O K-Means agrupa itens que possuem comportamento semelhante em relação à quantidade contratada e ao valor unitário.”

---

# 16. Vantagem de usar apenas duas variáveis no K-Means

Com duas variáveis, os clusters podem ser visualizados diretamente em um gráfico 2D:

```text
valor unitário
       ↑
       |
       |         cluster C
       |
       |    cluster B
       |
       | cluster A
       |
       +--------------------→ quantidade
```

Isso torna possível mostrar visualmente:

- onde os itens estão;
- onde os grupos se formaram;
- como os clusters se diferenciam.

Depois, é possível analisar:

> Quais divisões CATSER aparecem mais em cada cluster?

Assim, a divisão CATSER não precisa necessariamente entrar diretamente como variável do K-Means.

Ela pode ser usada **depois para interpretar os clusters**.

---

# 17. Escalonamento

Como quantidade e preço podem possuir escalas muito diferentes, algum escalonamento provavelmente será necessário antes do K-Means.

Porém, o notebook atual compara várias técnicas:

- `StandardScaler`;
- `MinMaxScaler`;
- `RobustScaler`.

Isso é mais complexo do que o necessário neste momento.

### Estratégia preferida

Quando chegar a essa etapa:

1. entender por que o K-Means precisa de escala;
2. escolher **uma técnica simples**;
3. justificar a escolha;
4. evitar uma comparação extensa entre vários scalers se ela não for requisito.

Essa decisão ainda pode ser tomada futuramente com base na distribuição real dos dados.

---

# 18. Fluxo ideal do novo notebook

O notebook deve seguir uma sequência muito mais curta e pedagógica.

```text
1. Entender as bases
        ↓
2. Carregar COMPRA_ITEM
        ↓
3. Carregar CATSER
        ↓
4. Selecionar apenas as colunas importantes
        ↓
5. Filtrar os 15 grupos CATSER
        ↓
6. Acrescentar e conferir as 4 divisões oficiais do CATSER
        ↓
7. Remover duplicatas exatas
        ↓
8. Verificar valores ausentes
        ↓
9. Verificar valores claramente inválidos
        ↓
10. Fazer análise exploratória
        ↓
11. Entender distribuições e possíveis outliers
        ↓
12. Fazer K-Means simples
        ↓
13. Interpretar clusters usando as divisões CATSER
```

---

# 19. O que evitar no novo notebook

Evitar adicionar código apenas porque parece “mais completo”.

Principalmente evitar, a menos que seja realmente necessário:

- dezenas de colunas;
- merges grandes antes de saber se são necessários;
- regras específicas para muitos casos excepcionais;
- correções manuais difíceis de explicar;
- várias técnicas de tratamento concorrentes;
- comparação excessiva entre scalers;
- imputação sofisticada;
- remoção automática de outliers;
- transformações matemáticas sem necessidade observada;
- código produzido por IA que o grupo não consegue explicar.

---

# 20. Filosofia metodológica para a apresentação

A melhor forma de descrever a estratégia não é:

> “Não conseguimos tratar os dados direito.”

A formulação correta é:

> “Adotamos um tratamento conservador, corrigindo apenas inconsistências evidentes e evitando alterações automáticas quando não havia uma regra segura para determinar o valor correto.”

Depois:

> “Problemas mais complexos identificados na base foram registrados como limitações e considerados durante a interpretação dos resultados.”

Isso é metodologicamente defensável.

---

# 21. Possível fala sobre tratamento

Uma fala curta para apresentação:

> “Como estamos trabalhando com uma base administrativa pública, identificamos alguns problemas de qualidade. Optamos por um tratamento conservador: removemos duplicatas exatas, verificamos valores ausentes e retiramos apenas valores claramente inválidos. Casos mais complexos, como versões diferentes do mesmo registro ou inconsistências que exigiriam validação individual, não foram corrigidos automaticamente para evitar introduzir suposições nos dados.”

---

# 22. Possível fala sobre outliers

> “Valores muito altos não foram considerados automaticamente como erros, porque grandes contratações podem ser legítimas. Por isso, analisamos a distribuição utilizando medidas como mediana e quartis e deixamos tratamentos mais agressivos apenas para análises de sensibilidade.”

---

# 23. Possível fala sobre divisões CATSER

> “Os 15 grupos selecionados pertencem a quatro divisões oficiais do CATSER. Usamos essas divisões para facilitar a comparação entre tipos de serviço, preservando também os códigos e nomes dos grupos. A escolha dos grupos incluídos no recorte foi feita pela equipe.”

---

# 24. Possível fala sobre clusterização

> “Para manter o modelo interpretável, usamos poucas variáveis numéricas. O objetivo do K-Means não é prever uma categoria, mas identificar grupos de contratações com comportamentos semelhantes em quantidade e preço. Depois observamos quais tipos de serviço aparecem em cada cluster.”

---

# 25. Perguntas originais do trabalho

As perguntas definidas inicialmente são:

1. Quais padrões de contratos/serviços de software podem ser identificados por clusterização?
2. Quais categorias apresentaram maior crescimento ou redução de participação ao longo do período?
3. Como os preços se comportaram ao longo do tempo e entre categorias?
4. Houve mudanças no perfil das contratações após 2022, considerando volume, categorias, preços e fornecedores?
5. Quais fornecedores possuem mais contratações com o serviço público federal?

Essas perguntas continuam como referência, mas **não é necessário tentar responder todas com um pipeline extremamente sofisticado**.

Se necessário, elas podem ser simplificadas futuramente.

---

# 26. Possível recorte analítico mais simples

Uma versão mais controlada do trabalho pode se concentrar em:

### Pergunta A
Como a participação das quatro divisões CATSER no recorte de software/TIC mudou entre 2021 e 2026?

### Pergunta B
Como quantidade e preço unitário se distribuem entre as quatro divisões CATSER?

### Pergunta C
Quais padrões de quantidade e preço podem ser identificados com K-Means?

### Pergunta D
Quais divisões CATSER aparecem com maior frequência em cada cluster?

Perguntas sobre fornecedores podem ser mantidas como análise descritiva separada, caso exista tempo.

---

# 27. Relação com inteligência artificial

A ideia original menciona a expansão de ferramentas de inteligência artificial generativa e procura investigar mudanças nas contratações após 2022.

É importante não afirmar causalidade sem evidência.

Evitar conclusões como:

> “A IA causou a redução do gasto com desenvolvimento de software.”

Preferir:

> “Foi observada uma mudança após 2022 no período em que ferramentas de IA generativa passaram a se popularizar.”

ou:

> “Os dados permitem comparar o comportamento antes e depois de 2022, mas não demonstram sozinhos que a IA foi a causa das mudanças.”

Esse cuidado deixa a interpretação mais correta.

---

# 28. Limitações que podem ser assumidas explicitamente

É aceitável declarar que o trabalho possui limitações.

Exemplos:

- tratamento conservador;
- possíveis inconsistências remanescentes na base pública;
- ausência de validação manual de todos os registros;
- algumas versões de registros podem exigir tratamento mais sofisticado;
- o recorte inclui apenas os 15 grupos selecionados, não todos os serviços das quatro divisões CATSER;
- valores extremos foram mantidos quando não havia evidência de erro;
- a análise é observacional;
- não é possível concluir causalidade entre adoção de IA e mudanças nas contratações;
- o período de 2026 pode estar incompleto dependendo da data de coleta;
- nem toda contratação de TIC representa estritamente desenvolvimento de software.

---

# 29. Regra para decisões futuras

Antes de adicionar qualquer tratamento ou técnica nova, perguntar:

### 1.
**Eu consigo explicar em uma ou duas frases o que esse código faz?**

### 2.
**Eu consigo explicar por que ele é necessário para responder à pergunta do trabalho?**

### 3.
**Se o professor perguntar por que escolhemos essa regra e não outra, eu sei responder?**

Se a resposta for **não**, existem três opções preferíveis:

- simplificar;
- retirar;
- declarar como limitação.

---

# 30. Contrato de simplicidade do projeto

Para evitar que o notebook volte a crescer sem controle:

## Bases

Preferência inicial:

```text
COMPRA_ITEM + CATSER
```

Outras bases somente quando necessárias.

## Classificação

```text
15 grupos CATSER oficiais
        ↓
4 divisões oficiais do CATSER: 11, 13, 16 e 17
```

## Tratamento

Principalmente:

```text
duplicatas exatas
ausentes essenciais
tipos numéricos
valores impossíveis
pequenas padronizações textuais
```

## Outliers

```text
identificar
descrever
não remover automaticamente
```

## Clusterização

Inicialmente:

```text
quantidade
+
valor_unitario_estimado
```

## Complexidade

Não adicionar técnicas avançadas apenas para deixar o trabalho “mais completo”.

---

# 31. O que fazer na próxima etapa

Antes de reconstruir o notebook inteiro:

1. abrir somente a base que será usada;
2. listar todas as colunas;
3. entender o significado de cada coluna candidata;
4. escolher aproximadamente 10–12 colunas;
5. confirmar quais delas possuem muitos valores ausentes;
6. confirmar os 15 grupos CATSER presentes;
7. acrescentar e validar os códigos e nomes das quatro divisões CATSER;
8. contar registros por divisão CATSER;
9. contar registros por ano;
10. observar quantidade, preço unitário e valor total;
11. somente então definir o tratamento definitivo;
12. depois começar o K-Means.

---

# 32. Instruções para um novo chat

Ao usar este arquivo como contexto em outro chat, a orientação principal é:

> **Não reintroduzir complexidade automaticamente.**

O objetivo não é reconstruir o notebook atual da forma mais sofisticada possível.

O objetivo é construir uma versão **pedagógica, simples e defensável**.

### O novo chat deve priorizar

- explicar cada etapa antes de programar;
- escrever código Pandas simples;
- evitar métodos compactos/difíceis quando uma solução básica funciona;
- mostrar o resultado de cada etapa;
- não criar dezenas de funções utilitárias;
- não criar abstrações desnecessárias;
- não fazer tratamentos complexos sem necessidade observada;
- explicar qualquer técnica estatística usada;
- preservar os códigos CATSER oficiais;
- utilizar as divisões oficiais 11, 13, 16 e 17 e explicar o recorte dos grupos;
- apontar limitações ao invés de escondê-las.

### Antes de escrever uma nova célula de código

O ideal é responder:

1. **O que queremos descobrir?**
2. **Que colunas precisamos?**
3. **O que essa célula fará?**
4. **Como explicaríamos essa célula ao professor?**

---

# 33. Estado das decisões

## Decidido / preferência forte

- simplificar significativamente o notebook;
- entender dados antes de programar;
- trabalhar principalmente com COMPRA_ITEM + CATSER no começo;
- reduzir o número de colunas;
- organizar os 15 grupos selecionados nas divisões oficiais 11, 13, 16 e 17;
- manter os códigos e nomes oficiais de grupo e divisão na base;
- aplicar tratamento conservador;
- não tentar corrigir todas as inconsistências;
- declarar problemas complexos como limitações;
- não remover outliers automaticamente;
- usar análise exploratória para avaliar impacto dos extremos;
- considerar análise de sensibilidade;
- simplificar o K-Means;
- começar o K-Means com quantidade e valor unitário estimado;
- evitar incluir `valor_total` junto das duas variáveis se ele for essencialmente derivado delas;
- priorizar explicabilidade sobre sofisticação.

## Propostas ainda passíveis de revisão

- título final do trabalho;
- conjunto final de 10–12 colunas;
- qual scaler utilizar no K-Means;
- necessidade ou não de `log1p`;
- número ideal de clusters;
- se informações de fornecedor continuarão no escopo final;
- quais perguntas originais serão realmente respondidas.

---

# 34. Fontes do projeto usadas como referência

Arquivos existentes no projeto:

- `projeto_topicos.ipynb` — notebook atual, contendo entendimento das bases, integração, tratamento e análises já desenvolvidas;
- `Definição do Problema Tópicos.docx` — definição do problema, título e perguntas de pesquisa;
- `api-docs.json` — documentação da API do ComprasGov;
- `colab_software.zip` — arquivos/dados utilizados pelo notebook.

A documentação da API confirma, entre outras coisas, a existência de endpoints e estruturas específicas para:

- catálogo de serviços CATSER;
- contratações;
- itens das contratações;
- resultados;
- contratos;
- fornecedores;
- preços praticados.

---

# 35. Resumo em uma frase

> **O projeto será reconstruído com foco em poucos dados, poucas categorias, tratamento conservador, análise exploratória clara e um K-Means simples, para que todas as decisões possam ser compreendidas e defendidas pelo grupo na apresentação.**
