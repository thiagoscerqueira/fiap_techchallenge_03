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

- **Entrada**: Lê um arquivo JSON (`trn.json`) que está localizado em um dataset de treinamento que criamos no Hugging Face Hub (`thiagoscerqueira/amazontitlestrn`) e corresponde ao dataset "1.3 MM" da Amazon. Este dataset consiste em consultas textuais reais de usuários e títulos associados de produtos relevantes encontrados na Amazon e suas descrições.

- **Processamento**:

  - Limpa e processa os dados: aqui eliminamos as linhas sem relevância para o treinamento, como por exemplo, linhas com descrição vazia. Também consideramos apenas os campos do json necessários para o treinamento ("title" e "content"), desprezando os demais;
  - Converte o arquivo JSON de entrada para um arquivo no formato CSV estruturado, facilitando a manipulação e com tamanho menor comparado a um arquivo com estrutura JSON;
  - Com estas ações, conseguimos reduzir o tamanho do arquivo de treinamento em praticamente 50% (de 1,75Gb para 906Mb).

- **Saída**: É gerado um arquivo CSV (`transformed_dataset.csv`) que é armazenado via upload no mesmo dataset de treinamento do Hugging Face Hub para ser utilizado na fase de fine-tuning.

## 2. **Chamada do foundation model antes do treinamento (`model_output_pretraining.ipynb`)**

Aqui fazemos uma chamada ao foundation model, antes do treinamento, para que possamos comparar o output gerado pelo modelo antes e depois do treinamento.

## 3. **Fine-Tuning do Modelo (LLaMA) (`finetuning.ipynb`)**

Para o processo de fine-tuning, escolhemos o Llama foundation model. O Llama oferece algumas vantagens para este processo, as quais podemos destacar:

- É um modelo acessível e open-source, com isto o custo para aplicar o processo de fine-tuning no contexto acadêmico é reduzido;
- Reconhecido por oferecer uma excelente performance;
- Sua flexibilidade permite aplicar técnicas como Low-Rank Adaptation (LoRA), que reduz consumo de memória e auxilia no desempenho geral do treinamento, com menor custo e consumo de GPUs.

Neste processo, também foi utilizado o "unsloth", uma biblioteca que oferece otimização e proporciona maior eficiência ao processo de fine-tuning. Podemos por exemplo carregar o modelo em precisão de 4 bits, com otimização de memória.

Também utilizamos algumas bibilotecas do Hugging Face, como transformers, datasets, tokenizers, etc, que abstraem complexidade e auxiliam bastante na execução de tarefas comuns ao processo de fine-tuning. Além disso, armazenamos os datasets utilizados e gerados, bem como o modelo tunado, no "Hugging Face Hub", que é uma plataforma pública de hospedagem e compartilhamento de recursos de machine learning.

### **Visão geral**

- **Entrada**: Utilizamos o dataset CSV (`transformed_dataset.csv`) pré-processado gerado na etapa 1 e armazenado no Hugging Face Hub. Como o arquivo tem 1,3mi+ de linhas, optamos por carregar um subconjunto das primeiras 100 mil linhas do dataset para treinamento, um número no qual julgamos suficiente para exercitar os conceitos;
- **Modelo**: Carregamos o modelo base LLaMA;
- **Fine-Tuning**: Aplicamos o fine-tuning do modelo. Para este exemplo acadêmico, configuramos o parâmetro "num_train_epochs" para 1 época, a fim de economizar tempo e recursos. Em um case real, poderíamos incrementar a quantidade de épocas, de forma a aumentar a capacidade de aprendizagem;
- **Saída**: Um modelo ajustado pronto para inferência. Efetuamos o upload do modelo treinado no Hugging Face Hub (`thiagoscerqueira/amazontitlesmodeltrn`).

## 4. **Chamada do foundation model após o treinamento (`trained_model_output.ipynb`)**

Aqui fazemos algumas chamadas ao modelo após o treinamento, para comparar com os outputs gerados antes do treinamento.

## Visão Geral dos Arquivos:

- `trn.json`: Dataset bruto.
- `transformed_dataset.csv`: Dataset csv limpo e pré-processado.
- `preprocess_dataset.ipynb`: Script de pré-processamento.
- `model_output_pretraining.ipynb`: Script para geração de outputs antes do treinamento.
- `trained_model_output.ipynb`: Script para geração de outputs do modelo treinado (após o treinamento).
