# Análise de Vendas e Rentabilidade no Varejo

## 1. Visão geral do projeto

Este projeto tem como objetivo analisar dados de vendas de uma operação de varejo para compreender seu desempenho comercial, identificar fatores associados à rentabilidade e gerar informações que possam auxiliar na tomada de decisões de negócio.

O projeto foi desenvolvido como um estudo prático de análise de dados, integrando Python, MySQL, SQL e Power BI em um fluxo de trabalho que vai desde a exploração e preparação dos dados até a criação de um dashboard interativo.

A proposta não foi apenas construir visualizações, mas desenvolver um processo analítico completo: compreender a base de dados, verificar sua qualidade, investigar questões de negócio, validar os resultados e transformar os dados em informações relevantes.

## 2. Objetivos

- Explorar e compreender uma base de dados de vendas.
- Verificar a qualidade e a consistência dos dados.
- Preparar os dados para análise e armazenamento em banco de dados.
- Utilizar SQL para responder a questões de negócio.
- Investigar vendas, lucro, margem de lucro, produtos, clientes, regiões e descontos.
- Desenvolver um dashboard interativo no Power BI.
- Comparar resultados obtidos em diferentes ferramentas para identificar possíveis inconsistências.
- Documentar o processo, as dificuldades encontradas e os principais aprendizados.

## 3. Ferramentas e tecnologias

| Ferramenta | Aplicação no projeto |
|---|---|
| Python | Exploração, validação e preparação inicial dos dados |
| Google Colab | Ambiente de desenvolvimento para executar o código Python |
| MySQL | Armazenamento dos dados e execução das consultas analíticas |
| SQL | Investigação das questões de negócio, agregações e classificações |
| Power BI Desktop | Criação do dashboard interativo |
| DAX | Criação de medidas e indicadores no Power BI |
| GitHub | Versionamento, organização e documentação do projeto |

## 4. Base de dados

O projeto utiliza uma base de dados de vendas no varejo, no formato Superstore, com informações sobre pedidos, clientes, produtos, entregas, vendas, descontos e lucros.

A base possui **9.994 registros** e contém campos como:

- Identificação e data dos pedidos;
- Data e modalidade de envio;
- Identificação e nome dos clientes;
- Segmento de clientes;
- País, estado, cidade e região;
- Identificação, nome, categoria e subcategoria dos produtos;
- Valor de vendas;
- Quantidade vendida;
- Desconto aplicado;
- Lucro obtido.

Durante a preparação, também foram criados campos derivados para ampliar as possibilidades de análise, como tempo de envio, ano do pedido, mês do pedido, nome do mês e margem de lucro.

### Verificações iniciais

A primeira etapa foi compreender a estrutura e a qualidade da base antes de iniciar as análises.

Foram realizadas verificações de:

- Quantidade de linhas e colunas;
- Existência de valores ausentes;
- Existência de registros duplicados;
- Quantidade de pedidos, clientes e produtos distintos;
- Tipos de dados das colunas, especialmente datas e valores numéricos.

Nas verificações iniciais, não foram identificados valores ausentes ou registros duplicados. Também foi observado que as colunas de data estavam inicialmente armazenadas como texto, o que exigiu atenção para permitir análises temporais adequadas.

## 5. Etapas de desenvolvimento

### 5.1. Exploração e preparação dos dados com Python

A primeira etapa foi realizada no Google Colab, utilizando Python para explorar a base e entender suas características.

Antes de partir para as consultas SQL e as visualizações, foi necessário verificar se os dados estavam estruturados corretamente e se poderiam ser utilizados nas análises propostas.

Além das verificações de qualidade, foram criadas colunas derivadas para facilitar as investigações:

- `shipping_days`: quantidade de dias entre o pedido e o envio;
- `order_year`: ano do pedido;
- `order_month`: número do mês do pedido;
- `order_month_name`: nome do mês do pedido;
- `profit_margin`: margem de lucro calculada a partir do lucro e das vendas.

Essas informações permitiram analisar o tempo de envio, a evolução das vendas e as diferenças de rentabilidade entre produtos e períodos.

### 5.2. Armazenamento e análise com MySQL

Após a preparação inicial, os dados foram importados para o MySQL, no banco de dados `retail_sales`.

O banco passou a ser o ambiente principal para a investigação das questões de negócio por meio de consultas SQL.

