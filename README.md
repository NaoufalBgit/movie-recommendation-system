# 🎬 Movie Recommendation System

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

> Plateforme web full-stack de recommandation de films avec système hybride combinant filtrage collaboratif et filtrage basé sur le contenu.

![Demo](assets/demo.gif)
*Interface utilisateur de la plateforme de recommandation*

---

## 📖 Table des matières

- [À propos](#à-propos)
- [Fonctionnalités](#fonctionnalités)
- [Architecture](#architecture)
- [Technologies](#technologies)
- [Dataset](#dataset)
- [Algorithmes](#algorithmes)
- [Résultats](#résultats)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Structure du projet](#structure-du-projet)
- [Roadmap](#roadmap)
- [Auteur](#auteur)

---

## 🎯 À propos

Ce projet consiste en une **plateforme de recommandation de films** de type VOD (Video On Demand) qui utilise des techniques avancées de Machine Learning pour suggérer des films personnalisés aux utilisateurs.

Le système implémente une approche **hybride** combinant :
- **Filtrage collaboratif** (SVD, Matrix Factorization)
- **Filtrage basé sur le contenu** (analyse de métadonnées, genres, acteurs)

L'objectif est de fournir des recommandations précises tout en gérant efficacement le problème du cold start et la sparsité des données.

---

## ✨ Fonctionnalités

- ✅ **Recommandations personnalisées** basées sur l'historique utilisateur
- ✅ **Filtrage hybride** combinant plusieurs algorithmes
- ✅ **API REST** documentée avec FastAPI
- ✅ **Interface utilisateur moderne** en React.js
- ✅ **Base de données relationnelle** PostgreSQL
- ✅ **Système de notation** et retours utilisateurs
- ✅ **Recherche et filtrage** par genre, année, popularité
- ✅ **Gestion du cold start** pour nouveaux utilisateurs

---

## 🏗️ Architecture
```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│   Frontend  │ ───> │   REST API   │ ───> │  Database   │
│  (React.js) │ <─── │  (FastAPI)   │ <─── │ (PostgreSQL)│
└─────────────┘      └──────────────┘      └─────────────┘
                             │
                             ↓
                    ┌──────────────────┐
                    │  ML Models       │
                    │  - SVD           │
                    │  - Content-Based │
                    │  - Hybrid        │
                    └──────────────────┘
```

---

## 🔧 Technologies

### **Backend**
- **Python 3.9+**
- **FastAPI** - Framework web moderne et rapide
- **pandas** - Manipulation de données
- **scikit-surprise** - Algorithmes de recommandation
- **scikit-learn** - Machine Learning
- **SQLAlchemy** - ORM pour PostgreSQL
- **Pydantic** - Validation de données

### **Frontend**
- **React.js** - Interface utilisateur
- **Axios** - Requêtes HTTP
- **Material-UI** - Composants UI

### **Database**
- **PostgreSQL** - Base de données relationnelle

### **DevOps**
- **Docker** - Containerisation
- **pytest** - Tests unitaires

---

## 📊 Dataset

**Source :** [MovieLens 25M Dataset](https://grouplens.org/datasets/movielens/25m/)

- **25 millions de ratings**
- **62,000 films**
- **162,000 utilisateurs**
- **Période :** 1995 - 2019

### Structure des données :
```
movies.csv
  - movieId, title, genres

ratings.csv
  - userId, movieId, rating, timestamp

tags.csv
  - userId, movieId, tag, timestamp
```

---

## 🧠 Algorithmes

### 1. **Filtrage Collaboratif (Collaborative Filtering)**

#### **SVD (Singular Value Decomposition)**
Décompose la matrice user-item en matrices de plus faible dimension :
```
R ≈ U × Σ × V^T
```

**Paramètres optimisés :**
- Facteurs latents : 100
- Régularisation : λ = 0.02
- Learning rate : 0.005
- Epochs : 20

#### **Matrix Factorization**
Prédit les ratings manquants en minimisant l'erreur quadratique :
```
min Σ (r_ui - q_i^T p_u)² + λ(||q_i||² + ||p_u||²)
```

### 2. **Filtrage Basé sur le Contenu (Content-Based)**

- **TF-IDF** sur les genres et tags
- **Similarité cosinus** entre films
- Analyse des métadonnées (réalisateurs, acteurs, année)

### 3. **Approche Hybride**

Combine les deux approches avec pondération :
```
score_final = α × score_collaborative + (1-α) × score_content
```

Avec α = 0.7 (déterminé par validation croisée)

---

## 📈 Résultats

### **Métriques d'évaluation**

| Algorithme | RMSE | MAE | Precision@10 | Recall@10 |
|------------|------|-----|--------------|-----------|
| SVD | 0.87 | 0.67 | 0.76 | 0.42 |
| Content-Based | 0.95 | 0.73 | 0.68 | 0.38 |
| **Hybrid** | **0.84** | **0.65** | **0.79** | **0.45** |

### **Analyse**
- ✅ Le modèle hybride améliore de **3.5%** les performances vs SVD seul
- ✅ Meilleure gestion du cold start grâce au contenu
- ✅ Temps de réponse API : < 200ms pour 10 recommandations

---

## 🚀 Installation

### **Prérequis**
- Python 3.9+
- PostgreSQL 13+
- Node.js 16+ (pour le frontend)

### **Étape 1 : Cloner le repository**
```bash
git clone https://github.com/NaoufalBgit/movie-recommendation-system.git
cd movie-recommendation-system
```

### **Étape 2 : Installer les dépendances backend**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### **Étape 3 : Configurer la base de données**
```bash
# Créer la base de données PostgreSQL
createdb movie_recommender

# Configurer les variables d'environnement
cp .env.example .env
# Éditer .env avec vos credentials
```

### **Étape 4 : Charger les données**
```bash
python scripts/load_data.py
```

### **Étape 5 : Entraîner les modèles**
```bash
python scripts/train_models.py
```

### **Étape 6 : Lancer le backend**
```bash
uvicorn app.main:app --reload
```

### **Étape 7 : Installer et lancer le frontend**
```bash
cd ../frontend
npm install
npm start
```

L'application sera accessible à `http://localhost:3000`

---

## 💻 Utilisation

### **API Endpoints**

#### Obtenir des recommandations
```bash
GET /api/recommendations/{user_id}?n=10
```

**Exemple de réponse :**
```json
{
  "user_id": 123,
  "recommendations": [
    {
      "movie_id": 456,
      "title": "Inception",
      "predicted_rating": 4.5,
      "genres": ["Action", "Sci-Fi"],
      "confidence": 0.87
    }
  ]
}
```

#### Ajouter un rating
```bash
POST /api/ratings
Content-Type: application/json

{
  "user_id": 123,
  "movie_id": 456,
  "rating": 5.0
}
```

**Documentation complète de l'API :** `http://localhost:8000/docs`

---

## 📁 Structure du projet
```
movie-recommendation-system/
│
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py              # Point d'entrée FastAPI
│   │   ├── models.py            # Modèles SQLAlchemy
│   │   ├── schemas.py           # Schémas Pydantic
│   │   ├── database.py          # Configuration DB
│   │   └── routers/
│   │       ├── recommendations.py
│   │       └── ratings.py
│   ├── ml/
│   │   ├── collaborative.py     # Filtrage collaboratif
│   │   ├── content_based.py     # Filtrage par contenu
│   │   ├── hybrid.py            # Modèle hybride
│   │   └── utils.py
│   ├── scripts/
│   │   ├── load_data.py
│   │   └── train_models.py
│   ├── tests/
│   ├── requirements.txt
│   └── .env.example
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── App.js
│   ├── public/
│   └── package.json
│
├── data/                        # Données MovieLens
│   ├── movies.csv
│   ├── ratings.csv
│   └── tags.csv
│
├── models/                      # Modèles entraînés
│   ├── svd_model.pkl
│   ├── content_model.pkl
│   └── hybrid_model.pkl
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_collaborative_filtering.ipynb
│   ├── 03_content_based.ipynb
│   └── 04_evaluation.ipynb
│
├── assets/                      # Images pour README
├── .gitignore
└── README.md
```

---

## 🗺️ Roadmap

### **Phase actuelle : MVP** ✅
- [x] Implémentation algorithmes de base
- [x] API REST fonctionnelle
- [x] Interface utilisateur basique
- [x] Déploiement local

### **Phase suivante : Améliorations**
- [ ] Intégration d'embeddings de films (Word2Vec)
- [ ] Deep Learning (Neural Collaborative Filtering)
- [ ] Système d'explications des recommandations
- [ ] A/B testing framework
- [ ] Déploiement en production (AWS/GCP)
- [ ] Support multi-langues
- [ ] Application mobile

---

## 👤 Auteur

**Naoufal Benamar**

- 🎓 Ingénieur en Informatique - Sup Galilée (Université Sorbonne Paris Nord)
- 💼 LinkedIn: [linkedin.com/in/naoufal-benamar-97217b1a4](https://www.linkedin.com/in/naoufal-benamar-97217b1a4/)
- 🐙 GitHub: [@NaoufalBgit](https://github.com/NaoufalBgit)
- 📧 Email: benamarnaoufal@gmail.com

---

## 🙏 Remerciements

- [MovieLens](https://grouplens.org/datasets/movielens/) pour le dataset
- [Surprise](http://surpriselib.com/) pour les algorithmes de recommandation
- [FastAPI](https://fastapi.tiangolo.com/) pour le framework backend

---

## 📚 Références

1. Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems.
2. Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender systems handbook.
3. Aggarwal, C. C. (2016). Recommender systems: The textbook.

---

<p align="center">
  ⭐ Si ce projet vous plaît, n'hésitez pas à lui donner une étoile !
</p>
