# **Proiect de bază de date pentru:** MagazinElectronice
Scopul acestui proiect este de a utiliza toate cunoștințele SQL acumulate în cadrul cursului de Testare a Software-ului și de a le pune în practică.

### **Aplicația supusă testării:** MagazinElectronice

### **Instrumentele utilizate:** MySQL Workbench

### **Descrierea bazei de date:
Aceasta este o bază de date pentru gestionarea unui magazin online de electronice. Baza de date va include tabele pentru produse, clienți, comenzi și detalii ale comenzilor. Toate componentele necesare, cum ar fi DDL (Data Definition Language), DML (Data Manipulation Language) și DQL (Data Query Language) sunt incluse, fiecare asociată cu un scenariu specific.

### **Schema bazei de date**

Tabelele sunt conectate în următorul mod:

'Comenzi' este conectat cu 'Clienti' printr-o relație de tipul ‘many-to-one’, care a fost implementată prin 'IDComanda’ drept cheie primară și 'IDClient’ drept cheie străină.
'DetaliiComanda' este conectat cu 'Comenzi' printr-o relație de tipul ‘many-to-one’, care a fost implementată prin 'IDDetaliuComanda’ drept cheie primară și 'IDComanda’ drept cheie străină. 'DetaliiComanda' este, de asemenea, conectat cu 'Produse' printr-o relație de tipul ”many-to-one", care a fost implementată prin 'IDDetaliuComanda’ drept cheie primară și 'IDProdus’ drept cheie străină.

### **DDL (Data Definition Language)**

Următoarele instrucțiuni au fost scrise în scopul CREĂRII structurii bazei de date. 
- CREATE TABLE Produse (
    IDProdus INT PRIMARY KEY AUTO_INCREMENT,
    NumeProdus VARCHAR(100),
    Categoria VARCHAR(50),
    Pret DECIMAL(10, 2),
    Stoc INT
);
- CREATE TABLE Clienti (
    IDClient INT PRIMARY KEY AUTO_INCREMENT,
    Prenume VARCHAR(100),
    Nume VARCHAR(100),
    Email VARCHAR(100) UNIQUE,
    Telefon VARCHAR(15)
);
- CREATE TABLE Clienti (
    IDClient INT PRIMARY KEY AUTO_INCREMENT,
    Prenume VARCHAR(100),
    Nume VARCHAR(100),
    Email VARCHAR(100) UNIQUE,
    Telefon VARCHAR(15)
);
- CREATE TABLE DetaliiComanda (
    IDDetaliuComanda INT PRIMARY KEY AUTO_INCREMENT,
    IDComanda INT,
    IDProdus INT,
    Cantitate INT,
    FOREIGN KEY (IDComanda) REFERENCES Comenzi(IDComanda),
    FOREIGN KEY (IDProdus) REFERENCES Produse(IDProdus)
);


După ce baza de date și tabelele au fost create, au fost scrise câteva instrucțiuni ALTER, DROP și TRUNCATE pentru a actualiza structura bazei de date, așa cum este descris mai jos:
- ALTER TABLE Clienti
ADD Adresa VARCHAR(255);
- ALTER TABLE Produse
DROP COLUMN Stoc;
- DROP TABLE DetaliiComanda;
- TRUNCATE TABLE Produse;


### **DML (Data Manipulation Language)**
Pentru a putea utiliza baza de date, am populat tabelele cu diverse date necesare pentru a efectua interogări și a manipula datele. În procesul de testare, aceste date necesare sunt identificate în faza de Design al Testelor și create în faza de Implementare a Testelor. Mai jos găsiți toate instrucțiunile de inserare care au fost create în scopul acestui proiect:
- INSERT INTO Produse (NumeProdus, Categoria, Pret)
VALUES 
    ('Laptop', 'Computere', 1200.00),
    ('Smartphone', 'Telefoane', 800.00);
- INSERT INTO Produse (NumeProdus, Categoria, Pret)
VALUES 
    ('Laptop', 'Computere', 1200.00),
    ('Smartphone', 'Telefoane', 800.00);
- INSERT INTO Comenzi (DataComanda, IDClient)
VALUES 
    ('2024-07-20', 1),
    ('2024-08-29', 2);

După inserare, pentru a pregăti datele astfel încât să fie mai bine adaptate pentru procesul de testare, am actualizat unele date în felul următor:
- UPDATE Produse 
SET Pret = 1100.00 
WHERE IDProdus = 1;

După procesul de testare, am șters datele care nu mai erau relevante pentru a menține baza de date curată:
- DELETE FROM Comenzi WHERE IDComanda = 1;

### **DQL (Data Query Language)**

Pentru a simula diverse scenarii care ar putea apărea în viața reală, am creat următoarele interogări care acoperă multiple situații potențiale din viața reală:
- SELECT * FROM Clienti;
- SELECT NumeProdus, Pret FROM Produse;
- SELECT * FROM Comenzi 
WHERE DataComanda > '2024-07-20';
- SELECT * FROM Clienti 
WHERE Email LIKE '%andrei%';
- SELECT * FROM Clienti 
WHERE Adresa LIKE '%Principală%' OR Prenume = 'Ioana';
- SELECT * FROM Clienti 
WHERE Adresa LIKE '%Principală%' AND Prenume = 'Andrei';
- SELECT * FROM Produse 
WHERE Categoria NOT LIKE 'Telefoane';
-SELECT Clienti.Prenume, Clienti.Nume, Comenzi.DataComanda
FROM Clienti
INNER JOIN Comenzi ON Clienti.IDClient = Comenzi.IDClient;
- SELECT Produse.NumeProdus, DetaliiComanda.Cantitate
FROM Produse
LEFT JOIN DetaliiComanda ON Produse.IDProdus = DetaliiComanda.IDProdus;
- SELECT Comenzi.DataComanda, DetaliiComanda.Cantitate
FROM Comenzi
RIGHT JOIN DetaliiComanda ON Comenzi.IDComanda = DetaliiComanda.IDComanda;
- SELECT Clienti.Prenume, Produse.NumeProdus
FROM Clienti
CROSS JOIN Produse;
- SELECT * FROM Clienti 
WHERE IDClient IN (
    SELECT IDClient 
    FROM Comenzi 
    GROUP BY IDClient 
    HAVING COUNT(*) > 1
);

### **Concluzie**
În acest proiect, am dezvoltat o bază de date cuprinzătoare pentru gestionarea unui magazin online de electronice. Pe parcursul acestui proces, am acoperit aspecte esențiale ale managementului bazei de date, inclusiv crearea și modificarea tabelelor (DDL), inserarea, ștergerea și actualizarea datelor (DML) și interogarea datelor (DQL). De asemenea, am explorat utilizarea cheilor primare și a cheilor străine pentru a stabili relații între tabele, asigurând integritatea datelor și facilitând managementul eficient al acestora.
Am învățat cum să proiectez o bază de date care să reprezinte eficient entitățile din lumea reală, cum ar fi produsele, clienții și comenzile. Înțelegerea relațiilor dintre aceste entități a fost crucială pentru crearea unei baze de date bine structurate.
Proiectul mi-a oferit oportunitatea de a aplica cunoștințele teoretice despre baze de date într-un context practic.
