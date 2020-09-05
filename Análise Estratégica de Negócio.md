# Análise Estratégica de Negócio

<p style='text-align: justify;'> Uma pequena Startup que presta serviços de lavanderia deseja construir uma grande rede ao atuar em diversas cidades.  A estratégia da Startup é atuar em cidades pequenas pois ela ainda não consegue competir com grandes empresas já estabelecidas nos grandes centros urbanos. O público-alvo da empresa são clientes que residem em apartamentos pequenos que não possuem espaço para uma máquina de lavar. O funcionamento do negócio consiste na ida de um funcionário até o cliente, onde são coletadas as peças de roupas, após isso as mesmas são lavadas, secadas, passadas e então são devolvidas ao cliente. A empresa também fornece um serviço especial para clientes que estão com mais pressa. 

<img src= "foto1.jpg">   
  
Atualmente a Startup já possui estabelecimentos em 140 diferentes localidades dos Estados Unidos e recentemente abriu 10 novas lojas. Essas 150 lojas estão dividas em duas regiões de vendas, visando estimular a competição e a busca por melhores resultados dentro da empresa
    
Sendo assim é necessário verificar qual das duas regiões está com uma melhor perfomance de desempenho. Pra isso a direção da Startup definiu que a melhor região deveria superar a outra em pelo menos doiss dos três indicadores de desempenho abaixo:
</p>

* **Receita Média (RM)**: Somatório de todas as receitas de uma região divido pela quantidade de lojas presentes naquela região
* **Gasto de Marketing Médio (GMM)**: Total de invesimento em Marketing de uma região divido pela quantidade de lojas pertencentes aquela região
* **Retorno Sobre Investimento (ROI)**: Total de receita de uma região dividio pelo total de investimento em marketing da região

Além disso a empresa também deseja que sejam identificadas quais das 10 novas localizações possuem o melhor potencial de investimento em Marketing




## 1) Os dados

Descrição dos dados fornecidos pela Startup:

**Store ID:** número de identificação da loja

**City:** cidade onde se localiza a loja

**State:** estado onde se localiza a loja

**Sales Region:** região de vendas na qual a loja faz parte

**New Expansion:** se a loja faz parte das 10 novas unidades ou se faz parte das 140 antigas 

**Marketing Spend:** gasto com propaganda

**Revenue:** receita gerada pela loja


```python
# importando principais bibliotecas
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams['figure.figsize'] = (8, 5)


# importanto os dados
data1 = pd.read_excel("dataset1.xlsx")
data1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store ID</th>
      <th>City</th>
      <th>State</th>
      <th>Sales Region</th>
      <th>New Expansion</th>
      <th>Marketing Spend</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Peoria</td>
      <td>Arizona</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2601</td>
      <td>48610</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Midland</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2727</td>
      <td>45689</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Spokane</td>
      <td>Washington</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2768</td>
      <td>49554</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Denton</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2759</td>
      <td>38284</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Overland Park</td>
      <td>Kansas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2869</td>
      <td>59887</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>146</td>
      <td>Paterson</td>
      <td>New Jersey</td>
      <td>Region 1</td>
      <td>New</td>
      <td>2251</td>
      <td>34603</td>
    </tr>
    <tr>
      <th>146</th>
      <td>147</td>
      <td>Brownsville</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>New</td>
      <td>3675</td>
      <td>63148</td>
    </tr>
    <tr>
      <th>147</th>
      <td>148</td>
      <td>Rockford</td>
      <td>Illinois</td>
      <td>Region 1</td>
      <td>New</td>
      <td>2648</td>
      <td>43377</td>
    </tr>
    <tr>
      <th>148</th>
      <td>149</td>
      <td>College Station</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>New</td>
      <td>2994</td>
      <td>22457</td>
    </tr>
    <tr>
      <th>149</th>
      <td>150</td>
      <td>Thousand Oaks</td>
      <td>California</td>
      <td>Region 2</td>
      <td>New</td>
      <td>2431</td>
      <td>40141</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 7 columns</p>
</div>



## 2) Análise de Desempenho das Regiões de Vendas 

Primeiramente vamos filtrar os dados por região e encontrar a quantidade de lojas em cada região


```python
# filtrando por região
r1 = data1[data1["Sales Region"] == "Region 1"]
r2 = data1[data1["Sales Region"] == "Region 2"]

