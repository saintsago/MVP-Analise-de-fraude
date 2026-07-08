# MVP - Análise de Fraude

Trabalho de MVP da Sprint de Machine Learning & Analytics (pós-graduação em Ciência de
Dados), baseado no desafio Kaggle **[IEEE-CIS Fraud Detection](https://www.kaggle.com/competitions/ieee-fraud-detection/overview)**.

## Sobre o projeto

O objetivo é prever a probabilidade de fraude (`isFraud`) em transações de cartão
"card-not-present", combinando engenharia de atributos orientada ao domínio (padrões
temporais, velocidade de uso por cartão/endereço, histórico de fraude por
cartão/e-mail/dispositivo) com um ensemble de gradient boosting (XGBoost, LightGBM,
CatBoost) via stacking. O resultado final atinge **ROC-AUC de 0,9031** no conjunto de
teste, contra 0,5010 de um baseline aleatório (`DummyClassifier`).

## Origem dos dados

- **Fonte oficial:** Kaggle — [IEEE-CIS Fraud Detection](https://www.kaggle.com/competitions/ieee-fraud-detection/overview)
- Os arquivos oficiais (`train_transaction.csv`, `train_identity.csv`,
  `test_transaction.csv`, `test_identity.csv`) ultrapassam 500MB cada e não são
  versionados neste repositório (limite de tamanho do GitHub).
- **Mirror público (Google Drive, "qualquer pessoa com o link" pode acessar):**
  https://drive.google.com/drive/folders/1NL-VsVQ-P3VX3ASD-74aiWa8FA_6cVBY
  — mantido exclusivamente para fins de execução acadêmica deste MVP.
- O notebook principal baixa os dados **automaticamente** via [`gdown`](https://github.com/wkentaro/gdown)
  na Seção 3.2 — não é necessário montar um Google Drive pessoal nem fazer upload manual.

## Como executar

1. Abrir `MVP_Analise_de_fraude.ipynb` no Google Colab:
   https://colab.research.google.com/drive/1YJ9O8AWL3AczPNQ4NxCbSuOAmqkQ8M0J
2. Executar todas as células, do início ao fim (`Ambiente de execução > Executar tudo`).
3. A Seção 3.2 baixa os 4 CSVs automaticamente via `gdown` para `./data/` dentro do
   runtime do Colab.
4. **Tempo estimado de execução:** o pipeline treina um `StackingClassifier` (XGBoost +
   LightGBM + CatBoost, com early stopping e validação cruzada de 5 folds) sobre
   ~472 mil transações — pode levar dezenas de minutos em CPU. Recomenda-se manter a aba
   do Colab ativa durante a execução para evitar desconexão por inatividade.

## Estrutura do repositório

- `README.md` — este arquivo.
- `MVP_Analise_de_fraude.ipynb` — **notebook oficial de entrega**, contendo o fluxo
  completo do projeto (as 15 seções exigidas pela disciplina: definição do problema,
  EDA, pré-processamento, modelagem, avaliação e conclusão).
- `EDA.ipynb` e `model.ipynb` — notebooks de desenvolvimento/histórico (exploração e
  modelagem originais, rodados localmente com os CSVs em `./data/`). Mantidos como
  registro do processo, mas **não são o entregável** — para a avaliação, use apenas
  `MVP_Analise_de_fraude.ipynb`.

## Tecnologias usadas

pandas, numpy, scikit-learn, category_encoders, XGBoost, LightGBM, CatBoost, gdown,
matplotlib, seaborn, joblib.

## Principais resultados

| Modelo | ROC-AUC (teste) |
|---|---:|
| Dummy (baseline) | 0,5010 |
| XGBoost | 0,8855 |
| LightGBM | 0,8809 |
| CatBoost | 0,8921 |
| **Stacking (final)** | **0,9031** |

- Taxa de fraude no dataset: **3,50%** das transações (forte desbalanceamento de classes).
- Threshold de decisão otimizado por F2-score (prioriza recall): **0,11**.
