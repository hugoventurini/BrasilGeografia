# Dicionário de Dados

Este documento descreve os campos e métricas utilizados no projeto **Brasil Geografia**.

---

## Tabelas / Consultas

### Tabela: `Municipios` _(exemplo — preencher conforme o modelo real)_

| Campo | Tipo | Descrição | Exemplo |
|-------|------|-----------|---------|
| `CD_MUN` | Texto | Código IBGE do município (7 dígitos) | `3550308` |
| `NM_MUN` | Texto | Nome do município | `São Paulo` |
| `CD_UF` | Texto | Código da Unidade Federativa (2 dígitos) | `35` |
| `NM_UF` | Texto | Nome do estado | `São Paulo` |
| `SIGLA_UF` | Texto | Sigla do estado | `SP` |
| `CD_RGINT` | Texto | Código da Região Geográfica Intermediária | `351` |
| `NM_RGINT` | Texto | Nome da Região Geográfica Intermediária | `São Paulo` |
| `CD_RGI` | Texto | Código da Região Geográfica Imediata | `3501` |
| `NM_RGI` | Texto | Nome da Região Geográfica Imediata | `São Paulo` |
| `CD_REGIAO` | Texto | Código da Região do Brasil (1 dígito) | `3` |
| `NM_REGIAO` | Texto | Nome da Região do Brasil | `Sudeste` |
| `AR_MUN_2022` | Decimal | Área do município em km² (2022) | `1521,11` |

---

### Tabela: `UF` _(exemplo)_

| Campo | Tipo | Descrição | Exemplo |
|-------|------|-----------|---------|
| `CD_UF` | Texto | Código da UF | `35` |
| `NM_UF` | Texto | Nome do estado | `São Paulo` |
| `SIGLA_UF` | Texto | Sigla | `SP` |
| `CD_REGIAO` | Texto | Código da região | `3` |
| `NM_REGIAO` | Texto | Nome da região | `Sudeste` |
| `AR_UF_2022` | Decimal | Área da UF em km² (2022) | `248219,63` |

---

## Medidas (Measures)

| Medida | Fórmula DAX (resumida) | Descrição |
|--------|------------------------|-----------|
| `Total Municípios` | `COUNTROWS(Municipios)` | Quantidade total de municípios selecionados |
| `Área Total (km²)` | `SUM(Municipios[AR_MUN_2022])` | Soma da área territorial dos municípios selecionados |

---

## Notas

- Preencha as tabelas acima com os campos reais do modelo de dados.
- Inclua todas as tabelas, colunas calculadas e medidas utilizadas no relatório.
