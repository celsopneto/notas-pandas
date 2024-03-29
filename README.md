# notas-pandas
  Notas sobre o [pandas][1]

## Índice
+  [Introdução](#o-que-é-pandas)
  + [Onde usar?](#onde-posso-usar)
  + [Estruturas de dados](#estruturas-de-dados)
+ [Como usar](#e-como-eu-faço)
  + [Instalar](#instalando)
  + [Importar](#importando)
  + [Criar DataFrame](#criando-dataframes)
    + [Testes e checagens rápidas](#testes-e-checagens-rápidas)
  + [Inspecionando/visualizando dados](#inspecionando-dados)
  + [Selecionando](#selecionando)
  + [Modificando](#modificando)
  + [Renomeando colunas](#renomeando-colunas)
  + [Agrupamentos](#agrupamento-o-groupby)
  

## Referências

## O que é pandas? 

  Um pacote de análise de dados e estatística para [Python][2] que visa fornecer estruturas de dados rápidas e flexíveis para trabalhar com dados relacionais ou classificados.

### Onde posso usar?
  pandas pode ser utilizado para diversos tipos de dados:
  
  + Dados tabulares e de tipos heterogêneos, como tabelas SQL ou de Excel.
  
  + Dados de séries temporais ordenados e não ordenados (sem uma frequência específica).
  
  + Qualquer outro tipo de dados observacionais/estatísticos ou matrizes com linhas e colunas.

### Estruturas de dados
As duas estruturas de dados principais do pandas são as `Series` e os `DataFrames`, ambos servem como recipientes para praticamente qualquer tipo de dado. É interessante pensar neles de forma hierárquica, ex: um `DataFrame` é composto por `Series` que são compostas por `str`, `int` etc. 



| Dimensões | Nome | Apelido |
| :--- | :--- | :--- |
| 1 | Series | `s` |
| 2 | DataFrame | `df` |


## E como eu faço?

### Instalando

  Se você usa [Anaconda][3] ele já está lá!
  
  Caso contrário `pip install pandas` no seu terminal de preferência resolve :smile:
  
### Importando
  ```Python console:
import pandas as pd
```


Ideal pra deixar logo depois do import.
  Essencial quando estiver olhando um arquivo no [*IPython*][4]


```Python console:
pd.options.display.max_rows = x  
pd.options.display.max_columns = y
```


## Criando DataFrames

Os __DataFrames__ são estrutura de dados primária do pandas.

```Python console:
df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
```

### Funções de leitura de input
Existe um `pd.read_*` para quase toda extensão de arquivo

```Python console:
pd.read_csv(nome_arquivo)
pd.read_json(nome_arquivo)
pd.read_excel(nome_arquivo, sheet_name=planilha) # sheet_name=None cria um OrderedDict com todas as planilhas do arquivo.
```



Também existe um `pd.to_*` pra quase todo `pd.read_*`
```Python console:
pd.to_csv(nome_arquivo)
pd.to_json(nome_arquivo)
#...
```

[Ver mais][8]


Para excel é possível escrever diversos dataframe em diferentes planilhas, exemplo:
```Python console:
writer = pd.ExcelWriter('nome_do_arquivo.xlsx', engine='xlsxwriter')
df1.to_excel(writer, 'DataFrame 1')
df2.to_excel(writer, 'DataFrame 2')
writer.save()
```
#### Testes e checagens rápidas

```Python:
"""
Cria um Dataframe com 20 linhas e 5 colunas de números decimais aleatórios, e dá os nomes `'a','b','c','d' e 'e'`
para as colunas.
"""

import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.rand(20,5))
df.columns = ['a','b','c','d','e']
``` 


> Note que você vai precisar de um `import numpy as np` também.

`pd.Series(minha_lista)` Cria uma série a partir de uma lista

`df.index = pd.date_range('1900/1/30', periods=df.shape[0])` Adiciona um índice de datas


### Inspecionando dados


`df.head(n)` Primeiras n linhas de um  DataFrame

`df.tail(n)` Últimas n rows do DataFrame

`df.shape` Retorna um tuple com o número de linhas e colunas do DataFrame

`df.info()` Informações sobre índices, tipos de dados e consumo de memória

`df.describe()` [Síntese estatística][6] para colunas numéricas

`s.value_counts(dropna=False)` Contagem das ocorrências

`s.unique()` Valores únicos 

`s.nunique()` Contagem de valores únicos

`df.isnull()` Retorna uma matriz booleana do DataFrame onde os valores nulos são True

`pd.notnull()` O contrário de `df.isnull()`
  

### Selecionando

`df[col]` Retorna a coluna "col" como `Series`

`df[[col1, col2]]` Retorna colunas col1, col2 como `DataFrame`

`s.iloc[0]` Seleção por índices numéricos

`s.loc['índice_um']` Seleção por índices nomeados

`df.loc[df[coluna] == 0]` Seleciona todos os items no dataframe que apresentem valor __0__ para a __coluna__

 `df.loc[(df[coluna] == 0) & (df[outra_coluna] > 0)]` Seleciona todos os items no dataframe que apresentem valor __0__ para __coluna__ e maior que __0__ para __outra_coluna__.
 

`df.iloc[0,:]` Primeira linha

`df.iloc[0,0]` Primeiro elemento da primeira linha



### Modificando

`df.dropna()` Deleta todas as linhas que posuem valores nulos

`df.dropna(axis=1)` Deleta todas as colunas que posuem valores nulos

`df.dropna(thresh=n)` Deleta todas as linhas que tenham menos que n valores não nulos

`df.fillna(x)` Troca todos os valores nulos por x

`s.fillna(s.mean())` Troca todos os valores nulos pela média (`mean()` pode ser substituido por outras funções do módulo [statistics][7]

`s.astype(tipo_dado)` Converte os valores da série para o tipo de dado

`s.replace(1,'one')` Troca todos os valores 1 por 'um'

`s.replace([1,3],['um','três'])` Troca todos os 1 por 'um' e 3 por 'três'

`df.set_index(col)` Transforma a coluna col no índice da tabela




#### Renomeando colunas

1. Quando você só precisa renomear mesmo.
```Python console:
df.rename(columns={'Nome':'nome'}, inplace=True)
# ou então:
df.columns = ['a','b','c']
```

2. Depois de um GroupBy (muito útil pra resetar MultiLevelIndexes também).
```Python console:
df.columns = [' '.join(col).strip()  for col in df.columns.values]
```

3. Quando você está desesperado e quer mudar tudo de uma vez
```Python console:
df.rename(columns=lambda x: x + 1)
```

## Agrupamentos, o groupby

```Python console:
df.grouped = df.groupby('column_to_group_by', as_index=False).agg({'other_columns': função ou lista de funções*})
```
funções aceitas:
 * em string ex: `min`, `max`, `mean`
 * definidas pelo usuário
 * funções `lambda`
 
## Referências
[A documentação do pandas][1]

[cheat sheet feita pelo blog da dataquest][5]

[1]: https://pandas.pydata.org/pandas-docs/stable/
[2]: https://www.python.org/
[3]: https://www.anaconda.com/distribution/
[4]: https://ipython.org/
[5]: https://www.dataquest.io/blog/pandas-cheat-sheet/
[6]: https://pt.wikipedia.org/wiki/S%C3%ADntese_estat%C3%ADstica
[7]: https://docs.python.org/3/library/statistics.html
[8]: https://pandas.pydata.org/pandas-docs/stable/reference/frame.html#serialization-io-conversion

