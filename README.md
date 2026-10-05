# Projeto de Tópicos — ComprasGov, CATSER e serviços de software/TIC

Este projeto acadêmico estuda contratações públicas de serviços relacionados a software e Tecnologia da Informação e Comunicação (TIC), utilizando dados do ComprasGov e a classificação do Catálogo de Serviços (CATSER). O período previsto é de 2021 a 2026, com cobertura parcial de 2026.

O trabalho está sendo reconstruído para que o grupo consiga compreender o código e explicar as decisões na apresentação. A prioridade é uma solução simples, com etapas curtas, resultados visíveis e critérios justificáveis.

## Comece pela documentação

Antes de modificar o projeto, leia os arquivos de [docs/contexto](docs/contexto/) na ordem numérica:

| Ordem | Arquivo | O que apresenta |
|---|---|---|
| 1 | [Definição do problema](docs/contexto/1-definicao-problema-topicos.docx) | Tema, período, perguntas de pesquisa e fontes dos dados. |
| 2 | [Relatório antigo](docs/contexto/2-relatorio_antigo.docx) | Metodologia e resultados registrados na versão anterior. |
| 3 | [Colab antigo](docs/contexto/3-colab_antigo.ipynb) | Código, explicações e saídas da versão anterior. |
| 4 | [Pacote de dados antigo](docs/contexto/4-colab_software.zip) | CATSER, CSVs anuais recortados e documentação do filtro. |
| 5 | [Contexto da simplificação](docs/contexto/5-contexto_projeto_topicos_simplificado.md) | Direção da reconstrução, preferências e decisões ainda abertas. |
| 6 | [Divisão de tarefas da Etapa 1](docs/contexto/6-divisao_tarefas_etapa1_projeto_topicos.md) | Responsabilidades e entregas da etapa atual. |
| 7 | [Documentação da API](docs/contexto/7-api-docs.json) | Endpoints, parâmetros e estruturas dos dados do ComprasGov. |

Os materiais antigos ajudam a entender o histórico. Para desenvolver a nova versão, considere principalmente as orientações dos arquivos 5 e 6 e as decisões posteriores da equipe.

## Materiais novos que serão editados

O arquivo [docs/links_novos.txt](docs/links_novos.txt) reúne os destinos das edições solicitadas pela equipe:

