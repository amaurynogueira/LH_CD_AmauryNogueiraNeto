
# Projeto de Precificação de Aluguéis Temporários - NYC

## Introdução
Este projeto foi desenvolvido no âmbito do desafio para Cientista de Dados da Indicium e demonstra a aplicação de técnicas avançadas de análise de dados, engenharia de features e modelagem preditiva. O objetivo principal é prever os preços de aluguéis temporários em Nova York utilizando dados reais, integrando análises exploratórias profundas, pipelines robustos e estratégias de otimização que vão além dos requisitos iniciais do desafio.

---

## Desafio e Escopo do Projeto

### Requisitos Originais
- **Análise Exploratória de Dados (EDA):**  
  Investigar características principais dos dados, identificar padrões e formular hipóteses de negócio.
- **Respostas Estratégicas:**  
  - Determinar locais para investimento em imóveis
  - Analisar influência de variáveis operacionais (mínimo de noites, disponibilidade)
  - Detectar padrões textuais indicativos de imóveis de alto valor
- **Modelagem Preditiva:**  
  Desenvolver pipeline para previsão de preços com detalhamento de:
  - Variáveis selecionadas
  - Transformações aplicadas
  - Escolha do modelo
  - Métricas de avaliação (MAPE como principal)
- **Entrega:**  
  Código, relatório em PDF, arquivo de requisitos e modelo final em `.pkl`.

### Funcionalidades Adicionais Implementadas
- **Integração de Dados:**  
  Uso opcional do dataset `train.csv` do Huggingface Hub para ampliar a análise.
- **Engenharia de Features Avançada:**  
  - Criação de variáveis derivadas (`tempo_atividade`, `log_price`)
  - Processamento de texto com TF-IDF para a coluna `nome`
- **Pipeline Modular:**  
  - Pré-processamento integrado (StandardScaler, TargetEncoder, TfidfVectorizer)
  - Comparação de 3 modelos (LightGBM, RandomForest, XGBoost)
  - Otimização de hiperparâmetros via `RandomizedSearchCV`
- **Aceleração com GPU:**  
  Implementação de fallback automático (GPU → CPU) para LightGBM/XGBoost.
- **Relatório Automatizado:**  
  Geração de PDF com EDA, visualizações e métricas via FPDF.

---

## Descrição dos Dados

### Dataset Principal: `teste_indicium_precificacao.csv`
| Coluna                          | Descrição                                      |
|---------------------------------|-----------------------------------------------|
| `id`                            | Identificador único do anúncio                |
| `nome`                          | Título do anúncio                             |
| `host_id`                       | ID do anfitrião                               |
| `bairro_group`                  | Grupo de bairros (ex: Manhattan)              |
| `latitude`/`longitude`          | Coordenadas geográficas                       |
| `room_type`                     | Tipo de acomodação                            |
| `price`                         | Preço por noite (USD)                         |
| `minimo_noites`                 | Mínimo de noites para reserva                 |
| `disponibilidade_365`           | Dias disponíveis no ano                       |

### Dataset Adicional: `train.csv` (Opcional)
- **Fonte:** Huggingface Hub (*Airbnb In NYC: Room Prices, Reviews, and Availability*)
- **Características:**  
  - 20k+ listagens
  - Colunas compatíveis com o dataset principal após renomeação
  - Atributos adicionais de reviews e disponibilidade



## Metodologia e Funcionalidades do Código

### 1. Pré-processamento de Dados
```python
# Exemplo de tratamento de outliers
Q1 = df['price'].quantile(0.25)
Q3 = df['price'].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df['price'] < (Q1 - 1.5 * IQR)) | (df['price'] > (Q3 + 1.5 * IQR))]
```

### 2. Pipeline de Modelagem

**Modelos Comparados:**
1. **LightGBM** (Otimizado com `RandomizedSearchCV`)
2. **RandomForest** (500 estimadores)
3. **XGBoost** (Com suporte a GPU)

### 3. Geração de Relatório
- **Seções do PDF:**  
  - Análise geoespacial (mapas de calor por preço)
  - Distribuição de `room_type` e `bairro_group`
  - Matriz de correlação entre variáveis-chave
  - Performance dos modelos (MAPE, R², RMSE)

---

## Instruções de Uso

### Instalação do Ambiente
```bash
pip install -r requirements.txt
```

### Configuração Inicial
```python
# Google Colab
from google.colab import drive
drive.mount('/content/drive')
file_path = '/content/drive/MyDrive/Dados/teste_indicium_precificacao.csv'
```

### Fluxo de Execução
1. **Pré-processamento:**
   - Carregar datasets
   - Remover outliers
   - Criar features derivadas

2. **Modelagem:**
   ```python
   pipeline.fit(X_train, y_train)
   predictions = pipeline.predict(X_test)
   ```

3. **Geração de Relatório:**
   - Salva em `Relatorios/Relatorio_amaury.pdf`

---

## Estrutura do Projeto
```
projeto-precificacao-alugueis-nyc/
├── notebook.ipynb
├── Dados/
│   ├── teste_indicium_precificacao.csv
│   └── train.csv (opcional)
├── Relatorios/
│   └── Relatorio_amaury.pdf
├── Modelo/
│   └── modelo_final.pkl
├── requirements.txt
└── README.md
```

---

## Considerações Finais
O projeto demonstra uma abordagem profissional para análise de mercado imobiliário, com:
- ✅ Validação cruzada estratificada
- ✅ Comparação sistemática de modelos
- ✅ Documentação automatizada
- ✅ Escalabilidade para novos dados

---

## Referências
- [Documentação do LightGBM](https://lightgbm.readthedocs.io/)
- [Scikit-learn: Pré-processamento e Modelos](https://scikit-learn.org/stable/)
- [FPDF para Geração de Relatórios](http://pyfpdf.readthedocs.io/)
  