Foram utilizados recursos como:

- `SELECT` e `WHERE` para selecionar e filtrar registros;
- `GROUP BY` para agrupar dados por região, categoria, subcategoria, cliente e desconto;
- `SUM`, `AVG` e `COUNT` para calcular indicadores;
- `COUNT(DISTINCT ...)` para contar pedidos e clientes distintos;
- `ORDER BY` e `LIMIT` para ordenar e classificar resultados;
- Expressões calculadas para avaliar lucro e margem de lucro.

O objetivo foi utilizar os dados para responder a perguntas concretas sobre o desempenho da operação, em vez de realizar consultas sem uma finalidade analítica definida.

### 5.3. Definição das questões de negócio

As consultas SQL foram organizadas em torno de questões que poderiam ser relevantes para uma empresa de varejo.

Entre os temas investigados estão:

1. Quais produtos apresentam maior volume de vendas ou maior faturamento?
2. Quais subcategorias são mais lucrativas?
3. Como os descontos se relacionam com a rentabilidade?
4. Qual é a presença de descontos elevados nas vendas?
5. Quais categorias e subcategorias apresentam melhor desempenho comercial?
6. Quais regiões contribuem mais para o lucro?
7. Como as vendas e a rentabilidade variam ao longo do tempo?
8. Quais produtos ou subcategorias apresentam margens negativas?
9. Como se comportam os pedidos e as compras dos clientes?
10. Quais clientes se destacam em frequência de pedidos, faturamento ou lucro?

Cada questão foi transformada em uma consulta SQL e interpretada considerando o indicador utilizado e o contexto de negócio.

Um cuidado importante foi diferenciar métricas que podem parecer semelhantes, mas respondem a perguntas distintas. Por exemplo, quantidade de unidades vendidas, número de pedidos distintos que contêm um produto e faturamento total não são medidas equivalentes.

A escolha da métrica depende da pergunta que se deseja responder.

### 5.4. Validação dos dados e resolução de problemas

Uma das etapas mais importantes do projeto foi validar os resultados e corrigir inconsistências identificadas durante a integração entre as ferramentas.

Durante uma das tentativas de importação para o MySQL, valores decimais foram interpretados incorretamente. Um valor como `907.152`, por exemplo, chegou a ser importado como `907152`, alterando significativamente os totais de vendas e lucro.

O problema estava relacionado à interpretação dos separadores numéricos durante a importação.

Para resolvê-lo, foi necessário revisar a configuração de importação, corrigir a interpretação dos valores e realizar uma nova carga dos dados. Em seguida, foram conferidas a quantidade de registros e as métricas gerais da base.

Os valores de referência utilizados na validação foram:

| Indicador | Resultado esperado |
|---|---:|
| Total de registros | 9.994 |
| Pedidos distintos | 5.009 |
| Faturamento total | 2.297.201,07 |
| Lucro total | 286.397,79 |
| Margem de lucro geral | Aproximadamente 12,47% |

Esses valores foram utilizados como referência para verificar se os dados estavam sendo interpretados corretamente.

A experiência reforçou um princípio fundamental da análise de dados: **uma consulta pode ser executada sem erros e, ainda assim, produzir resultados incorretos quando os dados de origem estão inconsistentes.**

Por isso, a validação foi tratada como parte essencial do processo, e não apenas como uma verificação final.

### 5.5. Desenvolvimento do dashboard no Power BI

Após estruturar as consultas e os indicadores, os dados foram importados para o Power BI Desktop para a construção de um dashboard interativo.

Foram criadas medidas em DAX para calcular os principais indicadores de desempenho:

```dax
Faturamento Total =
SUM(retail_sales_mysql[sales])

Lucro Total =
SUM(retail_sales_mysql[profit])

Margem de Lucro =
DIVIDE([Lucro Total], [Faturamento Total], 0)

Total de Pedidos =
DISTINCTCOUNT(retail_sales_mysql[order_id])
```

As medidas permitem que os indicadores sejam recalculados conforme os filtros e as dimensões selecionadas nos visuais.

O dashboard foi dividido em três páginas, cada uma com um foco analítico específico.

#### Página 1 — Visão geral de vendas e rentabilidade

Apresenta os principais indicadores da operação, incluindo faturamento, lucro, margem de lucro e quantidade de pedidos.

