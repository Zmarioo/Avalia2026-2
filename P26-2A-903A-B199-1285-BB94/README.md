# Projeto #32: Gerenciador de Eventos e Controle de Presença (RSVP)

- **Modalidade:** Projeto Oficial Sorteado (Valendo 100 pontos)
- **Estudante:** Jose Mario Gonzaga Junior
- **Turma:** 2A
- **Chave Única:** `P26-2A-903A-B199-1285-BB94`
- **URL_CLOUDFLARE:** `https://p26-2a-903a-b199-1285-bb94.pages.dev` *(Atualize com a URL pública real do seu deploy na Cloudflare antes de abrir o Pull Request)*

---

## Especificação do Projeto

Construa uma plataforma para organização de palestras, grupos de estudo e encontros acadêmicos. O criador do evento (autenticado via Discord) define título, local, data, horário e capacidade máxima de participantes do auditório ou sala.

Quando outros estudantes acessam a página do evento e confirmam presença, o backend na Cloudflare verifica no banco D1 se a lotação máxima já foi atingida. Caso as vagas estejam esgotadas, o sistema insere automaticamente o usuário em uma fila de espera ordenada por horário de solicitação, promovendo o primeiro da fila caso algum participante confirmado cancele sua inscrição.

---

## Como Publicar esta Subpasta no Cloudflare Pages + D1 + Discord OAuth2

1. No painel da Cloudflare (**Workers & Pages** → **Create** → **Pages** → **Connect to Git**), selecione o seu fork do repositório `Avalia2026-2`.
2. Configure os parâmetros de build apontando para esta subpasta:
   - **Root directory (Diretório raiz):** `P26-2A-903A-B199-1285-BB94`
   - **Build command:** *(deixe em branco — HTML/CSS/JS puro)*
   - **Build output directory:** `public`
3. Crie o banco de dados D1 e aplique a migração inicial (no terminal do laboratório usando Scoop):
   ```powershell
   cd P26-2A-903A-B199-1285-BB94
   npx.cmd wrangler d1 create db-p26-2a-903a-b199-1285-bb94
   # Copie o database_id gerado para o arquivo wrangler.jsonc e execute:
   npx.cmd wrangler d1 migrations apply DB --remote
   ```
4. **Vincule o Banco D1 ao seu Cloudflare Pages (Obrigatório para ativar env.DB):**
   - No painel da Cloudflare: acesse seu projeto Pages → **Settings** → **Functions** → **D1 database bindings**.
   - Clique em **Add binding**:
     - **Variable name:** `DB`
     - **D1 database:** selecione o banco que você criou (`db-p26-2a-903a-b199-1285-bb94`).
5. **Configuração do Discord OAuth2 (Obrigatório para autenticação real):**
   - Acesse o **Discord Developer Portal** (https://discord.com/developers/applications) e crie uma nova Application.
   - Na aba **OAuth2**:
     - Copie o **Client ID** e gere o **Client Secret**.
     - Em **Redirects**, adicione a URL exata do deploy:
       `https://p26-2a-903a-b199-1285-bb94.pages.dev/api/auth/discord/callback`
   - No painel do seu projeto no Cloudflare Pages:
     - Acesse **Settings** → **Environment variables**.
     - Adicione as variáveis de ambiente:
       - `DISCORD_CLIENT_ID`
       - `DISCORD_CLIENT_SECRET`
6. Atualize o campo **URL_CLOUDFLARE** no topo deste `README.md` com o endereço público gerado pela Cloudflare.
7. Na raiz do repositório, execute `node validar-local.mjs P26-2A-903A-B199-1285-BB94` para validar todos os critérios antes de fazer o commit e abrir o Pull Request.
