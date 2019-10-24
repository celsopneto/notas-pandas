# pandas-tricks
###### Coisas que são úteis saber para mexer com [pandas][1] :panda_face: :panda_face: :panda_face:
  Sabe aqueles pedacinhos de código que resolvem muita coisa?
  o pandas é cheio deles.
 
## Antes de mais nada

### O que é pandas? 

  Um pacote de análise de dados e estatística para [Python][2] que visa fornecer estruturas de dados rápidas e flexíveis para trabalhar com dados "relacionais" ou "classificados

### Onde posso usar?
  pandas pode ser utilizado para diversos tipos de dados:
  
  + Dados tabulares e de tipos heterogêneos, como tabelas SQL ou de Excel.
  
  + Dados de séries temporais ordenados e não ordenados (sem uma frequência específica).
  
  + Qualquer outro tipo de dados observacionais/estatísticos ou matrizes com linhas e colunas.

### Estruturas de dados
As duas estruturas de dados principais do pandas são as `Series` e os `DataFrames`, ambos servem como recipientes para praticamente qualquer tipo de dado. É interessante pensar neles de forma hierárquica, ex: um `DataFrame` é composto por `Series` que são compostas por `str`, `int` etc.

| Dimensões | Nome | Descrição |
| :--- | :--- | :--- |
| 1 | Series | Dados unidimensionais (pense em listas) rotulados e de tipos homogêneos |
| 2 | DataFrame | Dados bidimensionais (pense em tabelas)rotulados em colunas com dados potencialmente heterogêneos |


## E como eu faço?
  Se você usa [Anaconda][3] ele já está lá!
  
  Caso contrário `pip install pandas` no seu terminal de preferência resolve :smile:
  
  ```Python console:
import pandas as pd
```
  

  

  Ideal pra deixar logo depois do import.
  Essencial quando estiver olhando um arquivo no [*IPython*][4]
  

```Python console:
pd.options.display.max_rows = 100
pd.options.display.max_columns = 50
```

## Criando DataFrames

Existe um pd.read_* pra quase tudo :relaxed:
meus mais usados:
```Python console:
pd.read_csv(io=caminho_arquivo)
pd.read_json(io=caminho_arquivo)
pd.read_excel(io=caminho_arquivo, sheet_name=planilha)*
```
* Use `sheet_name=None` pra criar um `OrderedDict` com todas as planilhas do arquivo.

### Testes e checagens rápidas

`pd.DataFrame(np.random.rand(20,5))` Cria um Dataframe com 20 linhas e 5 colunas de números decimais aleatórios* 
`pd.Series(minha_lista)` Cria uma série a partir de uma lista
`df.index = pd.date_range('1900/1/30', periods=df.shape[0])` Adiciona um índice de datas

* Nesse caso você vai precisar de um `import numpy as np` também.

### Renomeando colunas

1. Quando você só precisa renomear mesmo.
```Python console:
df.rename(columns={'Nome':'nome'}, inplace=True)
```

2. Depois de um GroupBy (muito útil pra resetar MultiLevelIndexes também :smiley:).
```Python console:
df.columns = [' '.join(col).strip()  for col in df.columns.values]
```

### GroupBy
Agrupação dos dados pode ser por uma coluna:
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
