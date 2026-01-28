# 🏠 Como Hospedar o Cloaker Pro

## ⚠️ Importante: HostGator e Node.js

| Tipo de plano HostGator | Node.js funciona? |
|------------------------|-------------------|
| **Hospedagem compartilhada** (cPanel, PHP) | ❌ **NÃO** – não roda Node.js |
| **VPS** ou **Servidor dedicado** | ✅ **SIM** – você instala Node.js |

**Este projeto é Node.js.** Não basta zipar e enviar pelo cPanel na hospedagem **compartilhada** – ela não executa Node.js.

---

## 📦 Sobre o banco de dados

- **Não usa MySQL.**  
- Usa **SQLite**: um único arquivo `cloaker.db` na pasta do projeto.  
- Esse arquivo é criado automaticamente quando o servidor inicia.  
- Você **não** precisa criar banco no cPanel.

---

## Opção 1: Você tem VPS na HostGator

Se seu plano for **VPS** (ou dedicado), dá para rodar o projeto assim:

### 1. Acessar por SSH

- No painel da HostGator, use os dados de SSH (usuário, senha, IP ou host).
- Conecte com PuTTY (Windows) ou terminal: `ssh usuario@ip-do-servidor`

### 2. Instalar Node.js no VPS

```bash
# Instalar Node.js 18 (exemplo no Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 3. Enviar o projeto

- Zipar a pasta do projeto (sem `node_modules`).
- Enviar por FTP/SFTP para uma pasta no VPS (ex: `/home/usuario/cloaker`).
- Ou usar Git, se a HostGator permitir.

### 4. No servidor, instalar e rodar

```bash
cd /home/usuario/cloaker   # ou a pasta onde você enviou
npm install
npm start
```

Para manter rodando 24h (recomendado):

```bash
npm install -g pm2
pm2 start server.js --name cloaker
pm2 save
pm2 startup
```

### 5. Porta e domínio

- O app usa a porta **3000** (ou a que estiver no `server.js`).
- No painel da HostGator (VPS), configure:
  - **Firewall**: liberar a porta 3000 (ou a que usar).
  - **Domínio/subdomínio**: apontar para o IP do VPS e, se quiser, usar um proxy reverso (ex.: Nginx) para a porta 3000.

---

## Opção 2: Você tem só hospedagem COMPARTILHADA (cPanel)

Nesse caso **não dá para rodar este projeto Node.js** na HostGator só fazendo upload de arquivos.

Você pode:

### A) Usar um serviço grátis que roda Node.js

Envie o projeto (zip ou Git) para um desses e faça o deploy:

| Serviço   | Site        | Observação              |
|-----------|-------------|--------------------------|
| **Railway** | railway.app | Grátis, deploy fácil    |
| **Render**  | render.com  | Grátis, deploy fácil    |
| **Cyclic**  | cyclic.sh   | Grátis para Node.js     |

Passo geral nesses serviços:

1. Criar conta.
2. “New Project” / “Deploy”.
3. Conectar GitHub (ou enviar o zip/arquivos).
4. Eles detectam Node.js, rodam `npm install` e `npm start`.
5. Eles te dão uma URL (ex: `https://seu-app.railway.app`).

Use essa URL como “servidor” do cloaker (onde você cola o script nos sites).

### B) Subir para um VPS em outro lugar

- Contratar um VPS barato (ex.: DigitalOcean, Vultr, Contabo) e seguir os passos da **Opção 1**, mas no novo servidor.

---

## Resumo rápido

| Situação | O que fazer |
|----------|-------------|
| **HostGator compartilhada** | Não roda Node.js. Use Railway/Render (grátis) ou outro VPS. |
| **HostGator VPS** | SSH → instalar Node.js → enviar projeto → `npm install` → `pm2 start server.js`. |
| **Banco de dados** | Não precisa MySQL. O SQLite usa o arquivo `cloaker.db` na pasta do projeto. |

Se você disser se é **compartilhada** ou **VPS**, dá para detalhar só o caminho que vale para o seu caso (por exemplo só “HostGator VPS” ou só “Railway/Render”).
