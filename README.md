# Rossmann Sales Prediction

![](https://github.com/70HNM4C13L/rossmann_store_sales/blob/main/img/rossmann.jpeg)


# 1. Contexto  
  
  Esse projeto foi baseado em um desafio do Kaggle, que visa simular um problema de negocio real.
  A Rossmann 'e uma das maiores redes de drogarias da Europa, presente em 7 paises conta com mais de 4000 filiais.
  Os gerentes de lojas da Rossmann estão atualmente responsáveis por prever as vendas diárias de suas lojas com antecedência de até seis semanas. A quantidade de vendas é afetada por uma série de fatores, incluindo promoções, concorrência, feriados escolares e estaduais, variações sazonais e localidade. Com milhares de gerentes fazendo previsões baseadas nos seus conhecimentos empricos e condicoes unicas de cada loja,a precisão dos resultados pode ser bastante variável.
 

# 2. Desafio

## 2.1. Problema
O CFO quer fazer reformas nas lojas, mas para saber quanto pode gastar quer prever as vendas das proximas semanas.

## 2.2. Motivacoes

* O processo de previsao atual e imprecisso 

* O processo de previsão atual não é padronizado e baseado em dados

* Atualmente a previsão de vendas é feita manualmente pelas 1.115 lojas, o que a torna lenta e custosa

* A visualizao da previsao de vendas so esta disponivel no computador

## 2.3. Premissas do negocio
* Os dados contêm informações históricas de 1115 lojas;
* Os dados disponiveis estao compreendidos entre 01/01/2013 e 31/07/2015;
* Valores nulos de distância de competidor foram substituídos por 200.000 metros, assumindo que nao ha competidor proximo.
* Nao foram considerados os dias que as lojas estiveram fechadas.
<details>
<summary>Features definition</summary>
  
 **Atributos**        |  **Descrição**  |
| ------------------- | ------------------- |
|  id | um Id que representa um (Store, Date) concatenado dentro do conjunto de teste |
|  Store |  um id único para cada loja |
|  Sales |  o volume de vendas em um determinado dia |
|  Customers |  o número de clientes em um determinado dia |
|  Open |  um indicador para saber se a loja estava aberta: 0 = fechada, 1 = aberta |
|  StateHoliday |  indica um feriado estadual. Normalmente todas as lojas, com poucas exceções, fecham nos feriados estaduais. Observe que todas as escolas fecham nos feriados e finais de semana. a = feriado, b = feriado da Páscoa, c = Natal, 0 = Nenhum |
| SchoolHoliday |  indica se (Store, Date) foi afetada pelo fechamento de escolas públicas |
|  StoreType |  diferencia entre 4 modelos de loja diferentes: a, b, c, d |
|  Assortment |  descreve um nível de sortimento: a = básico, b = extra, c = estendido |
|  CompetitionDistance |  distância em metros até a loja concorrente mais próxima |
|  CompetitionOpenSince[Month/Year] |  apresenta o ano e mês aproximados em que o concorrente mais próximo foi aberto |
|  Promo |  indica se uma loja está fazendo uma promoção naquele dia |
|  Promo2 |  Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = a loja não está participando, 1 = a loja está participando |
|  Promo2Since[Year/Week] |  descreve o ano e a semana em que a loja começou a participar da Promo2 |
|  PromoInterval          | descreve os intervalos consecutivos de início da promoção 2, nomeando os meses em que a promoção é iniciada novamente. Por exemplo. "Fev, maio, agosto, novembro" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para aquela loja |
  
 </details>
 
 

## 2.4. Solucao

* Usar um modelo de machine learning para prever as vendas de cada loja

* Disponibilizar a previsao de vendas no telegran, atraves de uma API

# 3. Etaspas da solucao

## 3.1. Data Description 

Vizualizar a tabela, cheacar e tratar celulas vazias e valores nulos 

## 3.2. Feature Engineering

Criacao de novos atributos

<details>
<summary>Novos Atributos </summary>

| Atibuto           | Definicao                                          |
|-------------------|----------------------------------------------------|
| year              | Ano, obtido a partir data                          |
| month             | Mes, obtido a partir data                          |
| day               | Dia, obtido a partir data                          |
| week of year      | Semana do ano, obtido a partir data                |
| year week         | Ano e semana, obtido a partir data (Ano-Semana)    |
| promo since       | Data desde que a promocao comecou (Ano-Semana)     |
| competition since | Data desde que concorrente abriu  (Ano, Mes, Ano)  |

</details>



## 3.3. Analise de dados exploratoria

Buscar insights atraves da visualizao de graficos, fazer correlacoes entre os atributos e testar as hipoteses.


## 3.4. Preparacao dos dados

Processo de normalizacao,rescaling e

Using normalization, rescaling e and encoding to prepare the data to the Machine Learning model. Sine and cossine transformations were used in cyclical features as month, day and week of year.

## 3.5. Selecao de atributos

Foi usado o Boruta, um algoritimo que ajuda a escolher os atributos mais relevates para o modelo


## 3.6. Modelo de machine learning

Foram treinados diferentes modelos de machine learning e feita a comparacao de erros entre eles. O modelo escolhido foi o XGBoost, que apresentou melhor acuracia e velocidade.


![Performance sem ajuste fino](https://github.com/70HNM4C13L/rossmann_store_sales/blob/main/img/mlcompare.png)

## 3.7. Traducao do erro

Trazendo pra linguagem de negocios, foi mensurado em valores a acuracia do modelo


## 3.8. Deploy e bot do Telegran

Foi feito o deply no Render e usado o Flask na API de requisicao ao telegram<br>  
O usuario informa qual o numero da loja que deseja ver a previsao de vendas das proximas 6 semanas
 


<img src="https://user-images.githubusercontent.com/77629603/162584257-c7783ef3-d434-4910-9878-c2bfb4057228.png" alt="" style="width:300px;"/>


# 4. Results and Conclusion

## 4.1. Main Hypotesis 

The main hypotesis confirmed in the EDA step:

### H1. Stores with larger assortments should sell more.
**False** <br />Stores with larger assortments should sell less.

![Sales sum by assortment](https://user-images.githubusercontent.com/77629603/155387884-6c33a7be-82e5-4c57-8648-28bf0f217aae.png)


### H2. Stores with closer competitors should sell less.
**False**  <br />Stores with closer competitors sell more.

![Sales by competition distance (bin = 0-1000)](https://user-images.githubusercontent.com/77629603/155381618-a59fdbc2-e4af-45dd-8458-3159ddc01eac.png)


### H3. Stores with longer active promotions should sell more.
**False** <br />We can see that sales increase in the standard promos and decreases in the extended promos.
(Negative promo duration is regular promo, positive promo duration is extanded promo)

![Regplot representing sales by promo duration](https://user-images.githubusercontent.com/77629603/155382386-6c6462ab-0820-4dae-a1ca-51ea9a0aad33.png)

Note: Sales = Revenue

## 4.2.Conclusion
The model generates a dataframe with the prediction of each store and the respectives worst and best scenarios. 
The CFO now can know the budget available to renovate the stores, with 90% accuracy.

![First 15 rows of the prediction dataset.](https://user-images.githubusercontent.com/77629603/155379600-1321b4d9-6db2-4941-80cf-96012798fe00.png)

The user can get the results by Telegram. Here is some [demonstration](https://www.linkedin.com/posts/heitor-felix_datascience-datadriven-business-activity-6902361790051606528-2Fjo)!

## 4.3. Machine Learning Performance

**Final model performance**
![image](https://user-images.githubusercontent.com/77629603/162584149-291cea37-819d-4f18-bd67-0aac45349557.png)

Here is the demonstration of the model prediction vs real sales by date

![Seaborn lineplot](https://user-images.githubusercontent.com/77629603/155380531-060fbf29-4f30-486f-b875-4d3b0ead5178.png)


# 5. Next Steps

* Collecting feedback of the users and improve the usability if necessary
* Improve the performance in the next CRISP cycle

# 6. References
* [Kaggle](https://www.kaggle.com/c/rossmann-store-sales)
* [Comunidade DS](https://www.comunidadedatascience.com/)
