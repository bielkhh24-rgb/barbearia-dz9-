# ✂️ Barbearia Dz9 — Bot de Agendamento WhatsApp

Sistema completo de agendamento via WhatsApp com painel de controle em tempo real.

## 🧱 Tecnologias

| Camada | Tecnologia |
|---|---|
| Bot WhatsApp | Baileys (não oficial, gratuito) |
| Backend | Node.js + Express |
| Banco de dados | SQLite (better-sqlite3) |
| Painel web | HTML + CSS + JS puro (SSE tempo real) |
| Deploy | Railway / Render |

---

## 📁 Estrutura do projeto

```
dz9-agendamento/
├── src/
│   ├── index.js      ← Ponto de entrada
│   ├── bot.js        ← Lógica do bot WhatsApp (Baileys)
│   ├── server.js     ← API REST + SSE (painel web)
│   ├── db.js         ← Banco de dados SQLite
│   └── utils.js      ← Serviços, horários, datas
├── public/
│   └── index.html    ← Painel de controle web
├── data/             ← Gerado automaticamente
│   ├── dz9.sqlite    ← Banco de dados
│   └── auth/         ← Sessão do WhatsApp
├── package.json
├── railway.json
├── Procfile
└── .gitignore
```

---

## 🚀 Instalação local

### Pré-requisitos
- Node.js 18 ou superior
- npm

### Passos

```bash
# 1. Clone ou extraia o projeto
cd dz9-agendamento

# 2. Instale as dependências
npm install

# 3. Inicie o sistema
npm start
```

Ao iniciar pela primeira vez:
1. Um **QR Code** aparecerá no terminal
2. Abra o **WhatsApp** no celular
3. Vá em **⋮ → Aparelhos Conectados → Conectar aparelho**
4. Escaneie o QR Code
5. Pronto! O bot está ativo ✅

Acesse o painel em: **http://localhost:3000**

---

## ☁️ Deploy no Railway (nuvem gratuita)

### 1. Criar conta
Acesse [railway.app](https://railway.app) e crie uma conta gratuita (pode usar o GitHub).

### 2. Subir o projeto no GitHub

```bash
# Na pasta do projeto
git init
git add .
git commit -m "Barbearia Dz9 - agendamento WhatsApp"

# Crie um repositório no GitHub e suba:
git remote add origin https://github.com/SEU_USUARIO/dz9-agendamento.git
git push -u origin main
```

### 3. Criar projeto no Railway

1. No Railway, clique em **"New Project"**
2. Escolha **"Deploy from GitHub repo"**
3. Selecione o repositório `dz9-agendamento`
4. O Railway detecta automaticamente o Node.js e faz o deploy

### 4. Adicionar volume persistente (importante!)

Para que o banco de dados e a sessão do WhatsApp não se percam entre deploys:

1. No Railway, vá em seu projeto → aba **"Volumes"**
2. Clique em **"Add Volume"**
3. Mount path: `/app/data`
4. Clique em **"Add"**

### 5. Acessar o QR Code

1. No Railway, vá em **"Deployments" → logs do deploy**
2. Ou acesse o painel web pelo domínio gerado pelo Railway
3. O QR Code aparecerá automaticamente no painel quando o bot precisar conectar

### 6. Domínio público

1. No Railway, vá em **Settings → Networking**
2. Clique em **"Generate Domain"**
3. Seu painel estará em: `https://dz9-agendamento-xxx.railway.app`

---

## 🤖 Fluxo do Bot WhatsApp

```
Cliente envia "oi"
    ↓
Bot exibe menu principal
    ↓
1 → Agendar | 2 → Ver agendamento | 3 → Cancelar
    ↓
[Agendar]
    ↓
Escolhe serviço → Escolhe data → Escolhe horário → Informa nome → Confirma
    ↓
Agendamento salvo no SQLite
    ↓
Painel web atualiza em tempo real (SSE)
```

---

## 🎛️ Painel de Controle

O painel web em `http://localhost:3000` (ou seu domínio Railway) oferece:

- **Agendamentos de hoje** com cards detalhados
- **Filtros** por status (Pendente / Confirmado / Concluído)
- **Busca** por nome, serviço ou telefone
- **Ações rápidas**: confirmar, concluir, remover
- **Novo agendamento manual** (sem precisar do WhatsApp)
- **Atualização em tempo real** quando chega novo agendamento pelo bot
- **Relatórios** com gráficos de serviços e resumo geral
- **Status do bot** (conectado / desconectado) visível no topo

---

## ✂️ Personalizando

### Alterar serviços disponíveis
Edite o arquivo `src/utils.js`:
```js
const SERVICOS = [
  { nome: '✂️ Corte',  preco: 35, duracao: 30 },
  // adicione ou remova serviços aqui
];
```

### Alterar horários de funcionamento
No mesmo arquivo `src/utils.js`:
```js
const TODOS_HORARIOS = [
  '08:00','08:30','09:00', // ...
];
```

### Alterar mensagens do bot
Edite o objeto `MSG` em `src/bot.js`.

### Alterar nome/telefone do atendente
Em `src/bot.js`, na mensagem `atendente`:
```js
atendente: () => `📞 ... ligue: *(34) 9XXXX-XXXX*`
```

---

## ⚠️ Avisos importantes

- **Baileys** é uma biblioteca não oficial. O WhatsApp pode banir números usados em automação se abusado. Use com moderação.
- Para uso profissional em escala, considere a [WhatsApp Business API oficial](https://business.whatsapp.com/products/business-platform).
- A sessão fica salva na pasta `data/auth/`. **Não apague essa pasta** ou terá que escanear o QR Code novamente.
- O SQLite funciona bem para volumes pequenos/médios (até ~10.000 agendamentos). Para escala maior, migre para PostgreSQL.

---

## 🆘 Problemas comuns

| Problema | Solução |
|---|---|
| QR Code não aparece | Verifique os logs com `npm start` |
| Bot desconecta sozinho | Normal após alguns dias; escaneie novamente |
| "Cannot find module" | Rode `npm install` novamente |
| Porta já em uso | Mude a porta: `PORT=3001 npm start` |
| Banco corrompido | Delete `data/dz9.sqlite` e reinicie |

---

Feito com 💜 para a **Barbearia Dz9**
