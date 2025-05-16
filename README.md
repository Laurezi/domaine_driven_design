# DOMAINE DRIVEN DESIGN (DDD) : Entity - Value Objects - Aggregates - Repository


## Plan
#### Introduction
#### 1. Définition
#### 2. Concepts fondamentaux du DDD
#### 3. Histoire du DDD
#### 4. Concepts avancés du DDD
#### Conclusion
#### Références
***

## Introduction
Avant l’apparition du Domain-Driven Design (DDD), le développement logiciel reposait sur des approches générales comme la programmation orientée objet (POO), les modèles en couches et les architectures monolithiques. Les logiciels étaient souvent conçus autour des contraintes techniques, telles que les bases de données et les interfaces graphiques, plutôt que des besoins métier. Cependant, à mesure que les systèmes devenaient plus complexes, ces modèles montraient leurs limites. Il devenait de plus en plus difficile de représenter fidèlement les règles métier, et la confusion entre la logique applicative et la gestion des données était courante. De plus, le manque de collaboration entre experts métier et développeurs freinait la conception efficace des logiciels.
C’est dans ce contexte qu’est né le DDD comme une approche centrée sur le domaine. Dans cette présentation, nous explorerons les principes clés du DDD, son histoire, ses concepts fondamentaux comme les entités, objets valeurs, agrégats et repositories, ainsi que son impact sur le développement logiciel contemporain.

## 1. Définition
Domaine Driven Design = conception pilotée par le domaine est une approche de conception de logiciels, qui se concentre sur la modélisation de logiciels pour correspondre à un domaine en fonction des contributions des experts de ce domaine. La DDD s'oppose à l'idée d'avoir un seul modèle unifié ; au lieu de cela, elle divise un grand système en contextes délimités, chacun ayant son propre modèle.

## 2. Concepts fondamentaux du DDD
### a. Domaine
La sphère d'un métier ou activité pour laquelle on développe l'application

Ex: Fiscalité, cadastre, douane

### b. Modèle
Une abstraction qui décrit les concepts sélectionnés d'un domaine. Il capture les éléments essentiels d'un domaine. Il peut prendre plusieurs formes: diagramme UML, wiki...

Ex: le modèle **déclaration Fiscale** représenterait le processus de déclaration

### c. Contexte délimité(Bounded Context)
Il correspond à une partie distincte du domaine qui a ses propres règles et logiques. Chaque contexte délimité peut être géré séparément, avec son propre langage ubiquitaire, tout en interagissant avec les autres contextes par des interfaces bien définies. Cette approche permet de réduire la complexité tout en garantissant la cohérence.

Ex: Gestion des Contribuables, Déclaration Fiscale

### 3. Histoire du DDD
#### a. Origine
Le Domain-Driven Design, ou DDD, a été introduit par **Eric Evans** dans son livre influent **Domain-Driven Design: Tackling Complexity in the Heart of Software**, publié en **2004**. Cette approche est née de la nécessité de gérer la complexité inhérente aux logiciels d'entreprise, en particulier ceux qui ont une logique métier dense et diversifiée.
Evans a élaboré le DDD comme un ensemble de principes et de pratiques destinés à faciliter la conception et la mise en œuvre de logiciels axés sur des modèles de domaine complexes. Il a souligné l'importance d'une collaboration étroite entre les développeurs de logiciels et les experts du domaine, plaidant pour que le développement ne soit pas seulement une affaire technique, mais aussi un processus créatif et collaboratif.

#### b. Évolution et impact sur les pratiques de développement
Au fil des années, le DDD a évolué et s'est intégré à d'autres pratiques et mouvements de développement logiciel, tels que l'agilité, le développement Lean. La réflexion stratégique derrière le DDD a également inspiré de nombreuses architectures applicatives, promouvant des designs modulaires et des services autonomes.

#### c. DDD aujourd’hui
Le DDD reste une philosophie pertinente, offrant une base solide pour les entreprises qui entreprennent des transformations numériques et qui adoptent des architectures de microservices. L'approche est particulièrement valorisée dans des projets nécessitant une compréhension approfondie et une modélisation précise des processus d'affaires.

### 4. Concepts avancés du DDD
#### a. Entité (Entity)
Ce sont des objets qui possèdent une identité unique et qui peuvent évoluer au fil du temps ou encore idées pérennes dans les interactions des récits utilisateurs. Dans l'approche orientée objet, les entités seront reliées aux classes et aux objets

Ex: Contribuable (Nom, Identifiant fiscal, Catégorie de contribuable), Déclaration Fiscale (Période fiscale, Type d’impôt, Montant déclaré)

#### b. Objets valeurs (Value Objects)
Les objets valeurs sont des entités immuables dont la valeur est totalement déterminée par leurs attributs. Ils encapsulent des données liées à un concept spécifique, comme la couleur, l’adresse, le numéro de téléphone, le prix, etc. L’immutabilité est la caractéristique principale des VO, ce qui signifie que leurs valeurs ne changent jamais une fois qu’elles sont initialisées.