Também reúne visualizações de vendas ao longo do tempo, lucro por região, lucro por categoria, margem por subcategoria e análise de descontos.

O objetivo é oferecer uma visão geral do desempenho e permitir a identificação de áreas que merecem uma investigação mais detalhada.

#### Página 2 — Produtos e subcategorias

Concentra a análise do desempenho dos produtos, permitindo comparar indicadores de vendas, frequência de pedidos e rentabilidade.

Essa página ajuda a diferenciar produtos com grande presença comercial daqueles que efetivamente apresentam melhores resultados financeiros.

#### Página 3 — Clientes e pedidos

Explora o comportamento dos clientes e a distribuição dos pedidos ao longo do tempo.

Inclui análises de frequência de pedidos e classificações de clientes, permitindo investigar diferenças entre comportamento de compra, faturamento e contribuição para o lucro.

Filtros interativos, como região, ano e categoria, permitem explorar diferentes recortes dos dados.

### 5.6. Ajustes técnicos e refinamento do dashboard

A construção do dashboard também exigiu ajustes para garantir a confiabilidade dos indicadores e melhorar a leitura das informações.

**Correção dos valores numéricos**

Assim como ocorreu na importação para o MySQL, foi necessário corrigir a interpretação dos valores numéricos no Power BI. A conferência dos indicadores gerais ajudou a identificar que os valores iniciais não correspondiam aos resultados esperados.

**Configuração das datas**

Inicialmente, a visualização temporal apresentava dificuldades relacionadas à hierarquia de datas e à ordenação. Foi necessário atualizar a hierarquia e ajustar a classificação para que os meses fossem apresentados em ordem cronológica, e não ordenados pelo valor de vendas.

**Escolha adequada das métricas**

Também foi necessário distinguir o número de pedidos que contêm determinado produto da quantidade de unidades vendidas. Um produto pode aparecer em muitos pedidos, mas isso não significa necessariamente que tenha a maior quantidade total vendida.

A mesma atenção foi aplicada às classificações de clientes, separando frequência de pedidos, faturamento e lucro.

**Organização visual**

Por fim, o layout foi refinado para melhorar a legibilidade e a consistência entre as páginas. Foram organizados os indicadores, os filtros, os títulos e os gráficos, buscando reduzir a poluição visual e facilitar a interpretação dos resultados.

## 6. Principais análises e insights

As consultas SQL e as visualizações do Power BI permitiram identificar padrões que merecem atenção do ponto de vista comercial e financeiro.

### Desempenho regional

A comparação entre regiões mostrou diferenças de faturamento, lucro e margem de lucro.

Analisar essas três métricas em conjunto é mais informativo do que classificar as regiões apenas pelo faturamento, pois uma região com muitas vendas não necessariamente apresenta a melhor rentabilidade proporcional.

### Rentabilidade de produtos e subcategorias

A rentabilidade variou entre as subcategorias. Algumas subcategorias de Furniture, incluindo Tables e Bookcases, apresentaram resultados negativos de lucro nas análises realizadas.

Esse comportamento indica que o desempenho comercial deve ser avaliado em conjunto com a rentabilidade. Vender mais não significa necessariamente gerar mais lucro.

### Relação entre descontos e lucro

As análises identificaram situações em que descontos elevados estavam associados a margens negativas, além de diferenças de rentabilidade entre subcategorias e níveis de desconto.

Esse resultado reforça a importância de monitorar os descontos e avaliar seu impacto financeiro.

Entretanto, a associação entre descontos e menor rentabilidade não comprova, isoladamente, que os descontos sejam a causa dos prejuízos. Custos, composição dos produtos vendidos e outros fatores também podem influenciar o resultado.

### Comportamento dos clientes

As classificações de clientes variam de acordo com a métrica utilizada.

Um cliente com muitos pedidos não é necessariamente aquele que gera o maior faturamento ou lucro. Por isso, analisar frequência, vendas e rentabilidade separadamente permite uma compreensão mais completa do comportamento dos clientes.

### Evolução temporal

As análises por mês e ano permitem observar mudanças no faturamento e no lucro ao longo do tempo.

Para afirmar que existe sazonalidade, entretanto, é necessário comparar períodos equivalentes em diferentes anos. Uma concentração de vendas em determinado mês, isoladamente, não comprova a existência de um padrão sazonal recorrente.

