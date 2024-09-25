# Tech Challenge - Fase 3 (Grupo 40)

**FIAP - Pos Tech - IA para DEVs**

**Alunos:**

- Claiton Aparecido Pereira - RM355839
- Eduardo Pedrosa Cajueiro - RM355819
- Hiomone Oliveira Rodrigues - RM356011
- Leonardo Lima Ferreira - RM355721
- Thiago Silva Cerqueira - RM355830

# **Processo de Fine-Tuning**

## **Definição do problema**

Temos um grande dataset (The AmazonTitles-1.3MM), que consiste em consultas textuais reais de usuários e títulos associados de produtos relevantes encontrados na Amazon e suas descrições.

O desafio consiste em executar o fine-tuning de um foundation model utilizando este dataset (arquivo `trn.json`). Após o treinamento, executar um prompt de usuário onde dada a pergunta sobre o título do produto (campo "title" do dataset), retornar o resultado do aprendizado do modelo no contexto do campo de descrição do produto (campo "content" do dataset).

## **Fases da solução do problema**

As etapas para solução do problema proposto são descritas a seguir.

Cada etapa foi executada em um notebook separado no Google Colab. Essa abordagem facilita o entendimento e mantém o código organizado. Dentro de cada notebook, podem ter sido incluídos comentários e textos de documentação específicos, que detalham as particularidades de cada fase dos algoritmos implementados.

## 1. **Pré-processamento de Dataset (`preprocess_dataset.ipynb`)**

- **Entrada**: Lê um arquivo JSON (`trn.json`) que está localizado em um dataset de treinamento que criamos no Hugging Face (`thiagoscerqueira/amazontitlestrn`) e corresponde ao dataset "1.3 MM" da Amazon. Este dataset consiste em consultas textuais reais de usuários e títulos associados de produtos relevantes encontrados na Amazon e suas descrições.

- **Processamento**:

  - Limpa e processa os dados: aqui eliminamos as linhas sem relevância para o treinamento, como por exemplo, linhas com descrição vazia. Também consideramos apenas os campos do json necessários para o treinamento ("title" e "content"), desprezando os demais;
  - Converte o arquivo JSON de entrada para um arquivo no formato CSV estruturado, facilitando a manipulação e com tamanho menor comparado a um arquivo com estrutura JSON;
  - Com estas ações, conseguimos reduzir o tamanho do arquivo de treinamento em praticamente 50% (de 1,75Gb para 906Mb).

- **Saída**: É gerado um arquivo CSV (`transformed_dataset.csv`) que é armazenado via upload no mesmo dataset de treinamento do Hugging Face para ser utilizado na fase de fine-tuning.

## 2. **Chamada do foundation model antes do treinamento (`model_output_pretraining.ipynb`)**

Aqui fazemos uma chamada ao foundation model, antes do treinamento, para que possamos comparar o output gerado pelo modelo antes e depois do treinamento.

## 3. **Fine-Tuning do Modelo LLaMA (`finetuning.ipynb`)**

- **Entrada**: Utiliza o dataset CSV pré-processado gerado na etapa 1;
- **Modelo**: Carrega o modelo base LLaMA.
- **Fine-Tuning**:
  - Modifica a arquitetura do modelo para a tarefa específica (ex: classificação, geração de texto).
  - Treina o modelo utilizando os dados processados com hiperparâmetros especificados (taxa de aprendizado, tamanho do lote, etc.).
- **Saída**: Um modelo ajustado pronto para inferência.

## 4. **Chamada do foundation model após o treinamento (`trained_model_output.ipynb`)**

Aqui fazemos algumas chamadas ao modelo após o treinamento, para comparar com os outputs gerados antes do treinamento.

## Visão Geral dos Arquivos:

- `trn.json`: Dataset bruto.
- `transformed_dataset.csv`: Dataset csv limpo e pré-processado.
- `preprocess_dataset.ipynb`: Script de pré-processamento.
- `model_output_pretraining.ipynb`: Script para geração de outputs antes do treinamento.
- `trained_model_output.ipynb`: Script para geração de outputs do modelo treinado (após o treinamento).