# exibindo número de lojas por região
print("Região 1 possui " + str(len(r1)) + " lojas")
print("Região 2 possui " + str(len(r2)) + " lojas")
```

    Região 1 possui 64 lojas
    Região 2 possui 86 lojas
    

### 2.1) Receita Média (RM)


```python
# Receita total de cada região
RT1 = r1["Revenue"].sum()
RT2 = r2["Revenue"].sum()

# Receita média de cada região
RM1 = round(RT1/len(r1), 1)
RM2 = round(RT2/len(r2), 1)

# Exibindo resultados
print("Receita Média da Região 1: $ " + str(RM1))
print("Receita Média da Região 2: $ " + str(RM2))
```

    Receita Média da Região 1: $ 40567.2
    Receita Média da Região 2: $ 38359.5
    

### 2.2) Gasto de Marketing Médio (GMM)


```python
# Gasto total com marketing de cada região
MT1 = r1["Marketing Spend"].sum()
MT2 = r2["Marketing Spend"].sum()

# Gasto com marketing médio de cada região
MM1 = round(MT1/len(r1), 1)
MM2 = round(MT2/len(r2), 1)

# Exibindo resultados
print("Gasto com Marketing Médio da Região 1: $ " + str(MM1))
print("Gasto com Marketing Médio  da Região 2: $ " + str(MM2))
```

    Gasto com Marketing Médio da Região 1: $ 2889.0
    Gasto com Marketing Médio  da Região 2: $ 2896.2
    

### 2.3) Retorno Sobre Investimento (ROI) 


```python
# ROI de cada região
ROI1 = round(RT1/MT1, 1)
ROI2 = round(RT2/MT2, 1)

# Exibindo resultados
print("Retorno Sobre Investimento da Região 1: $ " + str(ROI1))
print("Retorno Sobre Investimento  da Região 2: $ " + str(ROI2))
```

    Retorno Sobre Investimento da Região 1: $ 14.0
    Retorno Sobre Investimento  da Região 2: $ 13.2
    

### 2.4) Resultado


```python
# Criando resumo das métricas de desempenho
dic = {'Região': [1, 2], 'RM': [RM1, RM2], 'GMM': [MM1, MM2], 'ROI': [ROI1, ROI2]}
df = pd.DataFrame(data=dic)
print(df.to_string(index=False))
```

     Região       RM     GMM   ROI
          1  40567.2  2889.0  14.0
          2  38359.5  2896.2  13.2
    

Olhando os dados da tabela acima podemos perceber que em relação a(ao):
* Receita Média: a Região 2 teve um melhor desempenho (quanto maior o valor melhor)
* Gasto Médio com Marketing: a Região 1 teve um melhor desempenho (quanto menor o valor melhor)
* Retorno Sobre Investimento: a Região 1 teve um melhor desempenho (quanto maior o valor melhor)

Dessa forma identificamos que a **Região 1 foi a região com melhor performance** já que obteve melhores resultados em 2 dos 3 indicadores. 

## 3) Análise do Potencial de Investimento em Marketing


Nesta seção faremos uma analise dos dados buscando identificar quais das 10 novas lojas possuem o melhor potencial de investimento em Marketing

### 3.1) Análise de Clusters

Vamos construir um gráfico de dispersão das 150 lojas onde podemos visualizar o quanto cada loja possui de investimento em marketing e a receita gerada


```python
plt.scatter(data1["Marketing Spend"], data1["Revenue"], s=50, alpha=0.5)
plt.xlabel('INVESTIMENTO EM MARKETING')
plt.ylabel('RECEITA')
plt.show()
```


![png](output_17_0.png)


<p style='text-align: justify;'> 

Ao olharmos para o gráfico temos a impressão que existem dois grandes grupos (clusters) um em baixo com um formato mais horizontal e outro mais acima inclinado.

O grupo de lojas que está na parte inferior do gráfico, parece ser um grupo no qual não importa qual seja o tamanho do investimento em marketing a receita não aumenta. Uma hipótese para esse fenômeno pode ser o fato do tamanho da população da cidade estar limitando o número máximo de clientes. Sendo assim, vamos importar um novo conjunto de dados com as informações do número de habitantes de cada cidade e adicionar essa informação em uma nova coluna chamada "Population" no nosso conjunto original de dados. 
</p>




```python
# importando novo conjunto de dados
data2 = pd.read_excel("dataset2.xlsx")
data2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2015 rank</th>
      <th>City</th>
      <th>State</th>
      <th>2015 estimate</th>
      <th>2010 Census</th>
      <th>Change</th>
      <th>2014 land area</th>
      <th>2010 population density</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>New York</td>
      <td>New York</td>
      <td>8550405</td>
      <td>8175133</td>
      <td>0.0459</td>
      <td>302.6 sq mi</td>
      <td>27,012 per sq mi</td>
      <td>40.6643°N 73.9385°W</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Los Angeles</td>
      <td>California</td>
      <td>3971883</td>
      <td>3792621</td>
      <td>0.0473</td>
      <td>468.7 sq mi</td>
      <td>8,092 per sq mi</td>
      <td>34.0194°N 118.4108°W</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Chicago</td>
      <td>Illinois</td>
      <td>2720546</td>
      <td>2695598</td>
      <td>0.0093</td>
      <td>227.6 sq mi</td>
      <td>11,842 per sq mi</td>
      <td>41.8376°N 87.6818°W</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Houston</td>
      <td>Texas</td>
      <td>2296224</td>
      <td>2100263</td>
      <td>0.0933</td>
      <td>599.6 sq mi</td>
      <td>3,501 per sq mi</td>
      <td>29.7805°N 95.3863°W</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Philadelphia</td>
      <td>Pennsylvania</td>
      <td>1567442</td>
      <td>1526006</td>
      <td>0.0272</td>
      <td>134.1 sq mi</td>
      <td>11,379 per sq mi</td>
      <td>40.0094°N 75.1333°W</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Retirando as colunas desnecessárias
