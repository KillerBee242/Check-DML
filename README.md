# Check-DML

## Création des tables

```
-- Table des départements
CREATE TABLE Departement (
    Num_S INT PRIMARY KEY,
    Label VARCHAR(50) NOT NULL,
    Manager_Name VARCHAR(100) NOT NULL
);

-- Table des employés
CREATE TABLE Employe (
    Num_E INT PRIMARY KEY,
    Nom VARCHAR(100) NOT NULL,
    Poste VARCHAR(50) NOT NULL,
    Salaire DECIMAL(10,2) NOT NULL CHECK (Salaire > 0),
    Department_Num_S INT NOT NULL,
    FOREIGN KEY (Department_Num_S) REFERENCES Departement(Num_S) ON DELETE CASCADE
);

-- Table des projets
CREATE TABLE Projet (
    Num_P INT PRIMARY KEY,
    Titre VARCHAR(100) NOT NULL,
    Date_Debut DATE NOT NULL,
    Date_Fin DATE NOT NULL CHECK (Date_Fin >= Date_Debut),
    Department_Num_S INT NOT NULL,
    FOREIGN KEY (Department_Num_S) REFERENCES Departement(Num_S) ON DELETE CASCADE
);

-- Table des employés affectés aux projets
CREATE TABLE Employe_Projet (
    Employee_Num_E INT NOT NULL,
    Project_Num_P INT NOT NULL,
    Role VARCHAR(50) NOT NULL,
    PRIMARY KEY (Employee_Num_E, Project_Num_P),
    FOREIGN KEY (Employee_Num_E) REFERENCES Employe(Num_E) ON DELETE CASCADE,
    FOREIGN KEY (Project_Num_P) REFERENCES Projet(Num_P) ON DELETE CASCADE
);

```

Explication des contraintes
- PRIMARY KEY assure l’unicité de chaque enregistrement.
- NOT NULL garantit que les champs essentiels ne contiennent pas de valeurs vides.
- CHECK impose des restrictions sur les valeurs (ex. salaire positif, date de fin après date de début).
- FOREIGN KEY établit les relations entre les tables et permet la suppression en cascade des enregistrements liés.

## Insertion des enregistrements

```
-- Insertion des départements
INSERT INTO Departement (Num_S, Label, Manager_Name) VALUES
(1, 'IT', 'Alice Johnson'),
(2, 'RH', 'Bob Smith'),
(3, 'Marketing', 'Clara Bennett');

-- Insertion des employés
INSERT INTO Employe (Num_E, Nom, Poste, Salaire, Department_Num_S) VALUES
(101, 'John Doe', 'Développeur', 60000.00, 1),
(102, 'Jane Smith', 'Analyste', 55000.00, 2),
(103, 'Mike Brown', 'Designer', 50000.00, 3),
(104, 'Sarah Johnson', 'Data Scientist', 70000.00, 1),
(105, 'Emma Wilson', 'Spécialiste RH', 52000.00, 2);

-- Insertion des projets
INSERT INTO Projet (Num_P, Titre, Date_Debut, Date_Fin, Department_Num_S) VALUES
(201, 'Refonte du site web', '2024-01-15', '2024-06-30', 1),
(202, 'Intégration des employés', '2024-03-01', '2024-09-01', 2),
(203, 'Étude de marché', '2024-02-01', '2024-07-31', 3),
(204, 'Mise en place infrastructure IT', '2024-04-01', '2024-12-31', 1);

-- Insertion des employés dans les projets
INSERT INTO Employe_Projet (Employee_Num_E, Project_Num_P, Role) VALUES
(101, 201, 'Développeur Frontend'),
(104, 201, 'Développeur Backend'),
(102, 202, 'Formateur'),
(105, 202, 'Coordinateur'),
(103, 203, 'Chef de recherche'),
(101, 204, 'Spécialiste réseau');

```

## Mise à jour du rôle de l'employé

```
UPDATE Employe_Projet
SET Role = 'Développeur Full Stack'
WHERE Employee_Num_E = 101;
```

## Suppression de l'employé et de ses entrées

```
DELETE FROM Employe_Projet WHERE Employee_Num_E = 103;
DELETE FROM Employe WHERE Num_E = 103;
```

