# Base de dados

A base utilizada neste projeto é **sintética e reproduzível**, criada exclusivamente para fins de portfólio.

O arquivo `Base_Performance_Financeira_Rentabilidade_PTBR.zip` contém as tabelas em CSV, com separador `;` e vírgula decimal, prontas para uso em ambiente pt-BR.

## Período

- janeiro de 2024 a setembro de 2026

## Dimensões

- 8 unidades
- 4 regiões
- 5 categorias
- 30 produtos

## Tabelas

- `DimCalendario`
- `DimCategoria`
- `DimProduto`
- `DimUnidade`
- `FatoRealizadoFinanceiro`
- `FatoOrcamento`

## Granularidade

- `FatoRealizadoFinanceiro`: mês × unidade × produto
- `FatoOrcamento`: mês × unidade × categoria

O orçamento **não é alocado artificialmente por produto**. Por isso, comparações entre realizado e orçamento são feitas apenas nos níveis em que o orçamento realmente existe.