data2 = data2[["City" , "State", "2015 estimate"]]
data2 = data2.rename({"2015 estimate": "Population"}, axis = 1)
data2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>State</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>New York</td>
      <td>New York</td>
      <td>8550405</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Los Angeles</td>
      <td>California</td>
      <td>3971883</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chicago</td>
      <td>Illinois</td>
      <td>2720546</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Houston</td>
      <td>Texas</td>
      <td>2296224</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Philadelphia</td>
      <td>Pennsylvania</td>
      <td>1567442</td>
    </tr>
  </tbody>
</table>
</div>




```python
# combinando os dataframes (left join)
data3 = pd.merge(data1, data2,  how='left', left_on=['City','State'], right_on = ['City','State'])
data3.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store ID</th>
      <th>City</th>
      <th>State</th>
      <th>Sales Region</th>
      <th>New Expansion</th>
      <th>Marketing Spend</th>
      <th>Revenue</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Peoria</td>
      <td>Arizona</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2601</td>
      <td>48610</td>
      <td>171237</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Midland</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2727</td>
      <td>45689</td>
      <td>132950</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Spokane</td>
      <td>Washington</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2768</td>
      <td>49554</td>
      <td>213272</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Denton</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2759</td>
      <td>38284</td>
      <td>131044</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Overland Park</td>
      <td>Kansas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2869</td>
      <td>59887</td>
      <td>186515</td>
    </tr>
  </tbody>
</table>
</div>



Agora vamos tentar visualizar como ficaria nosso gráfico de disperção com 2 clusters calculados com o algorítmo K-Means, onde são considerados três variáveis: Receita, Investimento em Marketing e o Tamanho da População.


```python
# importando bibliotecas
from pandas import DataFrame
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans

# criando os clusters  
kmeans = KMeans(n_clusters=2 , init='k-means++').fit(data3[["Marketing Spend" , "Revenue" , "Population"]])

