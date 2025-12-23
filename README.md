# Avaliação Automática de Redações em Português: Combinando Hand-Crafted Features e Transformers

Este repositório contém o código e a metodologia para o desenvolvimento de um sistema de **Avaliação Automática de Redações (AAR)** focado no contexto do ENEM. O projeto utiliza uma abordagem híbrida, unindo extração de características linguísticas manuais com modelos de deep learning.

## 📌 Resumo do Projeto

A correção de grandes exames como o ENEM exige ferramentas que possam escalar a avaliação mantendo a precisão. Este trabalho propõe um modelo que avalia as redações por competência (C1 a C5) através de:
1.  **Hand-crafted Features:** Métricas clássicas de coesão, gramática e léxico.
2.  **Transformers:** Representações semânticas profundas utilizando o modelo **BERTimbau**.
3.  **Regressão Ordinal:** Uso do framework **CORAL** para garantir que o modelo respeite a hierarquia natural das notas (0 a 200).

O modelo híbrido alcançou um **QWK médio de 0,42**, demonstrando ser superior ao uso de características isoladas e competitivo com o estado da arte para o português brasileiro.

## 🛠️ Ferramentas e Tecnologias

### Linguagem e Ambiente
* **Python 3.x**: Linguagem principal.
* **Google Colab**: Ambiente de desenvolvimento (utilizando GPU T4).

### Frameworks e Bibliotecas
* **PyTorch & PyTorch Lightning**: Construção e gerenciamento do ciclo de vida das redes neurais.
* **Hugging Face (Transformers)**: Implementação do modelo BERTimbau.
* **Coral_pytorch**: Implementação da camada de regressão ordinal.
* **Scikit-learn**: Pré-processamento, normalização (`StandardScaler`) e seleção de características.
* **Pandas & NumPy**: Manipulação de dados e operações matriciais.
* **NILC-Metrix**: Extração de métricas de legibilidade e complexidade.

## 📊 Base de Dados

Utilizou-se o dataset **AES_ENEM** (Silveira et al., 2024), focado na "Fonte A".
* **Total de Redações:** 1.165 textos.
* **Divisão:** Treino (65%), Validação (17%) e Teste (18%).
* **Escala de Notas:** 0 a 200 pontos por competência (distribuição ordinal).

## 🚀 Como Replicar este Projeto

### 1. Preparação dos Dados
Extraia as características das redações utilizando as três frentes:
* **NILC-Metrix:** 200 características (coesão, sintaxe, etc).
* **Métricas de Neves:** 27 características (gramática e semântica).
* **TF-IDF:** 1.000 termos mais relevantes (via `SelectKBest`).

### 2. Geração de Embeddings
Utilize o **BERTimbau Base**. Para redações longas (>512 tokens), aplique a técnica de **segmentação com stride (sobreposição) de 256 tokens** e extraia a média do token `[CLS]` de cada segmento.

### 3. Treinamento do Modelo
O modelo consiste em um Perceptron Multicamadas (MLP) integrado ao CORAL:
* **Arquitetura:** Camadas de 256, 128 e 64 neurônios.
* **Configuração:** Batch Size 128, Learning Rate 0,01.
* **Regularização:** Early Stopping (50 épocas) monitorando o QWK.

### 4. Execução
O treinamento deve ser realizado de forma independente para cada uma das 5 competências do ENEM.

## 📈 Resultados Principais
* **Melhor Desempenho:** Modelo Híbrido (Embedding + Neves).
* **Destaque:** Competência 5 (Proposta de Intervenção) obteve o maior pico de concordância (QWK 0,53) em redações de alto consenso humano.

---
**Desenvolvido por [Seu Nome] - 2025**
