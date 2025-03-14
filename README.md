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

---

# Abordagens diferentes

Além da abordagem tradicional de **FastAPI como backend e React como frontend**, existem outras formas de estruturar o projeto, dependendo dos requisitos e objetivos. Aqui estão algumas opções:

---

## **1️⃣ FastAPI como API + React como Frontend (Arquitetura Separada - RESTful)**
✅ **Melhor para:** Projetos escaláveis, onde o front-end e o back-end podem ser implantados separadamente.  

### 📌 Como funciona:
- O FastAPI expõe uma API RESTful (`/api/`).
- O React consome essa API usando `fetch()` ou `axios`.
- O front-end pode ser implantado no **Vercel/Netlify** e o back-end no **AWS/DigitalOcean/Heroku**.

### 🛠️ **Exemplo de fluxo:**
1. O usuário acessa `http://localhost:3000` (React).
2. O React chama `http://localhost:8000/api/users` (FastAPI).
3. FastAPI responde com JSON e React renderiza os dados.

🔹 **Vantagens:**  
✔ Separação clara de responsabilidades.  
✔ Fácil de escalar e hospedar.  
✔ Melhor organização para times separados.  

🔸 **Desvantagens:**  
❌ Pode exigir configuração de CORS.  
❌ Comunicação pode ter pequena latência devido às requisições entre serviços.  

---

## **2️⃣ FastAPI servindo React como template (Monolito)**
✅ **Melhor para:** Projetos menores ou MVPs rápidos.  

### 📌 Como funciona:
- O FastAPI não apenas fornece a API, mas também **serve o front-end React**.
- O React é **buildado** (`npm run build`) e armazenado em `static/` ou `templates/`.

### 🛠️ **Como fazer:**
1. Criar o build do React:
   ```bash
   npm run build
   ```
2. Copiar os arquivos para dentro do projeto FastAPI (`/static` ou `/frontend/build`).
3. Configurar o FastAPI para servir os arquivos estáticos:
   ```python
   from fastapi import FastAPI
   from fastapi.staticfiles import StaticFiles

   app = FastAPI()

   app.mount("/", StaticFiles(directory="frontend/build", html=True))
   ```
🔹 **Vantagens:**  
✔ Menos servidores para gerenciar.  
✔ Simples de hospedar (apenas um serviço).  

🔸 **Desvantagens:**  
❌ Menos flexível para escalar front-end e back-end separadamente.  
❌ Dificulta CI/CD quando o front-end e o back-end precisam ser atualizados separadamente.  

---

## **3️⃣ FastAPI + React com WebSockets (Tempo Real)**
✅ **Melhor para:** Apps que precisam de comunicação **tempo real** (chats, notificações, dashboards dinâmicos).  

### 📌 Como funciona:
- Em vez de uma API REST tradicional, FastAPI usa **WebSockets** para enviar/receber dados em tempo real.
- O React escuta as mensagens do WebSocket e atualiza a interface.

### 🛠️ **Exemplo simples de WebSocket com FastAPI:**
```python
from fastapi import FastAPI, WebSocket
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        data = await websocket.receive_text()
        await websocket.send_text(f"Mensagem recebida: {data}")
```

### 🛠️ **Consumindo WebSocket no React**
```jsx
import { useEffect, useState } from "react";

function WebSocketComponent() {
  const [message, setMessage] = useState("");
  const ws = new WebSocket("ws://localhost:8000/ws");

  useEffect(() => {
    ws.onmessage = (event) => {
      setMessage(event.data);
    };
  }, []);

  return <h1>{message}</h1>;
}
```
🔹 **Vantagens:**  
✔ Comunicação em tempo real sem precisar de polling.  
✔ Melhor para chats, notificações e dashboards dinâmicos.  

🔸 **Desvantagens:**  
❌ WebSockets pode ser mais difícil de escalar do que REST.  
❌ Pode exigir proxies como **NGINX** para gerenciar conexões.  

---

## **4️⃣ FastAPI + React com GraphQL (Apollo Client)**
✅ **Melhor para:** Aplicações que precisam de **queries flexíveis** e **menor consumo de dados**.  

### 📌 Como funciona:
- Em vez de REST, FastAPI usa **GraphQL** para fornecer os dados.
- O React consome esses dados via **Apollo Client**.

### 🛠️ **Exemplo básico de GraphQL com FastAPI**
1. Instalar `strawberry-graphql`
   ```bash
   pip install strawberry-graphql
   ```
2. Criar API GraphQL no FastAPI:
   ```python
   import strawberry
   from strawberry.fastapi import GraphQLRouter
   from fastapi import FastAPI

   @strawberry.type
   class Query:
       hello: str = "Olá, mundo!"

   schema = strawberry.Schema(query=Query)

   app = FastAPI()
   app.include_router(GraphQLRouter(schema), prefix="/graphql")
   ```
3. No React, usar **Apollo Client**:
   ```jsx
   import { useQuery, gql } from "@apollo/client";

   const GET_MESSAGE = gql`
       query {
           hello
       }
   `;

   function App() {
       const { data } = useQuery(GET_MESSAGE);
       return <h1>{data?.hello}</h1>;
   }

   export default App;
   ```
🔹 **Vantagens:**  
✔ Evita chamadas desnecessárias à API (diferente de REST).  
✔ Cliente pode escolher exatamente os dados que precisa.  

🔸 **Desvantagens:**  
❌ Pode ser mais complexo para configurar do que REST.  
❌ Pode ser menos eficiente para **pequenos projetos**.  

---

## **5️⃣ FastAPI + React Server Components (Full Stack Moderno)**
✅ **Melhor para:** Aplicações **SSR (Server-Side Rendering)** com React 18+.  

### 📌 Como funciona:
- FastAPI fornece **dados e renderização no servidor**.
- React 18 usa **Server Components** para otimizar o carregamento.

### 🛠️ **Como implementar:**
- Criar uma API com FastAPI que renderiza HTML com dados dinâmicos.
- Usar Next.js com **getServerSideProps** para chamar a API.

🔹 **Vantagens:**  
✔ Melhor para SEO.  
✔ Carregamento inicial mais rápido.  

🔸 **Desvantagens:**  
❌ Pode ser mais complexo de configurar.  
❌ Exige mais conhecimento de SSR/ISR.  

---

## 🚀 **Qual abordagem escolher?**
| Abordagem                          | Melhor Para                     | Escalabilidade |
|------------------------------------|---------------------------------|---------------|
| **FastAPI REST + React SPA**       | Apps normais, APIs públicas     | Alta 🚀 |
| **FastAPI servindo React**         | Projetos pequenos e MVPs        | Média |
| **FastAPI + WebSockets**           | Chats, notificações em tempo real | Média |
| **FastAPI + GraphQL**              | Apps com queries complexas      | Alta 🚀 |
| **FastAPI + React Server Components** | SSR, apps otimizados para SEO  | Alta 🚀 |
