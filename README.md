# HealthAssist API

Projeto de triagem médica e agendamentos, desenvolvido como parte da disciplina da faculdade. Este repositório contém a entrega do TP1 (Parte 1): base do dataset com EDA inicial e a API FastAPI com autenticação JWT.

## Objetivo do projeto

Construir uma API que receba relatos de sintomas de um paciente e, futuramente, classifique o tipo de atendimento/urgência necessário (triagem), usando como base um modelo treinado a partir de um dataset de perguntas médicas.

## Estrutura de pastas

```
healthassist-api/
├── app/
│   ├── __init__.py
│   ├── main.py                # entry point da API
│   ├── config.py              # configuração via variáveis de ambiente
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── health.py          # GET /health
│   │   ├── auth.py            # POST /auth/token
│   │   └── predict.py         # POST /predict
│   ├── security/
│   │   ├── __init__.py
│   │   └── jwt_handler.py     # criação e validação de JWT
│   └── models/
│       ├── __init__.py
│       └── schemas.py         # modelos Pydantic
├
│   ├── eda_medpt.py           # script de EDA
│   ├── medpt_amostra.csv      # amostra do dataset (5.000 linhas)
│   ├── eda_report.md          # relatório de EDA com gráficos e hipóteses
│   ├── grafico_categorias.png
│   ├── grafico_tamanho_perguntas.png
│   └── grafico_boxplot_categoria.png
├
│── dfd_cia.md             # DFD (Mermaid) + análise CIA
│── dfd.svg                # DFD em SVG (editável no draw.io)
├── .env                       # variáveis de ambiente (não versionar)
├── requirements.txt
└── README.md
```

## Instalação

1. Clone o repositório e entre na pasta:
   ```
   git clone <url-do-repositorio>
   cd healthassist-api
   ```
2. Crie um ambiente virtual (recomendado):
   ```
   python -m venv venv
   source venv/bin/activate      # Linux/Mac
   venv\Scripts\activate         # Windows
   ```
3. Instale as dependências:
   ```
   pip install -r requirements.txt
   ```
4. Crie um arquivo `.env` na raiz do projeto com:
   ```
   SECRET_KEY=troque-por-uma-chave-secreta-forte
   ALGORITHM=HS256
   TOKEN_EXPIRATION_HOURS=1
   DEBUG=True
   ```

## Execução

### Rodar a API

```
uvicorn app.main:app --reload
```

A API sobe em `http://127.0.0.1:8000`.

Rotas disponíveis:
- `GET /health` — verifica se a API está no ar (sem autenticação).
- `POST /auth/token` — envia `username` e `password`, recebe um token JWT.
- `POST /predict` — envia a lista de sintomas no corpo da requisição com o header `Authorization: Bearer <token>`, recebe uma predição (placeholder por enquanto).

### Rodar a EDA

```
cd eda
python eda_medpt.py
```

Isso gera os 3 gráficos (`grafico_categorias.png`, `grafico_tamanho_perguntas.png`, `grafico_boxplot_categoria.png`) e imprime no terminal o shape, dtypes, valores ausentes, duplicatas e distribuição das categorias. O relatório completo com as hipóteses está em `eda_report.md`.

## Escolha do dataset

- **Nome:** MedPT (AKCIT/MedPT)
- **Fonte:** https://huggingface.co/datasets/AKCIT/MedPT — associado ao paper "MedPT: A Massive Medical Question Answering Dataset for Brazilian-Portuguese Speakers" (LREC 2026)
- **Licença:** Creative Commons Attribution 4.0 International (CC BY 4.0).
- **Justificativa:** Dataset de interações reais entre pacientes e médicos, em português, com 384 mil perguntas de pacientes já classificadas por intenção (`question_type`, 7 categorias), o que se encaixa diretamente na proposta de triagem do HealthAssist. Foi selecionada uma amostra aleatória de 5.000 registros utilizando sample(n=5000, random_state=42), mantendo a reprodutibilidade da análise. O MedPT foi escolhido por representar interações entre pacientes e profissionais de saúde, contexto diretamente relacionado ao domínio do HealthAssist API. A coluna question representa a solicitação do usuário e question_type representa sua intenção semântica.
Detalhes completos da EDA (gráficos e hipóteses) estão em [`eda_report.md`](eda_report.md).

## Modelagem de ameaças (DFD + CIA)

O diagrama de fluxo de dados e a análise da tríade CIA (Confidencialidade, Integridade, Disponibilidade) estão em [`dfd_cia.md`](dfd_cia.md), com uma versão editável em [`dfd.svg`](dfd.svg).