# construindo o gráfico
plt.scatter(data3["Marketing Spend"], data3["Revenue"], c= kmeans.labels_.astype(float), s=50, alpha=0.5)
plt.xlabel('INVESTIMENTO EM MARKETING')
plt.ylabel('RECEITA')
plt.show()
```


![png](output_23_0.png)


O gráfico não ficou do jeito que imaginamos inicialmente. Como cada cor indica que uma determinada loja pertence a um cluster,  era para o grupo de pontos na parte inferior ficar com coloração roxa e a outro grupo ficar com coloração amarela. Porém onde esperávamos só a cor amarela também teve uma parte roxa. Sendo assim o número de clusters ideal para esse problema não é 2. Precisamos descobrir qual o número ótimo de cluster e para isso usaremos o "Elbow Method".


```python
# Aplicando diversos algoritmos de K-means e guardando a inertia
Inert = []
for cluster in range(1,11):
    kmeans = KMeans(n_clusters = cluster, init='k-means++')
    kmeans.fit(data3[["Marketing Spend" , "Revenue" , "Population"]])
    Inert.append(kmeans.inertia_)

# Plotando os resultados
frame = pd.DataFrame({'Cluster':range(1,11), 'Inertia':Inert})
plt.plot(frame['Cluster'], frame['Inertia'], marker='o')

# reta do melhor k
BestK = [3,3,3,3,3,3,3,3,3, 3]
plt.plot(BestK , frame['Inertia'], 'k--', color = 'red' , label = "Nº Ótimo de Clusters" )

# configurações do gráfico
plt.xlabel('Número de Clusters')
plt.ylabel('Inertia')
plt.title("The Elbow Method")
plt.legend(loc = 'best')
plt.show()
```


![png](output_25_0.png)


Agora sabemos que o número ótimo de clusters é 3, podemos refazer o processo de clusterização


```python
# criando os clusters  
kmeans = KMeans(n_clusters = 3, init='k-means++')
kmeans.fit(data3[["Marketing Spend" , "Revenue" , "Population"]])

# verificando qual é o cluster de cada loja
pred = kmeans.predict(data3[["Marketing Spend" , "Revenue" , "Population"]])

# adicionando essa informação no conjunto de dados
frame = pd.DataFrame(data3)
frame['Cluster'] = pred + 1

# exibindo quantas lojas existem em cada cluster
frame['Cluster'].value_counts()
```




    1    55
    2    50
    3    45
    Name: Cluster, dtype: int64




```python
frame.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store ID</th>
      <th>City</th>
      <th>State</th>
      <th>Sales Region</th>
      <th>New Expansion</th>
      <th>Marketing Spend</th>
      <th>Revenue</th>
      <th>Population</th>
      <th>Cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Peoria</td>
      <td>Arizona</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2601</td>
      <td>48610</td>
      <td>171237</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Midland</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2727</td>
      <td>45689</td>
      <td>132950</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Spokane</td>
      <td>Washington</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2768</td>
      <td>49554</td>
      <td>213272</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Denton</td>
      <td>Texas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2759</td>
      <td>38284</td>
      <td>131044</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Overland Park</td>
      <td>Kansas</td>
      <td>Region 2</td>
      <td>Old</td>
      <td>2869</td>
      <td>59887</td>
      <td>186515</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Filtrando pontos do cluster 1
c1 = frame[frame["Cluster"] == 1]
X1 = c1["Marketing Spend"].values.reshape(-1,1) 
y1 = c1["Revenue"].values.reshape(-1,1)


# Filtrando pontos do cluster 2
c2 = frame[frame["Cluster"] == 2]
X2 = c2["Marketing Spend"].values.reshape(-1,1) 
y2 = c2["Revenue"].values.reshape(-1,1)
           
           
# Filtrando pontos do cluster 3
c3 = frame[frame["Cluster"] == 3]
X3 = c3["Marketing Spend"].values.reshape(-1,1) 
y3 = c3["Revenue"].values.reshape(-1,1)
```


```python
# plotando o gráfico de dispersão
plt.scatter(X1, y1, color = 'red' , label = "Cluster 1" , s=50, alpha=0.8)
plt.scatter(X2, y2, color = 'darkcyan' , label = "Cluster 2" ,  s=50, alpha=0.8)
plt.scatter(X3, y3, color = 'orange' , label = "Cluster 3" , s=50, alpha=0.8)

