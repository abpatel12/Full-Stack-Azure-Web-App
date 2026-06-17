# Full-Stack Azure Web App

A production-ready full-stack **Employee Management System** deployed on **Microsoft Azure**, featuring a React.js frontend, ASP.NET Core backend, and Azure SQL Database — automated end-to-end with CI/CD pipelines in **Azure DevOps**.

**Live URLs:**
- 🌐 Frontend: `https://htmlfrontend12345-bwhdcrngbwghjay.koreasouth-01.azurewebsites.net`
- 🔗 Backend API: `https://backend123455-csc5cphxerafg4ey.koreasouth-01.azurewebsites.net/api/employee`

---

## 🏗️ Architecture

```
┌──────────────────────────────────┐
│  React JS Frontend 1             │
│  htmlfrontend12345               │
│  Azure App Service (Korea South) │
└────────────────┬─────────────────┘
                 │ HTTP (REST)
┌────────────────▼─────────────────┐
│  ASP.NET Core Web API            │
│  backend123455                   │
│  Azure App Service (Korea South) │
└────────────────┬─────────────────┘
                 │ SQL
┌────────────────▼─────────────────┐
│  Azure SQL Database              │
│  majorproj1.database.windows.net │
│  Resource Group: ab123           │
└──────────────────────────────────┘
         │ Secrets
┌────────▼──────────────────────┐
│  Azure Key Vault: sqldata     │
│  Region: East US              │
│  Variables: Server, Database, │
│  User, Password               │
└───────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology | Resource |
|---|---|---|
| Frontend | React.js | `htmlfrontend12345` — Korea South |
| Backend | ASP.NET Core (.NET) | `backend123455` — Korea South |
| Database | Azure SQL Server + Database | `majorproj1.database.windows.net` |
| Secrets | Azure Key Vault | `sqldata` — East US |
| CI/CD | Azure DevOps Pipelines | org: `abdullahmohd3047` |
| Hosting | Azure App Service (x2) | Resource Group: `ab123` |

---

## 🚀 CI/CD Pipelines (Azure DevOps)

### 1. Database Pipeline — project: `sql`

**CI (`sql-CI`):**
- Publishes SQL DACPAC artifact from the `main` branch

**CD (Release pipeline — SQL DACPAC Task):**
- Deploys to `majorproj1.database.windows.net` using Azure SQL Database deployment task
- Credentials injected securely from **Azure Key Vault (`sqldata`)** via Library variable group:
  - `$(Server)`, `$(Database)`, `$(User)`, `$(Password)`
- Action: **Publish** (deploy schema/data)

### 2. Backend Pipeline — project: `backend`

**CI (`backend-ASP.NET Core-CI`):**
| Step | Duration |
|---|---|
| Initialize job | — |
| Checkout `backend` @ main | 5s |
| Restore | 27s |
| Build | 11s |
| Publish (.NET Core) | 3s |
| Publish Artifact (`drop`) | <1s |

**CD (Release pipeline):**
- Deploys `drop` artifact to **`backend123455`** Azure App Service
- Stage 1: ✅ Succeeded

### 3. Frontend Pipeline — project: `backend` (React)

**CI:**
- `npm install` → `npm run build` → Publish build artifact
- `variable.js` configured with backend App Service URL

**CD:**
- Deploys to **`htmlfrontend12345`** Azure App Service
- Stage 1: ✅ Succeeded

---

## 🔐 Security

- All credentials stored in **Azure Key Vault `sqldata`** (East US, Standard tier, Vault access policy)
- Library variable group in Azure DevOps linked directly to Key Vault — secrets never hardcoded
- SQL authentication with scoped SQL user (`majorproj1`)

---

## ✅ Live API Response

`GET /api/employee`

```json
[
  {
    "EmployeeId": 1,
    "EmployeeName": "Abdullah Mohammed",
    "Department": "Cloud Infrastructure",
    "DateOfJoining": "2026-06-17",
    "PhotoFileName": "avatar.png"
  },
  {
    "EmployeeId": 2,
    "EmployeeName": "Jane Smith",
    "Department": "Cybersecurity",
    "DateOfJoining": "2026-06-17",
    "PhotoFileName": "jane.png"
  }
]
```

---

## 🖥️ Frontend Features

The React frontend ("React JS Frontend 1") includes:
- **Employee** tab — full CRUD: view, add, edit, delete employees
- **Department** tab — manage departments
- **Home** tab — dashboard view
- Live data from the ASP.NET Core API → Azure SQL Database

---

## 📁 Repository Structure

```
/
├── frontend/               # React.js app
│   ├── src/
│   │   └── variable.js     # Backend API URL config
│   └── package.json
├── backend/                # ASP.NET Core Web API
│   ├── Controllers/
│   │   └── EmployeeController.cs
│   └── appsettings.json
└── database/               # SQL scripts (DACPAC)
    └── schema.sql
```

---

## ☁️ Azure Resources Summary

| Resource | Name | Region |
|---|---|---|
| Resource Group | `ab123` | Korea South |
| SQL Server | `majorproj1` | Korea South |
| Key Vault | `sqldata` | East US |
| Backend App Service | `backend123455` | Korea South |
| Frontend App Service | `htmlfrontend12345` | Korea South |
| DevOps Organization | `abdullahmohd3047` | — |
