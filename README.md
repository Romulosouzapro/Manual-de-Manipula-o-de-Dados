# Manual de manipulação de dados

Contém os principais métodos usados no dia-a-dia para análise de dados, segmentação e tratamento dos dados.

```python
import numpy as np
import pandas as pd
```
#### Drop
```python
df = df.drop(columns=['Coluna'])
```

#### GroupBy
```python
df_media = (
    df.groupby('coluna')['valor']
    .mean()
    .rename('avg')
    .reset_index()
)

#Cria um count sem precisar de criar outro df e fazer merge
df['Total'] = df.groupby('Coluna').transform('size')

```

#### Where
```python
new['coluna'] = new['coluna'].where(~(new['coluna2'] == 1),True)

#more performed
new['coluna'] = np.where(new['coluna2'] == 1, True, new['coluna'])
```

#### Loc + mask
```python
mask = (new['coluna2'] == 'referencia')
new.loc[mask, 'coluna'] = 'valor'
```

#### Valores nulos/blank
```python
new['coluna'].isna()
new['coluna'].notna()
pd.isna(new['coluna'])
new['coluna'] = new['coluna'].fillna(0).astype(int)
```

#### Coalesce
```python
new['coluna'] = new['coluna'].combine_first(new['coluna2'])
```

#### Tratamento strings
```python
new['coluna'] = new['coluna'].str.capitalize()
new['coluna'] = new['coluna'].str.extract(r'\b([0-9_]{8}\S)\b')
```

#### Datetime
```python
diferenca = abs((hora1 - hora2).total_seconds())

data_inicio = '2025-01-01' / data_fim = datetime.now()
lista_datas = pd.date_range(start=data_inicio, end=data_fim, freq='MS').strftime('%Y-%m-%d').tolist()
['2025-01-01', '2025-02-01',...,'2026-03-01']

```

####PowerBi
each Text.Start(Text.From(_, "pt-PT"), 4) &"-"& Text.Middle(Text.From(_, "pt-PT"), 4, 2) &"-"& Text.End(Text.From(_, "pt-PT"), 2)
let
 Source = List.Dates(#date(2024,3,1), Number.From(DateTime.LocalNow())-Number.From(#date(2024,3,2)), #duration(1,0,0,0)),
 #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
 #"Renamed Columns" = Table.RenameColumns(#"Converted to Table",{{"Column1", "Data"}}),
 #"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"Data", type date}}),
 #"Inserted Year" = Table.AddColumn(#"Changed Type", "Year", each Date.Year([Data]), Int64.Type),
 #"Inserted Month" = Table.AddColumn(#"Inserted Year", "Month", each Date.Month([Data]), Int64.Type),
 #"Inserted Month Name" = Table.AddColumn(#"Inserted Month", "Month Name", each Date.MonthName([Data]), type text),
 #"Capitalized Each Word" = Table.TransformColumns(#"Inserted Month Name",{{"Month Name", Text.Proper, type text}}),
 #"Extracted First Characters" = Table.TransformColumns(#"Capitalized Each Word", {{"Month Name", each Text.Start(_, 3), type text}}),
 #"Inserted Day Name" = Table.AddColumn(#"Extracted First Characters", "Day Name", each Date.DayOfWeekName([Data]), type text),
 #"Inserted Start of Week" = Table.AddColumn(#"Inserted Day Name", "Start of Week", each Date.StartOfWeek([Data]), type date),
 #"Inserted Day" = Table.AddColumn(#"Inserted Start of Week", "Day", each Date.Day([Data]), Int64.Type),
 #"Reordered Columns" = Table.ReorderColumns(#"Inserted Day",{"Data", "Year", "Month", "Month Name", "Day", "Day Name", "Start of Week"}),
 #"Changed Type1" = Table.TransformColumnTypes(#"Reordered Columns",{{"Day", type text}}),
 #"Added Custom" = Table.AddColumn(#"Changed Type1", "Custom", each [Day] & "/" & [Month Name]),
 #"Renamed Columns1" = Table.RenameColumns(#"Added Custom",{{"Custom", "Diames"}})
in
 #"Renamed Columns1"