- [Relatório novo no Google Docs](https://docs.google.com/document/d/11EhDeRKvgAfZD_x90iEhZ7otCobHO_uk/edit).
- [Notebook novo no Google Colab](https://colab.research.google.com/drive/1o2u2IYXfsFYuv_dWIaIIR6XQSWLtiqR3).

Consulte esse arquivo para localizar os links. As edições nesses materiais devem seguir as solicitações da equipe.

## Antes de escrever código Pandas

**Consulte os notebooks de [docs/codigos_professor](docs/codigos_professor/) e os [slides das aulas](docs/slides/) antes de escrever ou alterar código Pandas.** Essas são as referências para manter o código próximo dos métodos, do estilo e do nível de complexidade ensinados pelo professor.

Os exemplos disponíveis são:

- [Python para Data Science — Pandas](docs/codigos_professor/CD_ML_Python_para_Data_Science_Pandas.ipynb): fundamentos de Python, estruturas do Pandas, seleção de dados e operações básicas.
- [Pré-processamento — Parte 1](docs/codigos_professor/CD_ML_Pre_Processamento_parte_1.ipynb): carga e exploração de dados, valores ausentes e transformações.

Os slides cobrem introdução à ciência de dados, fontes e coleta, tratamento e transformação dos dados.

Ao desenvolver uma célula:

1. Defina o que ela precisa descobrir ou transformar.
2. Identifique as colunas necessárias.
3. Procure uma operação equivalente nos exemplos do professor e nos slides.
4. Escreva o código de forma simples e explique o critério adotado.
5. Mostre o resultado e confira se ele corresponde ao objetivo.

Adapte os exemplos aos dados e às versões das bibliotecas utilizadas, preservando a abordagem didática. Uma técnica ensinada em aula deve ser aplicada quando houver necessidade no projeto: por exemplo, conhecer `fillna()` não significa preencher automaticamente todos os valores ausentes.

Quando uma solução exigir uma técnica adicional, explique sua necessidade e seu funcionamento. Cada integrante deve conseguir explicar sua parte e compreender o fluxo geral do trabalho.

## Direção da nova versão

A preferência inicial é trabalhar com `COMPRA_ITEM + CATSER`, selecionar poucos campos relevantes e consultar outras bases conforme as perguntas exigirem.

O recorte utiliza 15 grupos CATSER, organizados em quatro divisões oficiais:

| Código da divisão | Nome oficial da divisão | Grupos selecionados no trabalho |
|---|---|---|
| **11** | Serviços de desenvolvimento, manutenção e sustentação de software | 111, 112, 113, 114, 115, 116, 117 |
| **13** | Serviços de computação em nuvem | 131 |
| **16** | Serviços para a infraestrutura de Tecnologia da Informação e Comunicação (TIC) | 161, 162, 163, 164, 165 |
| **17** | Serviços de pesquisa, análise de dados e indicadores, consultoria e projetos de TIC | 172, 173 |

Divisão fica acima de Grupo na hierarquia oficial do CATSER. Preserve os códigos e nomes oficiais dos dois níveis na base. O recorte continua limitado aos 15 grupos selecionados. O CSV CATSER local não contém a divisão; acrescente `codigo_divisao_catser` e `nome_divisao_catser` conforme a correspondência da tabela.

O tratamento deve priorizar duplicatas exatas, tipos, ausências essenciais, valores claramente inválidos e pequenas padronizações textuais. Registre os critérios e as quantidades de registros afetados. Zeros, valores extremos e versões diferentes de um item exigem interpretação; problemas sem uma solução segura devem ser documentados como limitações.

## Escopo da Etapa 1

A etapa atual deve produzir:

- Entendimento das bases e definição do recorte.
- Seleção de colunas e organização nas quatro divisões oficiais do CATSER.
- Limpeza estrutural e validação básica dos valores.
- Estatística descritiva básica.
- Dataset tratado e dicionário de dados.
- Documentação dos tratamentos e das limitações.
- Referência, resumo e crítica de um trabalho científico relacionado.

A divisão de atividades fica assim:

| Pessoa | Integrante | Responsabilidade principal |
|---|---|---|
| Pessoa 1 (P1) | Davi | Entendimento das bases, seleção de colunas, CATSER, recorte, divisões oficiais, problema e questões de pesquisa. |
| Pessoa 2 (P2) | Mariana | Tipos das colunas, duplicatas exatas, valores ausentes e pequenas padronizações textuais. |
| Pessoa 3 (P3) | Maria Clara | Validação dos valores, estatística descritiva básica, dataset final, dicionário de dados e limitações. |

As atividades de notebook, relatório, apresentação e trabalho científico de cada pessoa estão detalhadas no [arquivo de tarefas](docs/contexto/6-divisao_tarefas_etapa1_projeto_topicos.md). K-Means, interpretação de clusters e análises aprofundadas de evolução temporal, preços e fornecedores ficam para uma etapa futura.

## Dados e cuidados de interpretação

- `dados_brutos/` armazena os dados locais e está listada no `.gitignore`. Ao baixar o projeto, confira se os arquivos necessários estão disponíveis; eles podem precisar ser obtidos separadamente nas fontes descritas na documentação.
- O ZIP antigo contém um recorte de 18 grupos e 62 códigos de serviço. O notebook antigo aplica uma seleção posterior de 15 grupos. Confira o filtro antes de reutilizar o pacote na nova versão.
- A definição inicial menciona administração federal, mas a base antiga inclui outras esferas. A nova versão precisa alinhar o recorte dos dados com o título e as perguntas adotadas.
- Diferencie o ano da contratação do ano do arquivo de origem. Considere a data de corte e a cobertura parcial de 2026 nas comparações.
- Uma linha do arquivo pode representar uma ocorrência ou versão de um item. Defina o que será contado antes de produzir estatísticas.
- Diferencie valores estimados, resultados homologados e pagamentos efetivamente realizados.
- As unidades de medida ajudam a interpretar quantidades e preços de serviços diferentes.
- A comparação antes/depois de 2022 pode descrever mudanças, mas, sozinha, não demonstra que a inteligência artificial foi sua causa.

## Ambiente local

O notebook novo está no Colab. Para trabalhar localmente com as bibliotecas registradas no repositório, execute os comandos abaixo na raiz, em um terminal Linux ou macOS com Python 3 instalado:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

O arquivo [requirements.txt](requirements.txt) registra as dependências atuais. Os comandos preparam o ambiente; a obtenção dos dados e a execução do notebook são etapas separadas. Células específicas do Colab, como acesso ao Drive e download de arquivos, precisam ser adaptadas quando executadas localmente.
