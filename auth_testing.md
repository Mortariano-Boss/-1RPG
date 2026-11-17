# Auth Testing — +1 RPG

Stack: FastAPI + MongoDB (motor) + JWT em cookies httpOnly (access 24h / refresh 7d) + bcrypt.

## Verificações
1. MongoDB: `mongosh` → db `test_database` → `db.users.find({role:"admin"})`; hash bcrypt começa com `$2b$`; índice único em `users.email`; `login_attempts.identifier`.
2. API:
```
curl -c cookies.txt -X POST http://localhost:8001/api/auth/login -H "Content-Type: application/json" -d '{"email":"admin@mais1rpg.com.br","password":"admin123"}'
curl -b cookies.txt http://localhost:8001/api/auth/me
```
3. Fluxos testados no build (via URL externa): register → me → interest → dup-interest (400) → mestre aceita → slots decrementam → favoritar → pedido da loja → denúncia.

## Endpoints de auth
- POST /api/auth/register {display_name, email, password, role(jogador|mestre|ambos), birth_date?, accept_terms:true}
- POST /api/auth/login {email, password}
- GET /api/auth/me (cookie ou Bearer)
- POST /api/auth/logout, POST /api/auth/refresh

## Contas seed (somente criadas se a coleção estava vazia)
- Admin: admin@mais1rpg.com.br / admin123 (role admin)
- Mestre demo: mestre@mais1rpg.com.br / mestre123 (6 mesas seed)
- Jogador demo: jogador@mais1rpg.com.br / jogador123
- Jogador de teste E2E: teste.e2e@mais1rpg.com.br / teste123 (tem 1 solicitação "accepted" em "O Despertar de Aether")
