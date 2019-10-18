# pandas-tricks
###### Coisas que são úteis saber para mexer com [pandas](https://pandas.pydata.org/pandas-docs/stable/search.html) :panda_face: :panda_face: :panda_face:
  Sabe aqueles pedacinhos de código que resolvem muita coisa?
  o pandas é cheio deles.
 
### Antes de mais nada
  Ideal pra deixar logo depois do import.
  Essencial quando estiver olhando um arquivo no *IPython*
  

```Python console:
pd.options.display.max_rows = 100
pd.options.display.max_columns = 50
```

### Criando DataFrames

Existe um pd.read_* pra quase tudo :relaxed:
meus mais usados:
```Python console:
pd.read_csv(io=caminho_arquivo)
pd.read_json(io=caminho_arquivo)
pd.read_excel(io=caminho_arquivo, sheet_name=planilha)*
```
* Use ```sheet_name=None``` pra criar um ```OrderedDict``` com todas as planilhas do arquivo.

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
