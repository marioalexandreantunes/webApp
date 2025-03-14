# webApp

🔥 integrar FastAPI com React

- 1️⃣ FastAPI como API e React como front-end separado
- ✅ Melhor performance e escalabilidade
- ✅ Front-end e back-end podem ser hospedados separadamente
- ✅ Melhor para desenvolvimento com CI/CD

## Estrutura de pastas 

```
meu_projeto/
│── backend/              # Diretório do FastAPI (Back-end)
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py       # Arquivo principal do FastAPI
│   │   ├── routes/       # Rotas da API
│   │   │   ├── __init__.py
│   │   │   ├── users.py
│   │   │   ├── products.py
│   │   ├── models/       # Modelos de banco de dados
│   │   ├── schemas/      # Schemas Pydantic (validação de dados)
│   │   ├── database.py   # Configuração do banco de dados
│   ├── venv/             # Ambiente virtual do Python
│   ├── requirements.txt  # Dependências do FastAPI
│   ├── .env              # Configurações sensíveis (ex: DB_URL)
│
│── frontend/             # Diretório do React (Front-end)
│   ├── public/
│   ├── src/
│   │   ├── components/   # Componentes React
│   │   ├── pages/        # Páginas do app
│   │   ├── services/     # Chamadas à API FastAPI
│   │   │   ├── api.js
│   │   ├── App.js
│   │   ├── index.js
│   ├── package.json      # Dependências do React
│   ├── .env              # Configurações (ex: URL da API)
│
│── docker-compose.yml    # Se for usar Docker
│── README.md             # Documentação do projeto
```
