# NFL Play-by-Play — Análise e Regressão Linear

Notebook de ciência de dados aplicado ao dataset de jogadas play-by-play da NFL (2009–2017), cobrindo o pipeline completo de pré-processamento, análise exploratória e modelagem preditiva.

---

## Descrição

Este projeto foi desenvolvido como atividade acadêmica da disciplina de Ciência de Dados. O objetivo é aplicar as etapas fundamentais de um projeto de dados reais:

- Tipificação e otimização de memória
- Tratamento de valores ausentes
- Análise exploratória (EDA)
- Regressão Linear Simples e Múltipla
- Exportação otimizada em Parquet

---

## Estrutura do Repositório

```
nfl-regressao-linear-2009-2017
 ┣ nfl_regressao_linear.ipynb   ← notebook principal
 ┗ README.md                    ← este arquivo
```

---

## Dataset

**NFL Play by Play 2009–2017 (v4)**  
Fonte: [Kaggle — maxhorowitz/nflplaybyplay2009to2016](https://www.kaggle.com/datasets/maxhorowitz/nflplaybyplay2009to2016)

| Característica | Valor |
|---|---|
| Linhas | 407.688 jogadas |
| Colunas | 102 variáveis |
| Período | Temporadas 2009 a 2017 |
| Formato original | CSV |

O dataset é carregado diretamente via `kagglehub` — não é necessário baixar nada manualmente.

---

## Estrutura do Notebook

| Seção | Conteúdo |
|---|---|
| 1. Importações e Carregamento | Bibliotecas e carregamento via kagglehub |
| 2. Tipificação | Conversão para `datetime`, `category`, `Int8`, `Int16`, `float32` |
| 3. Limpeza | Tratamento de nulos, remoção de colunas irrelevantes, filtro de jogadas |
| 4. Variáveis | Identificação e justificativa das variáveis dependente e independentes |
| 5. EDA | Distribuições, correlações, análise por down e tipo de jogada |
| 6. Regressão Simples | `ydstogo → Yards.Gained` com métricas e visualização |
| 7. Regressão Múltipla | 5 variáveis, StandardScaler, comparação com modelo simples |
| 8. Conclusões | Interpretação dos resultados e limitações do modelo |
| 9. Desafio Extra | Exportação otimizada em Parquet com Brotli |

---

## Modelagem

### Variável Dependente
**`Yards.Gained`** — jardas ganhas por jogada. Métrica central do futebol americano.

### Regressão Linear Simples
| Variável | Justificativa |
|---|---|
| `ydstogo` | Jardas para o próximo first down — quanto mais distância, mais a equipe arrisca jogadas longas |

### Regressão Linear Múltipla
| Variável | Justificativa |
|---|---|
| `ydstogo` | Distância para o próximo first down |
| `down` | Número da tentativa — define o nível de risco da jogada |
| `qtr` | Período do jogo — comportamento muda no fim do jogo |
| `ScoreDiff` | Diferença de placar — times perdendo jogam mais aberto |
| `AirYards` | Jardas da bola no ar — preditor mais direto de jardas ganhas |

---

## Otimização de Memória

A tipificação adequada reduziu significativamente o uso de memória:

| Tipo original | Tipo otimizado | Economia |
|---|---|---|
| `object` (strings) | `category` | ~90% por coluna |
| `float64` | `float32` | 50% |
| `int64` | `Int8` / `Int16` | 75–87% |

---

## Desafio Extra — Parquet com Brotli

O dataset foi exportado em formato Parquet com compressão Brotli, combinando três otimizações simultâneas:

1. **Formato colunar** — agrupa dados similares, facilitando compressão
2. **Downcasting de tipos** — menos bytes por valor
3. **Algoritmo Brotli** — melhor taxa de compressão sem perda de dados

Todos os dtypes são preservados na exportação e verificados na releitura.

---

## Como Executar

### Pré-requisitos

```bash
pip install pandas numpy matplotlib seaborn scikit-learn kagglehub pyarrow
```

### Execução

1. Abra o `nfl_regressao_linear.ipynb` no Jupyter, Kaggle ou VS Code
2. Execute **Kernel → Restart & Run All**
3. O dataset é baixado automaticamente via `kagglehub`

> É necessário ter uma conta no Kaggle e o token configurado para usar o `kagglehub`.

---

## Dependências

| Biblioteca | Uso |
|---|---|
| `pandas` | Manipulação de dados |
| `numpy` | Operações numéricas |
| `matplotlib` | Visualizações |
| `seaborn` | Visualizações estatísticas |
| `scikit-learn` | Modelagem e métricas |
| `kagglehub` | Download do dataset |
| `pyarrow` | Exportação Parquet |

---

## Observações sobre o R²

O R² baixo nos modelos é **esperado e contextualmente justificado**. Jardas ganhas em uma jogada individual dependem de fatores não capturáveis numericamente: leitura de defesa, habilidade individual, condições físicas e variabilidade inerente ao esporte. O modelo captura tendências táticas populacionais, não o resultado de cada jogada específica.

---

## Autor

Desenvolvido por **Sandro Viviani Filho** — Ciência de Dados, 2025
