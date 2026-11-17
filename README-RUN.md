# +1 RPG — Código-fonte completo

Plataforma web de conexão entre mestres e jogadores de RPG (+1 RPG).

## O que vem no pacote

```
├── backend/               API FastAPI + MongoDB (server.py, scripts auxiliares)
│   ├── .env.example       Exemplo de variáveis de ambiente do backend
│   └── requirements.txt   Dependências Python
├── frontend/              App React (Create React App) + Tailwind
│   ├── .env.example       Exemplo de variáveis de ambiente do frontend
│   ├── node_modules/      Dependências já incluídas no ZIP
│   └── package.json
└── README-RUN.md          Este guia
```

⚠️ Por segurança, os arquivos `.env` reais (com chaves) NÃO foram incluídos.
Use os arquivos `.env.example` como base.

## Como rodar (backend)

```bash
cd backend
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env      # edite o .env com seus valores
uvicorn server:app --host 0.0.0.0 --port 8001 --reload
```

Pré-requisitos: Python 3.11+ e um MongoDB acessível na `MONGO_URL`.

## Como rodar (frontend)

```bash
cd frontend
cp .env.example .env      # aponte REACT_APP_BACKEND_URL para o backend
yarn install              # opcional: o node_modules já vem no ZIP (Linux/Mac)
yarn start                # abre em http://localhost:3000
```

No Windows, se o `node_modules` incluído apresentar problemas (symlinks),
basta rodar `yarn install` para reconstruir as dependências.

## Chaves de serviços (preencha no backend/.env)

| Serviço        | Variável                                       | Onde obter |
|----------------|------------------------------------------------|------------|
| Stripe         | STRIPE_SECRET_KEY / STRIPE_PUBLISHABLE_KEY     | dashboard.stripe.com |
| E-mail (Resend)| EMERGENT_EMAIL_KEY                             | Painel Emergent |
| IA (imagens)   | EMERGENT_LLM_KEY                               | Painel Emergent (Chave Universal) |

O admin inicial é criado automaticamente no primeiro boot com
`ADMIN_EMAIL` / `ADMIN_PASSWORD` definidos no `.env`.

## Funcionalidades principais

- Autenticação JWT com cookies HTTP-only e áreas de jogador/mestre/admin
- Exploração de mesas por temas e tags, candidaturas e aprovações
- Loja com carrinho e checkout Stripe (+ Mercado Pago PIX opcional)
- Mural do Mestre com avisos, curtidas e comentários de seguidores
- Perfis públicos, seguidores, amizades com convites por e-mail
- Roll semanal de D20 para desbloquear cosméticos e avatares
- Página dedicada do sistema "Nijias" com produtos gerados por IA
- Painel admin: denúncias, advertências, suspensões automáticas,
  histórico de aceites dos termos versionados
