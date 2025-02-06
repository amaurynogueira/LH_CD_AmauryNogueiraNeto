# LH_CD_AmauryNogueiraNeto - Projeto de Precificação de Aluguéis Temporários - NYC

## Introdução  
Este projeto foi desenvolvido como parte do desafio para Cientista de Dados da Indicium. O objetivo é testar conhecimentos em resolução de problemas de negócios, análise de dados e aplicação de modelos preditivos. Nele, foi construída uma estratégia para previsão de preços de aluguéis temporários na cidade de Nova York, que abrange desde uma análise exploratória dos dados do maior concorrente até a validação e explicação do modelo preditivo. **Observação:** Não existe resposta certa – o que importa é a capacidade de descrever e justificar os passos utilizados na resolução do problema.

## Desafio  
O desafio consiste em:  
1. Realizar uma Análise Exploratória de Dados (EDA) demonstrando as principais características entre as variáveis e formulando hipóteses de negócio.  
2. Responder às seguintes perguntas:  
   - **a.** Supondo que uma pessoa esteja pensando em investir em um apartamento para alugar na plataforma, onde seria mais indicada a compra?  
   - **b.** O número mínimo de noites e a disponibilidade ao longo do ano interferem no preço?  
   - **c.** Existe algum padrão no texto do nome do local para lugares de mais alto valor?  
3. Explicar como foi feita a previsão do preço a partir dos dados, detalhando quais variáveis e transformações foram utilizadas e por que; o tipo de problema (regressão ou classificação); o modelo escolhido com seus prós e contras; e a métrica de performance adotada.  
4. Fornecer uma sugestão de preço para um apartamento com características pré-definidas.  
5. Salvar o modelo desenvolvido no formato `.pkl`.  
6. Disponibilizar a entrega por meio de um repositório público contendo:  
   - Um README com instruções de instalação e execução;  
   - Um arquivo de requisitos com todos os pacotes utilizados;  
   - Relatórios das análises e EDA (em PDF, Jupyter Notebook ou similar);  
   - Os códigos de modelagem (podendo estar no mesmo notebook);  
   - O arquivo do modelo `.pkl`.  
7. Criar um vídeo curto explicando o desenvolvimento e a execução das atividades.  

## Dicionário dos Dados  
O dataset de treinamento contém 16 colunas, conforme descrito abaixo:  
- **id:** Chave exclusiva para cada anúncio.  
- **nome:** Nome do anúncio.  
- **host_id:** ID do usuário que hospedou o anúncio.  
- **host_name:** Nome do usuário.  
- **bairro_group:** Nome do bairro onde o anúncio está localizado.  
- **bairro:** Nome da área do anúncio.  
- **latitude:** Latitude do local.  
- **longitude:** Longitude do local.  
- **room_type:** Tipo de espaço do anúncio.  
- **price:** Preço por noite em dólares listado pelo anfitrião.  
- **minimo_noites:** Número mínimo de noites que o usuário deve reservar.  
- **numero_de_reviews:** Número de comentários dados a cada listagem.  
- **ultima_review:** Data da última revisão do anúncio.  
- **reviews_por_mes:** Número de avaliações fornecidas por mês.  
- **calculado_host_listings_count:** Quantidade de listagens por host.  
- **disponibilidade_365:** Número de dias em que o anúncio está disponível para reserva.

## Como o Projeto Foi Feito  
1. **Coleta e Carregamento dos Dados:**  
   O dataset foi obtido a partir dos dados de um concorrente e carregado utilizando a biblioteca Pandas.

2. **Análise Exploratória de Dados (EDA):**  
   Foram realizadas análises descritivas, verificação de valores ausentes e geração de diversos gráficos, tais como:  
   - Distribuição de preços;  
   - Distribuição do número mínimo de noites;  
   - Distribuição do número de reviews;  
   - Distribuição da disponibilidade anual;  
   - Matriz de correlação;  
   - Análise geoespacial dos imóveis.

3. **Extração de Insights e Hipóteses de Negócio:**  
   A partir dos gráficos, foram formuladas hipóteses, por exemplo:  
   - **Investimento:** Embora bairros centrais como Manhattan apresentem preços elevados, áreas emergentes (ex.: certas regiões de Brooklyn) podem oferecer um melhor custo-benefício.  
   - **Políticas de Reserva:** Imóveis com número mínimo de noites baixo tendem a ter preços mais competitivos, enquanto a alta disponibilidade pode refletir estratégias diferenciadas de precificação.  
   - **Análise Textual:** Anúncios que incluem termos como “luxury”, “designer” e “central” estão frequentemente associados a preços mais altos.