# configurando elementos do gráfico
plt.title("CLUSTERS")
plt.xlabel('INVESTIMENTO EM MARKETING')
plt.ylabel('RECEITA')
plt.legend(loc = (1.01, 0.83))

plt.show()
```


![png](output_30_0.png)


A figura cima mostra a divisão das lojas nos três clusters. Alguns pontos vermelhos estão na região alaranjada, isso se deve ao fato de ao criarmos os cluster estamos levando em consideração três variáveis (Receita, Investimento em Marketing e o Tamanho da População) e com isso estamos vendo a projeção em 2 dimensões de algo tridimensional.

Agora vamos verificar o  tamanho da população de cada cluster, para ver se nossa hipótese sobre o Cluster 1 (em vermelho) tinha fundamentos:


```python
print("Cluster 1: ")
print("População mínimia: " + str(c1["Population"].min()))
print("População média: " + str(c1["Population"].mean()))
print("População máxima: " + str(c1["Population"].max()))

print("Cluster 2: ")
print("População mínimia: " + str(c2["Population"].min()))
print("População média: " + str(c2["Population"].mean()))
print("População máxima: " + str(c2["Population"].max()))

print("Cluster 3: ")
print("População mínimia: " + str(c3["Population"].min()))
print("População média: " + str(c3["Population"].mean()))
print("População máxima: " + str(c3["Population"].max()))
```

    Cluster 1: 
    População mínimia: 100242
    População média: 106804.61818181818
    População máxima: 113204
    Cluster 2: 
    População mínimia: 166913
    População média: 190854.2
    População máxima: 216108
    Cluster 3: 
    População mínimia: 126118
    População média: 135238.13333333333
    População máxima: 148278
    

Realmente o Cluster 1 é o que possui lojas que se encontram em cidades com os menores tamanhos populacionais, o que faz com que investimento em propaganda esteja "saturado" para essa população, não trazendo melhoria na receita

As lojas situadas no Cluster 2 (verde) se encontram nas cidades que possuem os maiores tamanhos populacionais. As lojas do Cluster 3 (laranja) atendem uma população de tamanho intermediário.

### 3.2) Clusters e Regressão Linear


Agora vamos identificar qual a melhor reta que descreve cada cluster, para podermos identificar qual Cluster é mais favorável a receber investimentos em Marketing


```python
from sklearn.linear_model import LinearRegression

# cluster 1
linearreg = LinearRegression()
lr1 = linearreg.fit(X1, y1)


# cluster 2
linearreg = LinearRegression()
lr2 = linearreg.fit(X2, y2)


# cluster 3
linearreg = LinearRegression()
lr3 = linearreg.fit(X3, y3)
```


```python
# plotando o gráfico de dispersão
plt.scatter(X1, y1, color = 'red' , label = "Cluster 1" , s=50, alpha=0.8)
plt.scatter(X2, y2, color = 'darkcyan' , label = "Cluster 2" ,  s=50, alpha=0.8)
plt.scatter(X3, y3, color = 'orange' , label = "Cluster 3" , s=50, alpha=0.8)

# plotando as melhores retas
plt.plot(X1, lr1.predict(X1) , color = 'red' , alpha=0.5)
plt.plot(X2, lr2.predict(X2) , color = 'darkcyan' , alpha=0.5)
plt.plot(X3, lr3.predict(X3) , color = 'orange' , alpha=0.5)

# configurando elementos do gráfico
plt.title("CLUSTERS E REGRESSÃO LINEAR")
plt.xlabel('INVESTIMENTO EM MARKETING')
plt.ylabel('RECEITA')
plt.legend(loc = (1.01, 0.83))

plt.show()
```


![png](output_36_0.png)


Quanto maior a inclinação da reta mostrada no gráfico acima, maior o impacto do investimento em Marketing na Receita. Visualmente podemos perceber que a reta do Cluster 1 está bem horizontal, o que mostra o comportamento que percebemos anteriormente de "saturação" do investimento. Agora nos resta saber entre os Cluster 2 e 3, que estão inclinados positivamente, qual deles é o melhor. Para isso iremos encontrar os coeficientes angulares de cada reta.


```python
# coeficientes angulares

