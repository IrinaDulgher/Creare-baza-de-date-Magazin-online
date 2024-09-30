# **Proiect de bază de date pentru:** MagazinElectronice
Scopul acestui proiect este de a utiliza toate cunoștințele SQL acumulate în cadrul cursului de *Testare a Software-ului* și de a le pune în practică.

### **Aplicația supusă testării:** MagazinElectronice

### **Instrumentele utilizate:** MySQL Workbench

### **Descrierea bazei de date:**
Aceasta este o bază de date pentru gestionarea unui magazin online de electronice. Baza de date va include tabele pentru produse, clienți, comenzi și detalii ale comenzilor. Toate componentele necesare, cum ar fi *DDL (Data Definition Language)*, *DML (Data Manipulation Language)* și *DQL (Data Query Language)* sunt incluse, fiecare asociată cu un scenariu specific.

### **Schema bazei de date**
Tabelele sunt conectate în următorul mod:
- *'Comenzi'* este conectat cu *'Clienti'* printr-o relație de tipul *‘many-to-one’*, care a fost implementată prin *'IDComanda’* drept cheie primară și *'IDClient’* drept cheie străină.
- *'DetaliiComanda'* este conectat cu *'Comenzi'* printr-o relație de tipul *‘many-to-one’*, care a fost implementată prin *'IDDetaliuComanda’* drept cheie primară și *'IDComanda’* drept cheie străină.
- *'DetaliiComanda'* este, de asemenea, conectat cu *'Produse'* printr-o relație de tipul *”many-to-one"*, care a fost implementată prin *'IDDetaliuComanda’* drept cheie primară și *'IDProdus’* drept cheie străină.


#### Următoarele instrucțiuni au fost scrise în scopul crearii structurii bazei de date.
**Crearea bazei de date pentru magazinul online**

> CREATE DATABASE MagazinElectronice;

### **DDL (Data Definition Language)**
> **Crearea tabelului *'Produse'***
> <p> CREATE TABLE Produse (
> IDProdus INT PRIMARY KEY AUTO_INCREMENT,
> <br> NumeProdus VARCHAR(100),
> <br> Categoria VARCHAR(50),
> <br> Pret DECIMAL(10, 2),
> <br> Stoc INT
> <br> );
> 
> **Crearea tabelului *'Clienti'***
> <p> CREATE TABLE Clienti (
> IDClient INT PRIMARY KEY AUTO_INCREMENT,
> <br> Prenume VARCHAR(100),
> <br> Nume VARCHAR(100),
> <br> Email VARCHAR(100) UNIQUE,
> <br> Telefon VARCHAR(15)
> <br> );
>
> **Crearea tabelului *'Comenzi'***
> <p> CREATE TABLE Comenzi (
> IDComanda INT PRIMARY KEY AUTO_INCREMENT,
> <br> DataComanda DATE,
> <br> IDClient INT,
> <br> FOREIGN KEY (IDClient)) REFERENCES Clienti(IDClient)
> );
>
> **Crearea tabelului *'DetaliiComanda'***
> <p>CREATE TABLE DetaliiComanda (
> IDDetaliuComanda INT PRIMARY KEY AUTO_INCREMENT,
> <br> IDComanda INT,
> <br> IDProdus INT,
> <br> Cantitate INT,
> <br> FOREIGN KEY (IDComanda) REFERENCES Comenzi(IDComanda),
> <br> FOREIGN KEY (IDProdus) REFERENCES Produse(IDProdus)
> <br> );

#### **După ce baza de date și tabelele au fost create, au fost scrise câteva instrucțiuni *ALTER*, *DROP* și *TRUNCATE* pentru a actualiza structura bazei de date, așa cum este descris mai jos:**

> **Adaugarea unei coloane noi in tabelul *'Clienti'***
> <br>ALTER TABLE Clienti
> <br>ADD Adresa VARCHAR(255);
>
> **Stergerea unei coloane din tabelul *'Produse'***
> <br>ALTER TABLE Produse
> <br>DROP COLUMN Stoc;
> 
> **Stergerea tabelului *'DetaliiComanda'***  
> <br>DROP TABLE DetaliiComanda;
>
> **Trunchierea tabelului *'Produse'***
> <br>TRUNCATE TABLE Produse;

### **DML (Data Manipulation Language)**
#### Pentru a putea utiliza baza de date, am populat tabelele cu diverse date necesare pentru a efectua interogări și a manipula datele. În procesul de testare, aceste date necesare sunt identificate în faza de Design al Testelor și create în faza de Implementare a Testelor. Mai jos găsiți toate instrucțiunile de inserare care au fost create în scopul acestui proiect:
> **Inserare de date in tabelul *'Produse'*** 
> <p>INSERT INTO Produse (NumeProdus, Categoria, Pret)
> <br>VALUES 
> <br>('Laptop', 'Computere', 1200.00),
> <br>('Smartphone', 'Telefoane', 800.00);
>
> **Inserare de date in tabelul *'Clienti'*** 
> <p> INSERT INTO Clienti (Prenume, Nume, Email, Telefon, Adresa)
> <br>VALUES 
> <br>('Andrei', 'Mocanu', 'andrei.mocanu@gmail.com', '0745673244', 'Strada Principală 5'),
> <br>('Ioana', 'Popescu', 'ioana.popescu@yahoo.com', '0756930751', 'Strada Plopilor 16');
>
> **Inserare de date in tabelul *'Comenzi"***
> </p>INSERT INTO Comenzi (DataComanda, IDClient)
> <br>VALUES 
> <br>('2024-07-20', 1),
> <br>('2024-08-29', 2);

