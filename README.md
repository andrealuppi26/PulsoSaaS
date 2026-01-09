# Portal do Cliente - Instruções de Deploy

## 📦 Arquivos do Portal

Você recebeu 4 arquivos:
- `config.js` - Configurações do Supabase (já configurado!)
- `index.html` - Página de login
- `dashboard.html` - Painel principal
- `meu-negocio.html` - Página de gerenciamento do negócio

## 🚀 Como fazer deploy na Vercel

### Opção 1: Deploy via Interface (MAIS FÁCIL)

1. **Baixe os arquivos** que estão na pasta `/mnt/user-data/outputs`

2. **Acesse:** https://vercel.com

3. **Faça login** com sua conta Google (que você já criou)

4. **Clique em:** "Add New" → "Project"

5. **Escolha:** "Deploy from GitHub" OU "Import Git Repository"
   - Se não quiser usar Git, escolha "Import" e arraste os arquivos

6. **OU use o Vercel CLI (mais rápido):**
   - Instale: `npm i -g vercel`
   - Na pasta dos arquivos, rode: `vercel`
   - Siga as instruções na tela

### Opção 2: Deploy via GitHub (RECOMENDADO)

1. **Crie um repositório no GitHub:**
   - Acesse: https://github.com/new
   - Nome: `portal-cliente`
   - Deixe como "Public" ou "Private"
   - Clique "Create repository"

2. **Faça upload dos arquivos:**
   - Na página do repositório, clique "Add file" → "Upload files"
   - Arraste os 4 arquivos
   - Commit: "Portal inicial"

3. **Conecte com Vercel:**
   - Volte para https://vercel.com
   - Clique "Import Project"
   - Escolha o repositório `portal-cliente`
   - Clique "Deploy"

4. **Pronto!** Em 1-2 minutos seu portal estará no ar!

## 🌐 Acessando o Portal

Após o deploy, a Vercel vai te dar um link tipo:
- `https://portal-cliente.vercel.app`
- Ou `https://portal-cliente-seu-usuario.vercel.app`

Compartilhe esse link com seus clientes!

## 👥 Como cadastrar clientes

1. **No Supabase** (`https://supabase.com/dashboard/project/smdgakxjggijnralnovl`)

2. **Vá em SQL Editor** e rode:

```sql
-- Criar cliente
INSERT INTO clientes (email, nome, empresa)
VALUES ('cliente@email.com', 'Nome do Cliente', 'Empresa Ltda');

-- Criar config
INSERT INTO configs_portal (cliente_id, link_chatwoot, whatsapp_conectado, calendar_conectado)
VALUES (
  (SELECT id FROM clientes WHERE email = 'cliente@email.com'),
  'https://link-do-chatwoot-dele.com',
  false,
  false
);

-- Criar usuário no Auth
INSERT INTO auth.users (
  instance_id, id, aud, role, email, encrypted_password, 
  email_confirmed_at, raw_app_meta_data, raw_user_meta_data, 
  created_at, updated_at, confirmation_token, email_change, 
  email_change_token_new, recovery_token
) VALUES (
  '00000000-0000-0000-0000-000000000000',
  gen_random_uuid(),
  'authenticated',
  'authenticated',
  'cliente@email.com',
  crypt('SenhaDoCliente123!', gen_salt('bf')),
  NOW(),
  '{"provider":"email","providers":["email"]}',
  '{}',
  NOW(), NOW(), '', '', '', ''
);
```

3. **Repita** para cada cliente novo!

## 🔧 Configurações

### Mudar link do Chatwoot:
```sql
UPDATE configs_portal
SET link_chatwoot = 'https://novo-link.com'
WHERE cliente_id = (SELECT id FROM clientes WHERE email = 'cliente@email.com');
```

### Atualizar status WhatsApp:
```sql
UPDATE configs_portal
SET whatsapp_conectado = true
WHERE cliente_id = (SELECT id FROM clientes WHERE email = 'cliente@email.com');
```

### Atualizar status Calendar:
```sql
UPDATE configs_portal
SET calendar_conectado = true
WHERE cliente_id = (SELECT id FROM clientes WHERE email = 'cliente@email.com');
```

## 🎨 Personalizar

Para mudar cores, logo, etc:
1. Edite os arquivos HTML
2. Faça commit no GitHub
3. Vercel faz deploy automático!

## 📞 Suporte

Qualquer dúvida, me chama! 🚀
