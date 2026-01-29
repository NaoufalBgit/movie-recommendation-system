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