#### După inserare, pentru a pregăti datele astfel încât să fie mai bine adaptate pentru procesul de testare, am actualizat unele date în felul următor:
> **Actualizarea pretului unui produs**
> <p> UPDATE Produse 
> <br>SET Pret = 1100.00 
> <br>WHERE IDProdus = 1;

#### După procesul de testare, am șters datele care nu mai erau relevante pentru a menține baza de date curată:
> **Stergerea unei comenzi**
> <p>DELETE FROM Comenzi WHERE IDComanda = 1;

### **DQL (Data Query Language)**
#### Pentru a simula diverse scenarii care ar putea apărea în viața reală, am creat următoarele interogări care acoperă multiple situații potențiale din viața reală:
> **Selectarea tuturor clientilor**
> 
> SELECT * FROM Clienti;
> 
> **Selectarea coloanelor specifice din tabelul *'Produse'***
> 
> SELECT NumeProdus, Pret FROM Produse;
>
> 
>**Filtrarea comenzilor care sunt după o dată specifică**
> 
> <p>SELECT * FROM Comenzi 
> <br>WHERE DataComanda > '2024-07-20';
> 
>
> **Filtrarea clienților al căror email conține 'andrei'**
>
> </p>SELECT * FROM Clienti 
> <br> WHERE Email LIKE '%andrei%';
> 
> <br>**Filtrarea cu AND și OR: Clienți care locuiesc pe 'Strada Principală' sau se numesc 'Ioana'**
>
> </p>SELECT * FROM Clienti 
> <br>WHERE Adresa LIKE '%Principală%' OR Prenume = 'Ioana';
> 
> <br>**Selectarea clienților care locuiesc pe 'Strada Principală' ȘI al căror prenume este 'Andrei'**
>
> </p>SELECT * FROM Clienti 
> <br>WHERE Adresa LIKE '%Principală%' AND Prenume = 'Andrei';
>
> 
> <br>**Selectarea produselor care NU sunt în categoria 'Telefoane'**
>
> </p>SELECT * FROM Produse 
> <br>WHERE Categoria NOT LIKE 'Telefoane';
>
> 
> <br>**INNER JOIN: extragerea datelor doar pentru clienții care au făcut comenzi**
>
> </p>SELECT Clienti.Prenume, Clienti.Nume, Comenzi.DataComanda
> <br>FROM Clienti
> <br>INNER JOIN Comenzi ON Clienti.IDClient = Comenzi.IDClient;
>
> 
> <br>**LEFT JOIN: extragerea tuturor produselor, chiar dacă unele nu au fost vândute**
>
> </p>SELECT Produse.NumeProdus, DetaliiComanda.Cantitate
> <br>FROM Produse
> <br>LEFT JOIN DetaliiComanda ON Produse.IDProdus = DetaliiComanda.IDProdus;
>
> 
> <br>**RIGHT JOIN: returnarea tuturor detaliilor comenzilor, inclusiv cele pentru care nu există comenzi asociate**
>
> </p>SELECT Comenzi.DataComanda, DetaliiComanda.Cantitate
> <br>FROM Comenzi
> <br>RIGHT JOIN DetaliiComanda ON Comenzi.IDComanda = DetaliiComanda.IDComanda;
>
> 
> <br>**CROSS JOIN: returnarea tuturor combinațiilor posibile între clienți și produse, fără sã fie neapărat o legãtura între ele**
>
> </p>SELECT Clienti.Prenume, Produse.NumeProdus
> <br>FROM Clienti
> <br>CROSS JOIN Produse;
>
> 
> <br>**Selectarea clienților care au plasat mai mult de o comandă**
>
> </p>SELECT * FROM Clienti 
> <br>WHERE IDClient IN (
> <br>SELECT IDClient 
> <br>FROM Comenzi 
> <br>GROUP BY IDClient 
> <br>HAVING COUNT(*) > 1
> <br>);

### **Concluzie**

În acest proiect, am dezvoltat o bază de date cuprinzătoare pentru gestionarea unui magazin online de electronice. Pe parcursul acestui proces, am acoperit aspecte esențiale ale managementului bazei de date, inclusiv *crearea și modificarea tabelelor (**DDL**)*, *inserarea, ștergerea și actualizarea datelor (**DML**)* și *interogarea datelor (**DQL**)*. De asemenea, am explorat utilizarea ***cheilor primare*** și a ***cheilor străine*** pentru a stabili relații între tabele, asigurând integritatea datelor și facilitând managementul eficient al acestora.

Am învățat cum să proiectez o bază de date care să reprezinte eficient entitățile din lumea reală, cum ar fi ***produsele***, ***clienții*** și ***comenzile***. Înțelegerea relațiilor dintre aceste entități a fost crucială pentru ***crearea unei baze de date*** bine structurate.

Proiectul mi-a oferit oportunitatea de a aplica cunoștințele teoretice despre baze de date într-un context practic.