4. **Desenvolvimento do Pipeline de Modelagem:**  
   O pipeline integra:  
   - **Variáveis Numéricas:** Latitude, longitude e mínimo de noites, padronizadas com StandardScaler.  
   - **Variáveis Categóricas:** Bairro_group e room_type, transformadas com TargetEncoder.  
   - **Variável Textual:** Nome do anúncio, processada com TfidfVectorizer.  
   O modelo preditivo escolhido foi o **LGBMRegressor**, que se mostrou adequado para resolver o problema de regressão.

5. **Validação do Modelo:**  
   Foi utilizada a técnica de validação cruzada com StratifiedKFold (utilizando a variável `price_strata`) para manter a distribuição dos preços nos conjuntos de treino e teste. As métricas de avaliação escolhidas foram MAPE, R² e RMSE, com ênfase no MAPE por fornecer uma interpretação percentual dos erros.

6. **Geração do Relatório PDF:**  
   Um relatório completo foi gerado utilizando a biblioteca FPDF, contendo todas as análises, gráficos, insights e justificativas das escolhas metodológicas.

7. **Respostas às Perguntas Estratégicas:**  
   Foram respondidas as perguntas do desafio, detalhando:  
   - Onde investir em um imóvel para aluguel;  
   - A influência do número mínimo de noites e da disponibilidade no preço;  
   - Padrões textuais associados a imóveis de alto valor.

8. **Explicação da Previsão do Preço:**  
   A previsão do preço foi realizada através de um pipeline que integra variáveis numéricas, categóricas e textuais. As transformações aplicadas garantem que cada tipo de variável contribua de forma adequada para a predição.  
   - **Problema:** Regressão, pois o objetivo é prever um valor contínuo (o preço).  
   - **Modelo:** LGBMRegressor, que possui vantagens como rapidez e eficiência, mas pode ser sensível a hiperparâmetros e propenso a overfitting se não for devidamente validado.  
   - **Métrica:** MAPE, escolhida por permitir uma interpretação percentual dos erros, facilitando a compreensão da performance do modelo.

## Como Utilizar o Projeto

### Pré-requisitos  
- Python 3.x instalado.  
- Ambiente de execução: Jupyter Notebook, Google Colab ou similar.  
- Dependências instaladas (preferencialmente via arquivo `requirements.txt`).

### Instalação  
1. **Clone o repositório:**  
   ```bash
   git clone https://github.com/seu_usuario/projeto-precificacao-alugueis-nyc.git

2. **Navegue até a pasta do projeto:**  
	cd projeto-precificacao-alugueis-nyc


3.	**Instale as dependências:**
	pip install -r requirements.txt



*Execução*
- Abra o Jupyter Notebook ou Google Colab e execute todas as células para:
- Carregar e analisar os dados;
- Gerar os gráficos e realizar a modelagem preditiva;
- Criar o relatório PDF com todas as análises e insights.
- O relatório PDF será salvo no diretório especificado no código.

**Estrutura do Projeto**
- notebook.ipynb: Notebook principal contendo o código e as análises.
- Relatórios/: Pasta onde o relatório PDF gerado é salvo.
- Dados/: Pasta com o dataset utilizado.
- Modelo/: Pasta para salvar o modelo preditivo (.pkl).

**Considerações Finais**

Este projeto demonstra a integração de técnicas de análise de dados, modelagem preditiva e visualização para apoiar a tomada de decisões estratégicas no mercado de aluguéis temporários. A abordagem adotada – que inclui a extração de hipóteses de negócio, a validação robusta do modelo e a explicação detalhada da previsão de preços – pode ser expandida para outros mercados e conjuntos de dados, servindo como base para projetos futuros na área de ciência de dados aplicada a negócios.

Referências
 - scikit-learn. User Guide. Disponível em: https://scikit-learn.org/stable/user_guide.html
 - LightGBM. Documentation. Disponível em: https://lightgbm.readthedocs.io/
 - FPDF. Documentation. Disponível em: https://pyfpdf.readthedocs.io/

