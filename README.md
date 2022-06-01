# Insiders Clustering
![image](https://user-images.githubusercontent.com/75793555/171304218-49c98584-e086-43cf-9181-0bc1bc38c638.png)

**Aviso:** O seguinte contexto é completamente fictício, a empresa, as questões de negócios existem apenas para o projeto.

Projeto de clusterização, estudo de uma base de dados de compras de clientes de um e-commerce. 

# 1. Business Problem.
All in One é um e-commerce que vende qualquer tipo de produto, após alguns anos de operação no mercado, a equipe de marketing percebeu um padrão de clientes que compram produtos mais caros, com uma frequência considerável e contribui com uma grande parcela do faturamento. Neste contexto, o time de marketing está planejando lançar um programa de fidelidade, chamado Insiders. Porém, não tem conhecimento avançado para análisar os dados classificar os clientes.

O time de marketing requisitou ao time de dados uma seleção de clientes elegíveis para o programa, usando técnicas avançadas de dados. Com posse dessa solução, a equipe de marketing poderá realizar ações direcionadas e assertivas para cada grupo do programa. 

Requisições pelo time de Marketing:
1. A indicação de pessoas para fazer parte do programa de fidelidade "INSIDERS".

Relatório com as respostas para as seguintes perguntas:
1. Quem são as pessoas elegíveis para participar do programa de Insiders ?
2. Quantos clientes farão parte do grupo?
3. Quais as principais características desses clientes ?
4. Qual a porcentagem de contribuição do faturamento, vinda do Insiders ?
5. Qual a expectativa de faturamento desse grupo para os próximos meses ?
6. Quais as condições para uma pessoa ser elegível ao Insiders ?
7. Quais as condições para uma pessoa ser removida do Insiders ?
8. Qual a garantia que o programa Insiders é melhor que o restante da base ?
9. Quais ações o time de marketing pode realizar para aumentar o faturamento?

# 2. Business Assumptions.
- Produtos com valores igual a 0, trataremos como brindes e foram retiradas do dataset
- Stock_codes que indicam valores menos que 0 ou sem descrição de produto, foram removidos, devido ao não entendimento da regra de negócio
- Registros sem customer_id serão utilizados, porém, foram criados valores aleatórias para captar o comportamento dos clientes
- Coluna de descrição não será utilizada
- Removido registros de country: European Community, Unspecified
- Removido usuário 16446, tratado com bad users
- Filtrado valores de quantidade maior que  0.04
- Splitado em 2 dataframes de dados de compra e dados de devolução

# 3. Solution Strategy
Estratégias utilizadas para este primeiro ciclo da solução:

**Step 01. Data Description:**
Entendimento dos dados e aplicação de estatística de primeira ordem, para enteder os dados e aplicar tratamentos

**Step 02. Data Filtering:**
Realizado a filtragem de variáveis primeiro, para em seguida executar a feature engineering somente com dados tratados

**Step 03. Feature Engineering:**
Criar novas features para análise e treinamento do modelo

**Step 04. Exploratory Data Analysis:**
Exploração dos dados e features criadas, análise de métricas de dispersão, coeficiente de variação de cada atributo, e estudo do espaço de dados.

**Step 05. Data Preparation:**
Aplicar métodos para rescalar os dados

**Step 06. Feature Selection:**
Seleção das features que serão utilizadas para treinamento dos modelos

**Step 07. Fine Tuning Model Clustering:**
Estudo, aplicação e análise das métricas dos modelos

**Step 08. Model Training:**
Treinamento do modelo selecionado com os melhores parâmetros

**Step 09. Cluster Analysis:**
Análise dos clusters e criação do perfil de cada clusters

**Step 10. Exploratory Data Analysis 2:**
Validação das hipóteses de negócio e resolução das perguntas de negócio

# 4. Top 3 Data Insights

**Hypothesis 01: Os clientes do cluster insiders possuem um volume de faturamento acima de 25% do total de compras**
**Verdade**, os clientes do cluster insiders possuem um GMV equivalente a 52% do total.


**Hypothesis 02: Os clientes do cluster insiders possuem um volume de compras acima de 10% do total de compras**
**Verdade**, os clientes do cluster insiders possuem um volume de compra 50% acima do total.

**Hypothesis 03: Os clientes do cluster insiders tem o número de devolução médio abaixo da média da base total de clientes**
**Falso**, o número de devolução médio do cluster insiders é maior que a média da base total, cerca de 80% maior que a média da base.

# 5. Machine Learning Model Applied
Foram aplicados 4 algoritmos de clusterização:
1. K-means
2. GMM
3. Hierarquical Clustering
4. DBScan

Para chegarmos na resolução deste primeiro ciclo, a base de dados foi submetida por vários teste nos algoritmos citados acima. Inicialmente, os modelos foram testados na base de dados com algumas features criadas, porém, análisando as métricas de WSS (Within-Cluster Sum of Square), SS (Silhouette Score), e a distribuição visual dos cluster, os resultado não foram tão satisfatórios, trazendo apenas 4 grupos de clusters. 

Nesse sentido, foi aplicado a criação de embedding, espaços de dados organizados. Utilizando técnicas como:
1. PCA
2. UMAP
3. t-SNE
4. Tree Based Embedding - embedding criado através de uma Random Forest, aplicada para predizer os dados de faturamento de cada cliente.

# 6. Machine Learning Modelo Performance
Mesmo perdendo a explicabilidade do modelo com a criação de embeddings, as métricas de clusterização melhoraram com a possibidade de clusterizar os dados em até 25 clusters. 

Resumo da Sillhouette score aplicada no treinamento dos modelos no espaço de embeddings:

**Silhouette Score by cluster**
| Models | 2        | 3        | 4        | 5        | 6        | 7        | 8        | 9        | 10       | 11       | 12       | 13       | 14       | 15       |
|--------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
| KMeans | 0.426794 | 0.506205 | 0.515082 | 0.551805 | 0.574856 | 0.582516 | 0.607634 | 0.609507 | 0.617511 | 0.646029 | 0.669617 | 0.631424 | 0.690154 | 0.686410 |
|    GMM | 0.389284 | 0.503923 | 0.499307 | 0.552071 | 0.573531 | 0.592102 | 0.548046 | 0.540257 | 0.576978 | 0.630087 | 0.652843 | 0.650725 | 0.619500 | 0.663549 |
|     HC | 0.426794 | 0.501154 | 0.487483 | 0.541446 | 0.553688 | 0.544013 | 0.579883 | 0.597353 | 0.623602 | 0.641098 | 0.669617 | 0.669732 | 0.690154 | 0.688029 |

Porém, para solução de modelos de clusterização, foi decidido a utilização do modelo GMM com de 8 clusters, pois perfil de cada cluster ficou melhor distribuído com essas configurações.

# 7. Business Results
--

# 8. Conclusions

# 9. Lessons Learned

# 10. Next Steps to Improve