Ex: Statut fiscal (Régime d'imposition, Exonérations), base Imposable (Montant des revenus ou des opérations déclarées)

#### c. Agrégats (Aggregates)
Un agrégat est un ensemble cohérent d'entités et d'objets de valeur qui forment une unité logique. Il garantit l'intégrité des règles métier.

Ex: -  Dossier Fiscal (Regroupe les déclarations effectuées par le contribuable)
- Exemple d'une bibliothèque
![](https://user.oc-static.com/upload/2020/04/15/15869382467946_VISUELS_AMANDINE-21.jpg)

#### d. Repository pattern
Les repositories sont des interfaces qui permettent d'accéder aux agrégats et aux entités de manière abstraite. Ils facilitent la récupération et la persistance des objets du domaine sans exposer les détails techniques de la base de données.

Ex: Repository de Déclarations Fiscales: 
- Définition du Repository
:  l’interface du DéclarationRepository définit les opérations essentielles :

```
public interface IDéclarationRepository
{
    DéclarationFiscale GetById(Guid id);
    List<DéclarationFiscale> GetByContribuable(Guid contribuableId);
    void Save(DéclarationFiscale déclaration);
}
```


- Implémentation du Repository:
 une implémentation utilisant Entity Framework pourrait ressembler à ceci :

```
public class DéclarationRepository : IDéclarationRepository
{
    private readonly DbContext _context;

    public DéclarationRepository(DbContext context)
    {
        _context = context;
    }

    public DéclarationFiscale GetById(Guid id)
    {
        return _context.Déclarations.Find(id);
    }

    public List<DéclarationFiscale> GetByContribuable(Guid contribuableId)
    {
        return _context.Déclarations.Where(d => d.ContribuableId == contribuableId).ToList();
    }

    public void Save(DéclarationFiscale déclaration)
    {
        _context.Déclarations.Update(déclaration);
        _context.SaveChanges();
    }
}

```

**Pourquoi utiliser un repository en DDD ?** 
- **Séparation des préoccupations** : elle détache la logique métier de l'accès aux données, garantissant ainsi une conception plus propre.
- **Accès centralisé** : toute la logique d'accès aux données est regroupée au même endroit, ce qui simplifie la maintenance.
- **Testabilité **: facilite les tests unitaires en permettant de simuler l'accès aux données.
- **Flexibilité** : Le changement de sources de données devient simple, car seul le référentiel doit être modifié.
- **Cohérence** : fournit une approche d’accès aux données unifiée, améliorant la maintenabilité du code.
- **Mise en cache** : l'accès centralisé aux données signifie une mise en œuvre plus simple de la mise en cache.
- **Découplage** : l'application n'est pas liée à des technologies ou bibliothèques d'accès aux données spécifiques.
- **Abstraction de requête** : évite les requêtes spécifiques à la base de données, rendant le code plus propre.

### Conclusion
Le **Domain-Driven Design (DDD)** offre une approche puissante pour concevoir des logiciels alignés sur les besoins métier. Grâce à cette méthodologie, la complexité des systèmes est mieux maîtrisée, favorisant une collaboration étroite entre les développeurs et les experts métier. En appliquant le DDD à des domaines comme la fiscalité, il devient possible de créer des modèles robustes pour gérer les **déclarations, liquidations et paiements fiscaux**. Cette approche améliore la **clarté, la maintenabilité et l’adaptabilité des applications**, offrant une base solide pour les entreprises engagées dans la transformation numérique et les architectures modernes. En adoptant ces principes, les systèmes peuvent évoluer efficacement tout en restant fidèles aux réalités métier qu’ils doivent servir. 

### Références
[1- https://en.wikipedia.org/wiki/Domain-driven_design](https://en.wikipedia.org/wiki/Domain-driven_design)

[2- https://www.axopen.com/blog/2024/10/comprendre-le-ddd-guide-pour-les-d%C3%A9veloppeurs/#:~:text=Contexte%20D%C3%A9limit%C3%A9%20(Bounded%20Context),ses%20propres%20r%C3%A8gles%20et%20logiques.](https://www.axopen.com/blog/2024/10/comprendre-le-ddd-guide-pour-les-d%C3%A9veloppeurs/#:~:text=Contexte%20D%C3%A9limit%C3%A9%20(Bounded%20Context),ses%20propres%20r%C3%A8gles%20et%20logiques.)

[3- https://www.elao.com/glossaire/ddd](https://www.elao.com/glossaire/ddd)

[4- https://www.astree-software.fr/expert/blog-tech/value-objects-vo-les-super-heros-de-la-coherence-des-donnees-dans-le-ddd/](https://www.astree-software.fr/expert/blog-tech/value-objects-vo-les-super-heros-de-la-coherence-des-donnees-dans-le-ddd/)

[5- https://www.abrahamberg.com/blog/repository-pattern-in-ddd-bridging-the-domain-and-data-models/](https://www.abrahamberg.com/blog/repository-pattern-in-ddd-bridging-the-domain-and-data-models/)
