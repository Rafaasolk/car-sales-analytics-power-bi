# Indicadores e regras de negócio

## Receita e volume

- `$ Faturamento`: soma, por linha de venda, quantidade multiplicada pelo preço associado ao veículo (`SUMX` + `RELATED`).
- `QTD Vendida`: soma a quantidade de veículos vendida, não a quantidade de transações.
- `$ Ticket Medio`: divide faturamento por **quantidade de veículos vendidos**; portanto, no código disponível é receita média por unidade, não necessariamente por transação.

## Custos e margem

- `$ Custo`: soma quantidade multiplicada pelo custo de cada veículo.
- `% Margem`: `(faturamento - custo) / faturamento`, via `DIVIDE`.

## Inteligência temporal

- `$ Faturamento LY`: período equivalente do ano anterior (`SAMEPERIODLASTYEAR`).
- `$ Faturamento YTD`: acumulado do ano (`DATESYTD`).
- `$ Faturamento LYTD`: acumulado no contexto deslocado para o ano anterior.
- `% Faturamento YoY`: variação percentual em relação ao ano anterior.
- `Ano selecionado`: ano do contexto de filtro (`SELECTEDVALUE`).

## Sazonalidade e horários

- `Rank dia semana`: classificação de dias da semana pelo faturamento.
- `Rank hora`: classificação de horas do dia pelo faturamento.
- `% Fat top1:semana`: distância percentual entre o melhor dia e a média semanal.
- `% Fat top7:semana`: distância percentual entre o sétimo dia e a média semanal.
- `Top 1 dia`, `Top 1 hora`, `Top 7 dia`: expressões voltadas à identificação de posições no ranking.

## Limitações de interpretação

Os nomes e as expressões foram preservados. `Top 1 dia` e `Top 1 hora` usam `TOPN` diretamente e merecem verificação no Power BI por retornarem tabela. A denominação `Ticket Medio` foi descrita segundo seu denominador efetivo. Não foram inferidos resultados de negócio, acurácia, ou validações que não constem dos arquivos.
