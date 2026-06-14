# 🔌 MCP Stack — Master Connection Guide (Hetzner VPS)

> **Drop this file into any project.** Any agent reading this file has everything it needs to connect to every MCP server in the stack.  
> Master config: `/home/julio/projects/mcp/mcp_servers.json`  
> **Esta VPS:** `46.62.233.228` (Hetzner) — VPS anterior `45.55.90.156` (DigitalOcean) **deprecada**.

---

## ⚡ Quick Reference — All Connections

| Server | Transport | Command | Auth Env Var |
|---|---|---|---|
| **github** | Stdio | `npx -y @modelcontextprotocol/server-github` | `GITHUB_TOKEN` / `GITHUB_PERSONAL_ACCESS_TOKEN` |
| **supabase** | Stdio | `npx -y @supabase/mcp-server-supabase@latest --access-token <token>` | `SUPABASE_ACCESS_TOKEN` (en `.env`) |
| **firecrawl** | Stdio | `npx -y firecrawl-mcp` | `FIRECRAWL_API_KEY` (en `.env`) |
| **netlify** | Stdio | `npx -y @netlify/mcp` | `NETLIFY_PERSONAL_ACCESS_TOKEN` |
| **vercel** | Stdio | `npx -y mcp-remote https://mcp.vercel.com --header "Authorization:Bearer <token>"` | `VERCEL_MCP_TOKEN` |
| **image-tools** | Stdio | `npx -y image-tools-mcp` | `TINIFY_API_KEY` |
| **dataforseo** | Stdio | `node /home/julio/projects/first-shot-website/dataforseo-mcp/build/main/main/index.js` | `DATAFORSEO_USERNAME` + `DATAFORSEO_PASSWORD` |

---

## 🖥 Esta VPS (Hetzner — `46.62.233.228`)

No hay servidores SSE persistentes acá. **Todo es stdio** via `npx` o `node`.

```bash
# Verificar conectividad a GitHub (token del .env)
curl -s -o /dev/null -w "%{http_code}" \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  https://api.github.com/user
# → 200 OK
```

---

## 🗑 VPS Anterior — DigitalOcean (45.55.90.156) — DEPRECADA

La VPS de DigitalOcean (`45.55.90.156`) está deprecada. Los SSE bridges que apuntaban ahí ya no funcionan.

---

## 📦 Cómo levantar un servidor (Stdio)

```bash
# GitHub
GITHUB_PERSONAL_ACCESS_TOKEN=$GITHUB_TOKEN npx -y @modelcontextprotocol/server-github

# Firecrawl
FIRECRAWL_API_KEY=$FIRECRAWL_API_KEY npx -y firecrawl-mcp

# Supabase
npx -y @supabase/mcp-server-supabase@latest --access-token $SUPABASE_ACCESS_TOKEN

# Netlify
NETLIFY_PERSONAL_ACCESS_TOKEN=$NETLIFY_PERSONAL_ACCESS_TOKEN npx -y @netlify/mcp

# Vercel (remoto, usa mcp-remote bridge)
npx -y mcp-remote https://mcp.vercel.com --header "Authorization:Bearer $VERCEL_MCP_TOKEN"

# Image Tools
TINIFY_API_KEY=$TINIFY_API_KEY npx -y image-tools-mcp

# DataForSEO
DATAFORSEO_USERNAME=$DATAFORSEO_USERNAME DATAFORSEO_PASSWORD=$DATAFORSEO_PASSWORD \
  node /home/julio/projects/first-shot-website/dataforseo-mcp/build/main/main/index.js
```

---

## 🔐 Required `.env` Keys (desde `/home/julio/projects/.env`)

```bash
# Anthropic / Claude
ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY      # value in .env

# GitHub (verificado ✅ — 200 OK como Cesars-dev)
GITHUB_TOKEN=$GITHUB_TOKEN                # value in .env

# Firecrawl
FIRECRAWL_API_KEY=$FIRECRAWL_API_KEY      # value in .env

# Supabase
SUPABASE_ACCESS_TOKEN=$SUPABASE_ACCESS_TOKEN  # value in .env

# DigitalOcean / S3
S3_ACCESS_KEY=$S3_ACCESS_KEY              # value in .env
S3_SECRET_KEY=$S3_SECRET_KEY              # value in .env
S3_REGION=auto

# VPS Hetzner (n8n)
N8N_VPS_HOST=129.212.184.175
N8N_VPS_SSH_KEY=~/.ssh/id_ed25519
N8N_VPS_SSH_PASSPHRASE=$N8N_VPS_SSH_PASSPHRASE  # value in .env
```

---

## 🧠 Agent Rules

1. **Solo stdio** en esta VPS — no hay SSE servers locales.
2. **Preferir `npx -y`** para servidores npm, `node` para scripts custom.
3. **Usar variables del `.env`** — nunca hardcodear secrets en configs.
4. **Levantar servidores on-demand** — cada proceso consume ~160MB RAM.
5. **Matar el servidor al terminar** — no dejar procesos zombi.

---

> **Última actualización:** 2026-06-03  
> **VPS activa:** Hetzner — `46.62.233.228`  
> **VPS deprecada:** DigitalOcean — `45.55.90.156`
