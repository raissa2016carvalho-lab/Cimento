# 🚛 Transport Cimento

Sistema de gerenciamento de frota e despacho de cimento com dois painéis:

- **`/fornecedor`** — Fornecedor cadastra veículos disponíveis no dia (placa + modal + motorista)
- **`/despacho`** — Despachante aloca cargas: destino, pedido, toneladas e sacos de 50kg

---

## ⚙️ Configuração Firebase

### 1. Criar projeto no Firebase
1. Acesse [console.firebase.google.com](https://console.firebase.google.com)
2. Clique em **"Adicionar projeto"** → dê um nome → criar
3. No menu lateral, vá em **Firestore Database** → **Criar banco de dados**
   - Escolha **Modo de produção** ou **Modo de teste** (teste é mais fácil para começar)
   - Selecione a região `us-east1` ou `southamerica-east1`
4. Vá em **Configurações do projeto** (ícone de engrenagem) → **Seus apps** → clique em `</>` (Web)
5. Registre o app e copie as credenciais

### 2. Regras do Firestore (modo teste)
No Firestore → **Regras**, cole:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
> ⚠️ Para produção, adicione autenticação!

### 3. Variáveis de ambiente
Copie `.env.example` para `.env` e preencha com suas credenciais:
```bash
cp .env.example .env
```

---

## 🚀 Deploy no Vercel (via GitHub)

### 1. Subir no GitHub
```bash
git init
git add .
git commit -m "feat: sistema transport cimento"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/SEU_REPO.git
git push -u origin main
```

### 2. Conectar ao Vercel
1. Acesse [vercel.com](https://vercel.com) → **Add New Project**
2. Importe o repositório do GitHub
3. Framework: **Create React App** (detectado automaticamente)
4. Em **Environment Variables**, adicione TODAS as variáveis do `.env`:
   - `REACT_APP_FIREBASE_API_KEY`
   - `REACT_APP_FIREBASE_AUTH_DOMAIN`
   - `REACT_APP_FIREBASE_PROJECT_ID`
   - `REACT_APP_FIREBASE_STORAGE_BUCKET`
   - `REACT_APP_FIREBASE_MESSAGING_SENDER_ID`
   - `REACT_APP_FIREBASE_APP_ID`
5. Clique em **Deploy**

### 3. URLs geradas
- **Fornecedor:** `https://seu-projeto.vercel.app/fornecedor`
- **Despacho:** `https://seu-projeto.vercel.app/despacho`

---

## 💻 Rodar localmente

```bash
npm install
cp .env.example .env   # preencha as credenciais
npm start
```

---

## 🗂 Estrutura do Firestore

### Coleção: `veiculos_disponiveis`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `placa` | string | Placa do veículo (ex: ABC1234) |
| `modal` | string | Tipo do veículo (Truck, Rodotrem...) |
| `motorista` | string | Nome do motorista |
| `capacidadeTon` | number | Capacidade em toneladas |
| `status` | string | `disponivel` ou `alocado` |
| `data` | string | Data no formato YYYY-MM-DD |
| `criadoEm` | timestamp | Timestamp de criação |

### Coleção: `pedidos_despacho`
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `placa` | string | Placa vinculada |
| `modal` | string | Tipo do veículo |
| `motorista` | string | Nome do motorista |
| `destino` | string | Local de entrega |
| `obsDestino` | string | Endereço / observação |
| `pedido` | string | Número do pedido |
| `toneladas` | number | Quantidade em toneladas |
| `sacos` | number | Quantidade de sacos (50kg) |
| `data` | string | Data YYYY-MM-DD |
| `criadoEm` | timestamp | Timestamp |
| `status` | string | `despachado` |

---

## 📱 Modais disponíveis

| Modal | Capacidade |
|-------|-----------|
| Toco (2 eixos) | 8t |
| Truck (3 eixos) | 14t |
| Bitruck (4 eixos) | 20t |
| Carreta Simples | 27t |
| Carreta LS | 29t |
| Vanderleia | 33t |
| Bitrem (7 eixos) | 45t |
| Treminhão (7 eixos) | 57t |
| Rodotrem (9 eixos) | 74t |
