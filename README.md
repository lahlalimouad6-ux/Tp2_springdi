# TP2 — Injection de dépendances avec Spring (Spring DI)

Projet Maven illustrant l'**injection de dépendances (Dependency Injection)** avec **Spring Context** et les annotations, sur une architecture en couches classique `DAO → Métier → Présentation`.

## 🎯 Objectifs

- Configurer un conteneur Spring par annotations (`@Configuration`, `@ComponentScan`).
- Déclarer des beans avec `@Component`.
- Réaliser une injection de dépendance par champ (`@Autowired`).
- Résoudre les ambiguïtés entre plusieurs implémentations d'une même interface.

## 🏗️ Architecture du projet

```
src/main/java/org/example
├── dao
│   ├── IDao.java          # Interface du DAO  →  getValue()
│   ├── DaoImpl.java       # Implémentation 1 (@Component "dao")  →  retourne 100.0
│   └── DaoImpl2.java      # Implémentation 2 (@Component "dao2") →  retourne 150.0
├── metier
│   ├── IMetier.java       # Interface métier →  calcul()
│   └── MetierImpl.java    # @Component "metier", @Autowired IDao → calcul() = dao.getValue() * 2
└── presentation
    └── Presentation2.java # @Configuration + @ComponentScan, méthode main()
```

## 🔧 Technologies utilisées

| Technologie    | Version |
|----------------|---------|
| Java           | 17+     |
| Spring Context | 6.1.2   |
| JUnit          | 4.13.2  |
| Maven          | 3.8+    |

## ⚙️ Fonctionnement

1. `Presentation2` est annotée `@Configuration` et `@ComponentScan(basePackages = {"org.example.dao", "org.example.metier"})`.
2. Spring scanne ces packages et instancie les beans `dao`, `dao2` et `metier`.
3. `MetierImpl` reçoit automatiquement un bean `IDao` via `@Autowired`.
4. L'ambiguïté entre `DaoImpl` et `DaoImpl2` est résolue par **correspondance de nom** : le champ s'appelle `dao`, Spring injecte donc `DaoImpl`.

## ▶️ Exécution

```bash
# Compiler le projet
mvn clean compile

# Lancer la classe principale
# Depuis IntelliJ : bouton ▶ sur Presentation2.main
```

## ✅ Résultat attendu

![Résultat de l'exécution](docs/screenshots/resultat.png)

```text
Résultat = 200.0
```

**Explication du calcul :**

```
dao.getValue() = 100.0        (DaoImpl)
calcul()       = 100.0 * 2 = 200.0
```

> Pour utiliser `DaoImpl2` (150.0), il suffirait d'ajouter `@Qualifier("dao2")`
> sur le champ `dao` de `MetierImpl`. Le résultat serait alors `300.0`.

## 📸 Captures d'écran

| Description                    | Image                            |
|--------------------------------|----------------------------------|
| Structure du projet (IntelliJ) | `docs/screenshots/structure.png` |
| Exécution et résultat          | `docs/screenshots/resultat.png`  |

> Placez vos propres captures dans le dossier `docs/screenshots/` pour qu'elles s'affichent.

## 📄 Licence

Projet pédagogique — libre d'utilisation.
