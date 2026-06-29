# Log Classifier — Classificação de Logs Maliciosos com Modelos Interpretáveis

Repositório do trabalho final da disciplina Reconhecimento de Padrões na Universidade Tecnológica Federal do Paraná (UTFPR), Campus Campo Mourão.

## Sobre o projeto

Este trabalho investiga a viabilidade do uso de classificadores *white-box* — **Árvore de Decisão**, **Regressão Logística** e **KNN** — para detecção de ataques de amplificação DNS (DrDoS_DNS), utilizando o conjunto de dados [CIC-DDoS2019](https://www.unb.ca/cic/datasets/ddos-2019.html).

O objetivo central é demonstrar que modelos intrinsecamente interpretáveis, aliados a um pipeline estruturado de pré-processamento, são capazes de alcançar desempenho competitivo sem abrir mão da transparência necessária para auditoria e resposta a incidentes em ambientes de cibersegurança.

## Pipeline implementado

```
Dados brutos (5.074.413 instâncias)
        │
        ▼
Remoção de duplicatas e atributos constantes
        │
        ▼
Tratamento de valores ausentes e infinitos
        │
        ▼
Split estratificado treino/teste (80/20)
        │
        ▼
RandomUnderSampler (apenas no treino)
        │
        ▼
Seleção de features por Informação Mútua (limiar MI > 0,5)
        │
        ▼
Normalização via StandardScaler (LR e KNN)
        │
        ▼
GridSearchCV com K-Fold Estratificado (k=10)
        │
        ▼
Avaliação: F1-Score, FP, FN, teste t pareado, McNemar
```

## Principais resultados

| Modelo | Configuração | F1-Score | FN | FP |
|--------|-------------|---------|-----|-----|
| Regressão Logística | Baseline | 0,76 | 334 | 131 |
| Regressão Logística | Otimizado | 0,72 | 4 | 1.299 |
| KNN | Baseline | 0,97 | 56 | 23 |
| KNN | Otimizado | 0,90 | 0 | 279 |
| Árvore de Decisão | Baseline | 0,99 | 12 | 5 |
| Árvore de Decisão | Otimizado | 0,91 | 0 | 266 |

**Interpretabilidade:** apenas 2 das 11 features selecionadas (`Flow Bytes/s` e `Min Packet Length`) são responsáveis por 96,4% das decisões da Árvore de Decisão otimizada — ambas com interpretação física direta no contexto de amplificação DNS.

## Estrutura do repositório

```
log-classifier/
├── metodo.ipynb          # Notebook principal com todo o pipeline
├── requirements.txt      # Dependências do projeto
├── data/
│   ├── DrDoS_DNS.csv     # Dataset original (não versionado)
│   ├── attack_uniq.csv   # Instâncias únicas de ataque
│   ├── benign.csv        # Instâncias benignas
│   ├── final.csv         # Base final para treino/teste
│   └── validate.csv      # Base de validação externa
└── figures/              # Gráficos gerados
```

> **Nota:** os arquivos CSV não estão versionados por conta do tamanho. O dataset original está disponível em [https://www.unb.ca/cic/datasets/ddos-2019.html](https://www.unb.ca/cic/datasets/ddos-2019.html). Utilize o subconjunto `DrDoS_DNS.csv` do segundo dia de captura.

## Como reproduzir

**1. Clone o repositório**
```bash
git clone https://github.com/FerDelbo/log-classifier.git
cd log-classifier
```

**2. Crie um ambiente virtual e instale as dependências**
```bash
python -m venv env
source env/bin/activate        # Linux/macOS
env\Scripts\activate           # Windows

pip install -r requirements.txt
```

**3. Baixe o dataset**

Acesse [https://www.unb.ca/cic/datasets/ddos-2019.html](https://www.unb.ca/cic/datasets/ddos-2019.html), faça o download do arquivo `DrDoS_DNS.csv` e coloque-o na pasta `data/`.

**4. Execute o notebook**
```bash
jupyter notebook metodo.ipynb
```

Execute as células em ordem. O notebook está organizado nas seguintes seções:
- **Pré-processamento** — limpeza, remoção de duplicatas, balanceamento
- **Seleção de features** — Informação Mútua
- **Baseline** — modelos com parâmetros default e 76 features
- **Método proposto** — GridSearchCV com 11 features selecionadas
- **Interpretabilidade** — análise de complexidade, estabilidade e concentração

## Requisitos

- Python 3.9+
- Veja `requirements.txt` para as dependências completas

## Referência

Se utilizar este código, por favor cite:

```
Delbone, F. R.; Basseto, L. B. Classificação de logs maliciosos utilizando
técnicas de aprendizado de máquina interpretáveis. UTFPR, Campo Mourão, 2026.
```

## Autores

- **Fernando Roberto Delbone** — [fernandodelbone@alunos.utfpr.edu.br](mailto:fernandodelbone@alunos.utfpr.edu.br)
- **Luiza Batista Basseto** — [luizabatista@alunos.utfpr.edu.br](mailto:luizabatista@alunos.utfpr.edu.br)

Universidade Tecnológica Federal do Paraná (UTFPR) — Campus Campo Mourão
