# Principais medidas DAX

```DAX
Receita Líquida =
SUM ( FatoRealizadoFinanceiro[ReceitaLiquida] )
```

```DAX
Custos Totais =
SUM ( FatoRealizadoFinanceiro[CustosTotais] )
```

```DAX
Lucro Operacional =
SUM ( FatoRealizadoFinanceiro[LucroOperacional] )
```

```DAX
Margem % =
DIVIDE ( [Lucro Operacional], [Receita Líquida] )
```

```DAX
Receita Orçada =
SUM ( FatoOrcamento[ReceitaOrcada] )
```

```DAX
Custos Orçados =
SUM ( FatoOrcamento[CustosOrcados] )
```

```DAX
Lucro Orçado =
SUM ( FatoOrcamento[LucroOrcado] )
```

```DAX
Margem Orçada % =
DIVIDE ( [Lucro Orçado], [Receita Orçada] )
```

```DAX
Desvio Lucro =
[Lucro Operacional] - [Lucro Orçado]
```

```DAX
Desvio Lucro % =
DIVIDE ( [Desvio Lucro], [Lucro Orçado] )
```

```DAX
Desvio Custos =
[Custos Totais] - [Custos Orçados]
```

```DAX
Desvio Custos % =
DIVIDE ( [Desvio Custos], [Custos Orçados] )
```

```DAX
Desvio da Margem p.p. =
( [Margem %] - [Margem Orçada %] ) * 100
```

```DAX
Lucro Ano Anterior =
VAR AnoAtual = MAX ( DimCalendario[Ano] )
RETURN
    CALCULATE (
        [Lucro Operacional],
        REMOVEFILTERS ( DimCalendario[Ano] ),
        DimCalendario[Ano] = AnoAtual - 1
    )
```

```DAX
Crescimento Anual do Lucro % =
IF (
    HASONEVALUE ( DimCalendario[Ano] ),
    VAR LucroAtual = [Lucro Operacional]
    VAR AnoAtual = SELECTEDVALUE ( DimCalendario[Ano] )
    VAR LucroAnterior =
        CALCULATE (
            [Lucro Operacional],
            REMOVEFILTERS ( DimCalendario[Ano] ),
            DimCalendario[Ano] = AnoAtual - 1
        )
    RETURN
        DIVIDE ( LucroAtual - LucroAnterior, LucroAnterior )
)
```

```DAX
Rank Produto por Receita =
RANKX (
    ALLSELECTED ( DimProduto[Produto] ),
    [Receita Líquida],
    ,
    DESC,
    DENSE
)
```

```DAX
Leitura do Período Resumida =
VAR DesvioLucro = [Desvio Lucro %]
VAR DesvioMargem = [Desvio da Margem p.p.]
VAR DesvioCustos = [Desvio Custos %]
RETURN
SWITCH (
    TRUE(),
    DesvioLucro > 0 && DesvioMargem < 0 && DesvioCustos > 0,
        "Lucro acima do orçamento, mas com pressão de custos e margem abaixo do planejado.",
    DesvioLucro < 0 && DesvioMargem < 0 && DesvioCustos > 0,
        "Resultado abaixo do orçamento, pressionado por custos elevados e menor margem.",
    DesvioLucro > 0 && DesvioMargem > 0 && DesvioCustos <= 0,
        "Resultado favorável, com lucro e margem acima do planejado e custos sob controle.",
    DesvioLucro < 0 && DesvioCustos <= 0,
        "Custos estão controlados, mas o lucro permanece abaixo do orçamento.",
    "O período apresenta sinais mistos entre lucro, margem e custos."
)
```
