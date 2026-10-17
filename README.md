# Car Sales Analytics | Power BI

Projeto de Business Intelligence voltado à análise de vendas de veículos, com indicadores de faturamento, custos, margem de contribuição e comportamento comercial ao longo do tempo.

## Objetivo

Transformar registros de vendas em indicadores que permitam acompanhar o desempenho comercial, comparar períodos e identificar os dias e horários de maior faturamento.

## Indicadores e análises

- **Faturamento:** receita estimada a partir da quantidade vendida e do valor de venda de cada veículo.
- **Custos e margem:** custo calculado por veículo e margem de contribuição percentual.
- **Volume de vendas:** quantidade total de veículos vendidos.
- **Ticket médio:** faturamento dividido pela quantidade vendida, conforme a definição implementada no modelo.
- **Inteligência temporal:** faturamento acumulado no ano (YTD), mesmo período do ano anterior (LY), acumulado equivalente do ano anterior (LYTD) e variação anual (YoY).
- **Análise de sazonalidade:** ranking de faturamento por dia da semana e por hora do dia, incluindo identificação de períodos de maior e menor desempenho.

## Modelo de dados

O modelo analítico utiliza uma tabela fato e três dimensões:

| Tabela | Papel | Registros informados na documentação |
| --- | --- | ---: |
| `fVendas` | Fato de vendas, com quantidade, data, hora e veículo | 7.909 |
| `dVeiculo` | Cadastro de veículos, valores e custos | 8 |
| `dCalendario` | Dimensão de datas para análise temporal | 1.096 |
| `dHora` | Dimensão de tempo para análise intradiária | 86.400 |

Os três relacionamentos são ativos, do tipo muitos-para-um (`N:1`) e com filtragem unidirecional, conectando `fVendas` às dimensões por veículo, data e hora. O modelo possui **17 medidas DAX** organizadas em grupos de indicadores padrão e rankings.

## Exemplos de medidas DAX

```dax
$ Faturamento =
SUMX (
    fVendas,
    fVendas[quantidade] * RELATED ( dVeiculo[valor_venda] )
)

$ Custo =
SUMX (
    fVendas,
    fVendas[quantidade] * RELATED ( dVeiculo[custo_veiculo] )
)

% Margem =
DIVIDE ( [$ Faturamento] - [$ Custo], [$ Faturamento] )

$ Faturamento YTD =
CALCULATE (
    [$ Faturamento],
    DATESYTD ( dCalendario[data] )
)

% Faturamento YoY =
DIVIDE ( [$ Faturamento] - [$ Faturamento LY], [$ Faturamento LY] ) + 0
```

## Tecnologias e conceitos

- **Power BI:** modelo semântico e análises de negócio.
- **DAX:** medidas iterativas, contexto de filtro, inteligência temporal e rankings.
- **Power Query:** camada de obtenção e transformação de dados do Power BI.
- **Modelagem dimensional:** tabela fato, dimensões e relacionamentos `N:1`.
- **Fontes identificadas:** CSV, Excel, JSON e SQL.

## Estrutura sugerida do repositório

```text
car-sales-analytics-power-bi/
├── README.md
├── images/
│   ├── overview.png
│   ├── temporal-analysis.png
│   └── data-model.png
└── dax/
    └── medidas-exemplo.md
```

> As imagens são espaços reservados para capturas reais do dashboard e do modelo de dados. Não foram fornecidas capturas de tela do relatório neste material.

## Perguntas de negócio respondidas

1. Qual é o faturamento e como ele evolui em relação ao ano anterior?
2. Qual é o custo associado às vendas e qual a margem de contribuição?
3. Qual é o volume vendido e o valor médio por unidade vendida?
4. Quais dias da semana e horários concentram maior faturamento?
5. Como o faturamento acumulado no ano se compara ao período equivalente anterior?

## Sobre os dados

Esta documentação foi construída a partir de um inventário técnico do modelo Power BI. Os totais acima representam a estrutura descrita no inventário, não resultados financeiros calculados ou evidências de desempenho operacional. O arquivo `.pbix`, os dados originais e as telas do dashboard não estão incluídos neste repositório.

**Privacidade:** antes de publicar arquivos ou capturas, remova caminhos locais, credenciais e quaisquer dados confidenciais.