Os resultados deste projeto devem ser entendidos como análises descritivas e pontos de partida para investigações adicionais, não como provas definitivas de causalidade.

## 7. Fluxo dos dados e arquitetura

O fluxo de trabalho desenvolvido pode ser resumido da seguinte forma:

```text
Base de dados original
        |
        v
Python / Google Colab
Exploração e preparação
        |
        v
MySQL
Armazenamento estruturado
        |
        v
Consultas SQL
Questões de negócio e indicadores
        |
        v
Power BI Desktop
Medidas DAX e visualizações
        |
        v
Dashboard e insights de negócio
```

Cada ferramenta desempenhou uma função específica no processo.

O Python foi utilizado para compreender e preparar os dados; o MySQL forneceu uma estrutura para armazená-los e consultá-los; o SQL foi utilizado para investigar as questões de negócio; e o Power BI transformou os resultados em visualizações interativas.

A comparação dos resultados entre as ferramentas também foi importante para identificar inconsistências e aumentar a confiança nas análises.

## 8. Estrutura

O repositório foi organizado para separar as consultas, o dashboard e a documentação complementar.

```text
retail-sales-profitability-analysis/
│
├── README.md
│
├── Python/
│   └── Retail_Sales_&_Profitability_Analysis.ipynb
│
├── powerbi/
│   ├── retail_sales_dashboard.pbix
│   └── dashboard.pdf
│
├── docs/
    ├── SQL_Business_Questions_Retail_Sales.pdf
    └── Relatorio_Insights_Retail_Sales.pdf
```

- `sql/`: contém as consultas SQL utilizadas nas análises.
- `powerbi/`: contém o arquivo do dashboard e sua versão em PDF.
- `docs/`: reúne os documentos com as questões de negócio e os principais insights.

A base original não precisa ser incluída no repositório caso existam restrições de redistribuição. Nesse caso, o arquivo `data/README.md` deve informar a fonte dos dados e as instruções necessárias para reproduzir a análise.

## 9. Como explorar o projeto

Para conhecer o projeto, recomenda-se seguir esta ordem:

1. Consultar o arquivo `business_questions.sql` para entender as perguntas investigadas e as consultas utilizadas.
2. Ler os relatórios na pasta `docs/` para conhecer os resultados e as interpretações.
3. Abrir o PDF do dashboard para visualizar rapidamente os principais gráficos.
4. Abrir o arquivo `.pbix` no Power BI Desktop para explorar as páginas e os filtros interativos.

Para reproduzir as análises, será necessário obter a base de dados, seguir as etapas de preparação, carregá-la no MySQL e adaptar as consultas à estrutura do banco utilizado.

O dashboard também pode exigir a configuração da conexão com a fonte de dados antes de permitir a atualização dos resultados.

## 10. Competências desenvolvidas

O desenvolvimento deste projeto permitiu praticar e integrar competências relacionadas a:

- Análise exploratória de dados;
- Verificação de qualidade e consistência dos dados;
- Preparação de dados com Python;
- Modelagem e armazenamento em banco de dados relacional;
- Consultas SQL e agregações analíticas;
- Criação de indicadores e medidas DAX;
- Construção de dashboards interativos;
- Análise temporal;
- Comparação e validação de resultados entre ferramentas;
- Resolução de problemas de importação e interpretação numérica;
- Interpretação de resultados no contexto de negócio;
- Organização e documentação de projetos no GitHub.

## 11. Conclusão

O projeto de Análise de Vendas e Rentabilidade no Varejo demonstra um fluxo de trabalho de análise de dados que parte da exploração de uma base original e chega à construção de consultas analíticas, indicadores e visualizações interativas.

Um dos principais aprendizados foi compreender a importância da qualidade dos dados e da validação dos resultados antes de tirar conclusões. Problemas na interpretação de valores numéricos, escolhas inadequadas de métricas e agregações mal interpretadas podem comprometer uma análise, mesmo quando as consultas e os gráficos parecem funcionar corretamente.

A integração entre Python, MySQL, SQL e Power BI permitiu praticar diferentes etapas do trabalho de um analista de dados: preparar informações, investigar questões relevantes, validar resultados e comunicar descobertas de maneira estruturada.

Mais do que apresentar números, o projeto busca demonstrar como os dados podem ser utilizados para identificar padrões, formular hipóteses e apoiar decisões de negócio de forma mais fundamentada.
