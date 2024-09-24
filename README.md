# Processo de Fine-Tuning Usando o Modelo LLaMA

Este projeto consiste em duas fases principais: o pré-processamento de dados e o fine-tuning de um modelo pré-treinado LLaMA para uma tarefa específica.

## 1. **Pré-processamento de Dataset (`preprocess_dataset.ipynb`)**

- **Entrada**: Lê um arquivo JSON (`trn.json`) contendo o dataset.
- **Processamento**:
  - Limpa e processa os dados.
  - Converte o arquivo JSON para o formato CSV estruturado, facilitando a manipulação.
  - Prepara os dados para o processo de fine-tuning.
- **Saída**: Gera um arquivo CSV utilizado na fase de fine-tuning.

## 2. **Fine-Tuning do Modelo LLaMA (`finetuning.ipynb`)**

- **Entrada**: Utiliza o dataset CSV pré-processado.
- **Modelo**: Carrega o modelo base LLaMA.
- **Fine-Tuning**:
  - Modifica a arquitetura do modelo para a tarefa específica (ex: classificação, geração de texto).
  - Treina o modelo utilizando os dados processados com hiperparâmetros especificados (taxa de aprendizado, tamanho do lote, etc.).
- **Saída**: Um modelo ajustado pronto para inferência.

## Visão Geral dos Arquivos:

- `trn.json`: Dataset bruto.
- `preprocess_dataset.ipynb`: Script de pré-processamento.
- `finetuning.ipynb`: Script de fine-tuning.

## Requisitos:

- Modelo LLaMA (via Hugging Face, etc.).
- Bibliotecas Python: Transformers, Pandas, PyTorch/TensorFlow.

## Como Executar:

1. **Pré-processamento**: Execute o arquivo `preprocess_dataset.ipynb` para gerar o CSV.
2. **Fine-Tuning**: Use o arquivo `finetuning.ipynb` para ajustar o modelo LLaMA usando o CSV processado.
