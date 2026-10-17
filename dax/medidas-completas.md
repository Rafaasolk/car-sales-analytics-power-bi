# Catálogo completo de medidas DAX

As **17 expressões abaixo foram transcritas do inventário técnico fornecido**, mantendo os nomes e as fórmulas do modelo original. As descrições acompanham o arquivo-fonte; não são validação independente dos resultados.

## medidas padrao

### % Faturamento YoY

O que faz: Calcula a variação percentual do faturamento em relação ao mesmo período do ano anterior.

```dax
DIVIDE ( [$ Faturamento] - [$ Faturamento LY], [$ Faturamento LY] ) + 0
```

### % Margem

O que faz: Calcula a margem de contribuição como percentual do faturamento (faturamento menos custo dividido por faturamento).

```dax
DIVIDE ( [$ Faturamento] - [$ Custo], [$ Faturamento] )
```

### $ Custo

O que faz: Soma o custo total multiplicando quantidade vendida pelo custo de aquisição de cada veículo.

```dax
SUMX ( 
    fVendas,
    fVendas[quantidade] * RELATED ( dVeiculo[custo_veiculo] )
)
```

### $ Faturamento

O que faz: Soma o faturamento total multiplicando quantidade vendida pelo preço de cada veículo.

```dax
SUMX (
    fVendas,
    fVendas[quantidade] * RELATED ( dVeiculo[valor_venda] )
)
```

### $ Faturamento LY

O que faz: Repete o faturamento do mesmo período no ano anterior, permitindo comparação anual.

```dax
CALCULATE (
    [$ Faturamento],
    SAMEPERIODLASTYEAR ( dCalendario[data] )
)
```

### $ Faturamento LYTD

O que faz: Soma o faturamento acumulado no ano anterior até a mesma data, permitindo comparação YTD.

```dax
CALCULATE (
    [$ Faturamento LY],
    DATESYTD ( dCalendario[data] )
)
```

### $ Faturamento YTD

O que faz: Soma o faturamento acumulado desde o início do ano até a data selecionada.

```dax
CALCULATE (
    [$ Faturamento],
    DATESYTD ( dCalendario[data] )
)
```

### $ Ticket Medio

O que faz: Divide o faturamento total pela quantidade de transações, indicando o valor médio por venda.

```dax
DIVIDE ( [$ Faturamento], [QTD Vendida] )
```

### Ano selecionado

O que faz: Retorna o ano selecionado no filtro, usualmente para mostrar em cartão ou validar contexto.

```dax
SELECTEDVALUE ( dCalendario[ano] )
```

### QTD Vendida

O que faz: Conta a quantidade total de veículos vendidos no período filtrado.

```dax
SUM ( fVendas[quantidade] )
```

## medidas rank

### % Fat top1:semana

O que faz: Calcula quantos por cento o melhor dia da semana supera a média semanal.

```dax
VAR varAvgSemana = AVERAGEX( VALUES(dCalendario[dia_semana]), [$ Faturamento])
VAR varFatTop1 = 
CALCULATE(
    [$ Faturamento],
    FILTER(
        VALUES(dCalendario[dia_semana]),
        [Rank dia semana] = 1
    )
)
RETURN
DIVIDE ( varFatTop1 - varAvgSemana, varAvgSemana )
```

### % Fat top7:semana

O que faz: Calcula quantos por cento o pior dia da semana fica abaixo da média semanal.

```dax
VAR varAvgSemana = AVERAGEX( VALUES(dCalendario[dia_semana]), [$ Faturamento])
VAR varFatTop7 = 
CALCULATE(
    [$ Faturamento],
    FILTER(
        VALUES(dCalendario[dia_semana]),
        [Rank dia semana] = 7
    )
)
RETURN
DIVIDE ( varAvgSemana - varFatTop7 , varAvgSemana )
```

### Rank dia semana

O que faz: Ordena os dias da semana pelo faturamento gerado, de maior para menor.

```dax
RANKX ( 
    ALL( dCalendario[dia_semana],dCalendario[nome_dia_semana] ),
    [$ Faturamento]
)
```

### Rank hora

O que faz: Ordena as horas do dia pelo faturamento gerado, de maior para menor.

```dax
RANKX ( 
    ALL ( dHora[Hora Num] ),
    [$ Faturamento]
)
```

### Top 1 dia

O que faz: Identifica qual dia da semana teve o maior faturamento no período.

```dax
TOPN(
    1,
    VALUES(dCalendario[nome_dia_semana]),
    [$ Faturamento]
)
```

### Top 1 hora

O que faz: Identifica qual hora do dia teve o maior faturamento no período.

```dax
TOPN(
    1,
    VALUES(dHora[Hora Num]),
    [$ Faturamento]
)
```

### Top 7 dia

O que faz: Identifica qual é o sétimo melhor dia da semana em faturamento (o pior dia).

```dax
CALCULATE(
    SELECTEDVALUE(dCalendario[nome_dia_semana]),
    FILTER(
        VALUES(dCalendario[nome_dia_semana]),
        [Rank dia semana] = 7
    )
)
```

## Nota de leitura

As expressões `Top 1 dia` e `Top 1 hora` aparecem no inventário como medidas contendo `TOPN`, que retorna uma tabela. O catálogo preserva exatamente o código reportado, sem presumir que essas expressões possam ser usadas como medidas escalares sem ajustes.