a1 = lr1.coef_
a2 = lr2.coef_
a3 = lr3.coef_

print("Cluster 1: " + str(a1).strip('[]'))
print("Cluster 2: " + str(a2).strip('[]'))
print("Cluster 3: " + str(a3).strip('[]'))

```

    Cluster 1: 0.54330482
    Cluster 2: 3.17218152
    Cluster 3: 7.03989011
    

Como o Cluster 3 (laranja) apresenta o maior coeficiente angular (maior inclinação da reta), suas lojas são as que a empresa deve investir em Marketing. Nas lojas do Cluster 3, para cada 1,00 Dólar investido em Marketing teremos um aumento na Receita de 7,03 Dólares.  


Como a empresa deseja que identificar quais das 10 novas lojas possuem o melhor potencial de investimento em Marketing, iremos verificar quais dessas novas lojas estão no melhor cluster para se investir.


```python
# filtrar as novas expansões do cluster 3
new_in3 = c3[c3["New Expansion"] == "New"]
new_in3.set_index('Store ID', inplace=True)
new_in3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>State</th>
      <th>Sales Region</th>
      <th>New Expansion</th>
      <th>Marketing Spend</th>
      <th>Revenue</th>
      <th>Population</th>
      <th>Cluster</th>
    </tr>
    <tr>
      <th>Store ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>143</th>
      <td>Joliet</td>
      <td>Illinois</td>
      <td>Region 1</td>
      <td>New</td>
      <td>3279</td>
      <td>48315</td>
      <td>147861</td>
      <td>3</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Paterson</td>
      <td>New Jersey</td>
      <td>Region 1</td>
      <td>New</td>
      <td>2251</td>
      <td>34603</td>
      <td>147754</td>
      <td>3</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Rockford</td>
      <td>Illinois</td>
      <td>Region 1</td>
      <td>New</td>
      <td>2648</td>
      <td>43377</td>
      <td>148278</td>
      <td>3</td>
    </tr>
    <tr>
      <th>150</th>
      <td>Thousand Oaks</td>
      <td>California</td>
      <td>Region 2</td>
      <td>New</td>
      <td>2431</td>
      <td>40141</td>
      <td>129339</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plotando o gráfico de dispersão
plt.scatter(X3, y3, color = 'grey' , label = "Cluster 3 - Antigas" , s=50, alpha=0.4)

# plotando as melhores retas
plt.plot(X3, lr3.predict(X3) , color = 'grey' , alpha=0.5)

# destacando as novas expansões
X3_new = new_in3["Marketing Spend"]
y3_new = new_in3["Revenue"]
plt.scatter( X3_new , y3_new  , label = "Cluster 3 - Novas " , color = "orange" , s = 70 , alpha = 1.0)

# escrevendo o nome das cidades no gráfico
for x,y,city in zip(X3_new, y3_new, new_in3["City"]):

    plt.annotate(city, # this is the text
                 (x,y), # this is the point to label
                 textcoords="offset points", # how to position the text
                 xytext=(0,7), # distance from text to points (x,y)
                 ha='center') # horizontal alignment can be left, right or center


# configurando elementos do gráfico
plt.title("NOVAS LOJAS COM MAIOR POTÊNCIAL PARA INVESTIMENTO")
plt.xlabel("Marketing Spend")
plt.ylabel("Revenue")
plt.legend(loc = (1.01, 0.88))
plt.show()
```


![png](output_41_0.png)



```python
print("Melhores lojas (dentre as novas lojas) para se investir:")

new_in3[["City" , "State"]]
```

    Melhores lojas (dentre as novas lojas) para se investir:
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>State</th>
    </tr>
    <tr>
      <th>Store ID</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>143</th>
      <td>Joliet</td>
      <td>Illinois</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Paterson</td>
      <td>New Jersey</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Rockford</td>
      <td>Illinois</td>
    </tr>
    <tr>
      <th>150</th>
      <td>Thousand Oaks</td>
      <td>California</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
