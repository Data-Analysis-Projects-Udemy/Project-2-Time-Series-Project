# Project -2 Time Series Project

1. [Importar librerías ](#schema1)
# a.- Análisis del precio de cierre de las acciones y del volumen de operaciones
2. [Hacemos un dataframe con las datos de las compañías.](#schema2)
3. [Obtenemos la lista de los nombres de las compañías que vamos a analizar.](#schema3)
4. [Hacemos un gráfico con los valores de cierre.](#schema4)
5. [Analicemos el volumen total de acciones que se negocian cada día](#schema5)
# b.- Analizamos la devoluciones diarias

<hr>

<a name="schema1"></a>

# 1. Importar librerías

~~~python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
~~~
<hr>

<a name="schema2"></a>

# 2. Hacemos un dataframe con las datos de las compañías.

~~~python
path='./data'
company_list = ['AAPL_data.csv', 'GOOG_data.csv', 'MSFT_data.csv', 'AMZN_data.csv']
all_data = pd.DataFrame()

for file in company_list:
    current_df = pd.read_csv(path+"/"+file)
    all_data = pd.concat([all_data, current_df])
~~~
![img](./images/001.png)
![img](./images/002.png)

<hr>

<a name="schema3"></a>

# 3. Obtenemos la lista de los nombres de las compañías que vamos a analizar.


~~~python
tech_list = all_data['Name'].unique()
~~~
![img](./images/003.png)

<hr>

<a name="schema4"></a>

# 4. Hacemos un gráfico con los valores de cierre.

~~~python
plt.figure(figsize=(20,12))
for i, company in enumerate(tech_list,1):
    plt.subplot(2, 2, i)
    df=all_data[all_data['Name']==company]
    plt.plot(df['date'],df['close'])
    plt.title(company)
    plt.savefig(f"./images/{company}")
~~~

![img](./images/comp.png)



<hr>

<a name="schema5"></a>

# 5. analicemos el volumen total de acciones que se negocian cada día

~~~python
plt.figure(figsize=(20,12))
for i, company in enumerate(tech_list,1):
    plt.subplot(2, 2, i)
    df=all_data[all_data['Name']==company]
    plt.plot(df['date'],df['volume'])
    plt.title(company)
    plt.savefig(f"./images/{company}_stock")
~~~
![img](./images/stock.png)



<hr>

<a name="schema6"></a>

# 6. Importamos las librerías y cargamos el data set

~~~python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv('./data/AAPL_data.csv')
~~~
![img](./images/004.png)

<hr>

<a name="schema7"></a>

# 7. Como no tenemos el precio diario se calcula, después lo ponemos en porcentajes y los dibujamos
~~~python
df["daily_change_price"] = df["close"]- df["open"]
~~~
![img](./images/005.png)
~~~python
df['1day % return']=((df['close']-df['open'])/df['close'])*100
df.head()
~~~
![img](./images/006.png)

~~~python
plt.figure(figsize=(10,6))
df['daily_price_perct'].plot()
plt.savefig("./images/price_perct")
~~~
![img](./images/price_perct.png)

Para obtener unos datos de unos días en concreto

~~~Python
df.set_index('date')['2016-01-01':'2016-03-31']['daily_price_perct'].plot()
plt.xticks(rotation='vertical')
plt.savefig("./images/date.png")
~~~
![img](./images/date.png)

<hr>

<a name="schema7"></a>

# 7. Analizar la media mensual de la columna de cierre
1º Hacemos un a copia del dataset
~~~python
df2=df.copy()
~~~

2º Convertimos los datos `date` como `datatime` porque eran  `object` y lo hacemos el índice del dataset
~~~python
df2.dtypes
~~~
![img](./images/007.png)

~~~python
df2['date']=pd.to_datetime(df2['date'])
df2.set_index('date',inplace=True)
~~~
![img](./images/006.png)

3º Remuestrear los datos del dataset basados en meses
~~~python
df2['close'].resample('M').mean().plot()

~~~
![img](./images/mean-month.png)

4º Remuestrear los datos del dataset basados en años
~~~python
df2['close'].resample('Y').mean().plot(kind = 'bar')
~~~
![img](./images/mean-year.png)
















https://drive.google.com/drive/u/0/folders/10owYwrtRQIRCawOFgZy1qY7gG3ta8VfS