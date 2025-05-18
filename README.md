
# Déploiement Kubernetes - Projet IC GROUP

## 📄 Contexte
Ce projet vise à déployer trois applications dans un cluster Kubernetes local (Minikube) :
- `ic-webapp` : application Flask (site vitrine)
- `pgAdmin` : interface d’administration PostgreSQL
- `Odoo` : ERP open source connecté à PostgreSQL

## 📦 Environnement
- OS : Debian 12
- Cluster : Minikube avec Docker driver
- Namespace : `icgroup`
- Labels globaux : `env=prod`

## 🚀 Services déployés
- Deployments : `ic-webapp`, `pgadmin`, `odoo`, `postgres`
- PVCs : `pgdata`, `odoo-pvc`
- Services : `webapp-service`, `pgadmin-service`, `odoo-service`

## 📸 Captures d’écran

### Minikube + ressources
![Pods et Services](images/2a5a6637-2f01-459f-991c-506b5706abc7.png)

### Accueil site vitrine (`ic-webapp`)
![ic-webapp](images/c2e76e78-19da-4895-ba19-40323364f81d.png)

### pgAdmin opérationnel
![pgadmin](images/87c8b1a8-ab9e-4039-bb9b-3e916900bdeb.png)

### Odoo CrashLoopBackOff
![odoo crash](images/b5ec275c-c66c-46d4-9ff3-07beaf297e88.png)

### Inspect Pod Odoo
![describe pod](images/b44bbd75-5ecc-4d73-81aa-242b2b8e3a37.png)

### Tentatives accès Odoo (navigateur)
![Odoo navigateur NOK](images/41e10fda-c7b1-4428-bfbd-083fef00b936.png)
![Odoo navigateur NOK 2](images/8c8e9af9-ecae-46de-9e69-bab12dc87374.png)

### Pod Odoo en `Running`
![Running](images/618f6a06-3cf8-446f-91da-7816e96e7197.png)

### Erreur finale `host "db"`
![Erreur DB](images/9172e634-50e1-48ef-8e33-f8749794932d.png)

## ❗ Problème identifié

Malgré l'ajout des arguments de connexion PostgreSQL dans `odoo.yaml` :
```yaml
args: ["--db_host=postgresql-service", "--db_user=odoo", "--db_password=odoo"]
```
Le conteneur Odoo tente toujours de se connecter à `host=db`, ce qui indique qu’il n’utilise pas la configuration attendue.

## ✅ Recommandations

- Supprimer entièrement le déploiement :
```bash
kubectl delete deployment odoo -n icgroup
kubectl apply -f odoo.yaml -n icgroup
```

- Confirmer via `kubectl logs` que le nom d’hôte PostgreSQL est bien `postgresql-service`

---

**Auteur : Hayouba - Projet PROET-KUB**
