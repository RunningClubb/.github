# 🏃‍♂️ RunningClub

**RunningClub** est une plateforme sportive inspirée de Strava, dédiée au suivi des performances running, à la gestion de clubs et au partage d'activités au sein d'une communauté de coureurs.

## 🎯 Fonctionnalités principales

- 📊 Suivi détaillé des runs (distance, allure, dénivelé, GPS)
- 👥 Gestion de clubs de running et challenges communautaires
- 📈 Tableaux de bord personnalisés et statistiques
- 🗺️ Cartographie des parcours avec heatmaps
- 🔔 Notifications et classements en temps réel

## 🏗️ Architecture

```mermaid
graph TB
    subgraph Azure["🌐 Azure Cloud"]
        ACI1[Container Instance<br/>WebApp] 
        ACI2[Container Instance<br/>API Backend]
        ACI3[Container Instance<br/>PostgreSQL DB]
        VNET[VNet + Subnets]
    end
    
    subgraph Local["💻 Développement Local"]
        FE[WebApp<br/>React/Next.js]
        BE[API<br/>Node.js/Express]
        DB[PostgreSQL Local]
    end
    
    subgraph CI_CD["🔄 CI/CD Pipeline"]
        GitHub[GitHub Repo]
        AzurePipelines[Azure DevOps]
        TF[Terraform Apply]
        DockerBuild[Docker Build & Push]
    end
    
    GitHub --> AzurePipelines
    AzurePipelines --> TF
    AzurePipelines --> DockerBuild
    TF --> VNET
    TF --> ACI1
    TF --> ACI2
    TF --> ACI3
    
    FE -.-> ACI1
    BE -.-> ACI2
    DB -.-> ACI3
    
    ACI2 -.-> ACI3
    ACI1 -.-> ACI2
    
    classDef azure fill:#0078D4,stroke:#fff,color:#fff
    classDef local fill:#F0F0F0,stroke:#333
    classDef cicd fill:#FFA500,stroke:#333
    
    class ACI1,ACI2,ACI3,VNET azure
    class FE,BE,DB local
    class GitHub,AzurePipelines,TF,DockerBuild cicd
```

## 📁 Structure du projet

```
RunningClub/
├── infra/                 # 🛠️ Infrastructure as Code (Terraform)
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── azure-container.tf
├── webapp/                # 🌐 Frontend React/Next.js
│   ├── src/
│   ├── Dockerfile
│   └── package.json
├── api/                   # ⚙️ Backend API Node.js
│   ├── src/
│   ├── Dockerfile
│   └── package.json
├── bdd/                   # 🗄️ Base de données PostgreSQL
│   ├── schema.sql
│   ├── migrations/
│   └── seed.sql
├── .github/               # 🤖 GitHub Actions (futur)
└── docs/                  # 📚 Documentation
```

## 🚀 Déploiement IaC avec Terraform

### Prérequis
```bash
# Installer les outils
terraform version >= 1.5
az login
docker buildx
```

### Déploiement en 3 étapes

```mermaid
sequenceDiagram
    participant Dev as Développeur
    participant Git as GitHub
    participant TF as Terraform
    participant Azure as Azure Cloud
    
    Dev->>Git: git push
    Git->>TF: terraform init
    TF->>TF: terraform plan
    TF->>Azure: terraform apply
    Azure->>Azure: Containers up & running
    Azure-->>Dev: URLs de production
```

```bash
cd infra
terraform init
terraform plan
terraform apply
```

**Outputs Terraform :**
```
webapp_url = "https://runningclub-web-[random].azurecontainer.io"
api_url = "https://runningclub-api-[random].azurecontainer.io"
db_connection = "postgresql://user:pass@host:5432/RunningClub"
```

## 🔄 CI/CD Pipeline (À venir)

**Pipeline Azure DevOps planifié :**
- Tests unitaires + E2E
- Build multi-arch Docker images
- Déploiement Terraform avec approval gates
- Rollback automatique

## 🛠️ Stack technologique

| Composant | Technologie | Version |
|-----------|-------------|---------|
| Infrastructure | Terraform + Azure | 1.5+ |
| Frontend | React/Next.js | 14+ |
| Backend | Node.js + Express | 20+ |
| Base de données | PostgreSQL | 16+ |
| Conteneurs | Docker + ACI | Latest |
| CI/CD | Azure DevOps | - |

## 🤝 Contribution

1. Fork le projet
2. Créer une feature branch `feat/nouvelle-fonction`
3. Commit avec Conventional Commits
4. Push et créer une Pull Request

```bash
git checkout -b feat/gestion-clubs
git commit -m "feat: add club management endpoints"
git push origin feat/gestion-clubs
```

***

**🚀 Status : En développement | Prochain milestone : MVP v1.0 avec CI/CD**

<div align="center">
  <img src="https://img.shields.io/badge/Ready%20for%20MVP-%F0%9F%9A%80-green" alt="MVP Ready">
</div>
