## Wichtige SQL-Befehle

|   **Befehl**       |  **Beschreibung**                                                      |   **Beispiel** |
|--------------------|------------------------------------------------------------------------|-----------------|
| `SELECT`           | Daten aus einer Tabelle abfragen                                       | `SELECT * FROM kunden;` |
| `WHERE`            | Bedingung für die Abfrage setzen                                       | `SELECT * FROM kunden WHERE land = 'Deutschland';` |
| `INSERT INTO`      | Neue Daten in eine Tabelle einfügen                                    | `INSERT INTO kunden (name, land) VALUES ('Anna', 'Italien');` |
| `UPDATE`           | Bestehende Daten verändern                                              | `UPDATE kunden SET land = 'Spanien' WHERE name = 'Anna';` |
| `DELETE`           | Daten löschen                                                          | `DELETE FROM kunden WHERE name = 'Anna';` |
| `CREATE TABLE`     | Neue Tabelle erstellen                                                  | `CREATE TABLE kunden (id INT PRIMARY KEY, name VARCHAR(50));` |
| `DROP TABLE`       | Tabelle löschen                                                        | `DROP TABLE kunden;` |
| `ALTER TABLE`      | Tabelle verändern (z. B. Spalte hinzufügen)                            | `ALTER TABLE kunden ADD geburtstag DATE;` |
| `JOIN`             | Tabellen über gemeinsame Spalten verbinden                             | `SELECT * FROM bestellungen JOIN kunden ON kunden.id = bestellungen.kunden_id;` |
| `GROUP BY`         | Ergebnisse gruppieren                                                   | `SELECT land, COUNT(*) FROM kunden GROUP BY land;` |
| `HAVING`           | Bedingung für Gruppenergebnisse                                         | `SELECT land, COUNT(*) FROM kunden GROUP BY land HAVING COUNT(*) > 5;` |
| `ORDER BY`         | Ergebnisse sortieren                                                    | `SELECT * FROM kunden ORDER BY name ASC;` |
| `LIMIT`            | Anzahl der zurückgegebenen Zeilen begrenzen                            | `SELECT * FROM kunden LIMIT 10;` |
| `DISTINCT`         | Doppelte Einträge vermeiden                                             | `SELECT DISTINCT land FROM kunden;` |
| `IN`               | Abfrage, ob ein Wert in einer Liste vorkommt                           | `SELECT * FROM kunden WHERE land IN ('Deutschland', 'Frankreich');` |
| `BETWEEN`          | Werte in einem bestimmten Bereich abfragen                             | `SELECT * FROM kunden WHERE id BETWEEN 10 AND 20;` |
| `LIKE`             | Textsuche mit Wildcards                                                 | `SELECT * FROM kunden WHERE name LIKE 'A%';` |
| `IS NULL`          | Überprüfung auf NULL-Werte                                              | `SELECT * FROM kunden WHERE geburtstag IS NULL;` |
| `UNION`            | Ergebnisse aus mehreren Abfragen kombinieren                            | `SELECT name FROM kunden UNION SELECT name FROM lieferanten;` |
| `CASE`             | Wenn-Dann-Logik in Abfragen                                             | `SELECT name, CASE WHEN land = 'DE' THEN 'Deutsch' ELSE 'Andere' END FROM kunden;` |


# SQL mit C#-Anwendung

### SQL-Befehle

#### `SELECT`
**Beschreibung:** Daten aus einer Tabelle abfragen
```sql
SELECT * FROM kunden;
SELECT name, land FROM kunden;
```
```csharp
// SQL-Befehl definieren
var command = new SqlCommand("SELECT name, land FROM kunden", connection);

// Befehl ausführen
var reader = command.ExecuteReader();

// Ergebnisse verarbeiten
while (reader.Read())
{
    Console.WriteLine($"{reader["name"]} aus {reader["land"]}");
}
```

#### `WHERE`
**Beschreibung:** Bedingungen in Abfragen setzen
```sql
SELECT * FROM kunden WHERE land = 'Deutschland';
```
```csharp
var command = new SqlCommand("SELECT * FROM kunden WHERE land = @land", connection);
command.Parameters.AddWithValue("@land", "Deutschland");
var reader = command.ExecuteReader();
```

#### `INSERT INTO`
**Beschreibung:** Neue Daten in eine Tabelle einfügen
```sql
INSERT INTO kunden (name, land) VALUES ('Anna', 'Italien');
```
```csharp
var command = new SqlCommand("INSERT INTO kunden (name, land) VALUES (@name, @land)", connection);
command.Parameters.AddWithValue("@name", "Anna");
command.Parameters.AddWithValue("@land", "Italien");
command.ExecuteNonQuery();
```

#### `UPDATE`
**Beschreibung:** Daten in einer Tabelle aktualisieren
```sql
UPDATE kunden SET land = 'Spanien' WHERE name = 'Anna';
```
```csharp
var command = new SqlCommand("UPDATE kunden SET land = @land WHERE name = @name", connection);
command.Parameters.AddWithValue("@land", "Spanien");
command.Parameters.AddWithValue("@name", "Anna");
command.ExecuteNonQuery();
```

#### `DELETE`
**Beschreibung:** Daten löschen
```sql
DELETE FROM kunden WHERE name = 'Anna';
```
```csharp
var command = new SqlCommand("DELETE FROM kunden WHERE name = @name", connection);
command.Parameters.AddWithValue("@name", "Anna");
command.ExecuteNonQuery();
```

#### `JOIN`
**Beschreibung:** Tabellen über Spalten verbinden
```sql
SELECT * FROM bestellungen JOIN kunden ON kunden.id = bestellungen.kunden_id;
```
```csharp
var command = new SqlCommand("SELECT * FROM bestellungen JOIN kunden ON kunden.id = bestellungen.kunden_id", connection);
var reader = command.ExecuteReader();
```

#### `ORDER BY`
**Beschreibung:** Ergebnisse sortieren
```sql
SELECT * FROM kunden ORDER BY name ASC;
```
```csharp
var command = new SqlCommand("SELECT * FROM kunden ORDER BY name ASC", connection);
var reader = command.ExecuteReader();
```

#### `GROUP BY` & `HAVING`
**Beschreibung:** Ergebnisse gruppieren & filtern
```sql
SELECT land, COUNT(*) FROM kunden GROUP BY land;
SELECT land, COUNT(*) FROM kunden GROUP BY land HAVING COUNT(*) > 5;
```
```csharp
var command = new SqlCommand("SELECT land, COUNT(*) FROM kunden GROUP BY land HAVING COUNT(*) > 5", connection);
var reader = command.ExecuteReader();
while (reader.Read())
{
    Console.WriteLine($"{reader["land"]}: {reader["COUNT(*)"]} Kunden");
}
```

#### `IN`, `BETWEEN`, `LIKE`, `IS NULL`
```sql
SELECT * FROM kunden WHERE land IN ('Deutschland', 'Frankreich');
SELECT * FROM kunden WHERE id BETWEEN 10 AND 20;
SELECT * FROM kunden WHERE name LIKE 'A%';
SELECT * FROM kunden WHERE geburtstag IS NULL;
```
```csharp
var command = new SqlCommand("SELECT * FROM kunden WHERE geburtstag IS NULL", connection);
var reader = command.ExecuteReader();
```

#### `DISTINCT`
```sql
SELECT DISTINCT land FROM kunden;
```
```csharp
var command = new SqlCommand("SELECT DISTINCT land FROM kunden", connection);
var reader = command.ExecuteReader();
```

#### `LIMIT`
```sql
SELECT * FROM kunden LIMIT 10;
```
```csharp
var command = new SqlCommand("SELECT TOP 10 * FROM kunden", connection); // für SQL Server
var reader = command.ExecuteReader();
```

#### `CASE`
```sql
SELECT name, CASE WHEN land = 'DE' THEN 'Deutsch' ELSE 'Andere' END FROM kunden;
```
```csharp
var command = new SqlCommand("SELECT name, CASE WHEN land = 'DE' THEN 'Deutsch' ELSE 'Andere' END AS herkunft FROM kunden", connection);
var reader = command.ExecuteReader();
while (reader.Read())
{
    Console.WriteLine($"{reader["name"]} – {reader["herkunft"]}");
}
```

#### `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`
```sql
CREATE TABLE kunden (id INT PRIMARY KEY, name VARCHAR(50));
ALTER TABLE kunden ADD geburtstag DATE;
DROP TABLE kunden;
```
```csharp
var createCommand = new SqlCommand("CREATE TABLE kunden (id INT PRIMARY KEY, name VARCHAR(50))", connection);
createCommand.ExecuteNonQuery();
```

#### `UNION`
```sql
SELECT name FROM kunden UNION SELECT name FROM lieferanten;
```
```csharp
var command = new SqlCommand("SELECT name FROM kunden UNION SELECT name FROM lieferanten", connection);
var reader = command.ExecuteReader();
while (reader.Read())
{
    Console.WriteLine(reader["name"].ToString());
}
```

---

### 🔧 Erweiterte SQL-Konzepte

#### Datentypen
```sql
CREATE TABLE beispiel (
  id INT,
  name VARCHAR(100),
  aktiv BOOLEAN,
  geburtstag DATE,
  kontostand DECIMAL(10,2)
);
```

#### Primär- & Fremdschlüssel
```sql
CREATE TABLE kunden (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);
CREATE TABLE bestellungen (
  id INT PRIMARY KEY,
  kunden_id INT,
  FOREIGN KEY (kunden_id) REFERENCES kunden(id)
);
```

#### Constraints
```sql
CREATE TABLE benutzer (
  id INT PRIMARY KEY,
  username VARCHAR(100) UNIQUE NOT NULL,
  alter INT CHECK (alter >= 18),
  erstellt_am DATE DEFAULT CURRENT_DATE
);
```

#### Indexes
```sql
CREATE INDEX idx_name ON kunden(name);
```

#### Views
```sql
CREATE VIEW aktive_kunden AS
SELECT * FROM kunden WHERE aktiv = TRUE;
```

#### Stored Procedures (Beispiel für MySQL)
```sql
DELIMITER //
CREATE PROCEDURE HoleKunden()
BEGIN
  SELECT * FROM kunden;
END //
DELIMITER ;
```

#### Transactions
```sql
START TRANSACTION;
UPDATE konto SET saldo = saldo - 100 WHERE id = 1;
UPDATE konto SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
```

#### Normalization (Beispielhafte Trennung)
```sql
-- Statt name, stadt, land in einer Tabelle → getrennte Tabellen:
CREATE TABLE land (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE stadt (id INT PRIMARY KEY, name VARCHAR(100), land_id INT);
CREATE TABLE kunde (id INT PRIMARY KEY, name VARCHAR(100), stadt_id INT);
```

#### UNION / INTERSECT / EXCEPT
```sql
SELECT name FROM kunden
UNION
SELECT name FROM lieferanten;

SELECT name FROM kunden
INTERSECT
SELECT name FROM lieferanten;

SELECT name FROM kunden
EXCEPT
SELECT name FROM lieferanten;
```

#### Subqueries
```sql
SELECT name FROM kunden WHERE id IN (
  SELECT kunden_id FROM bestellungen WHERE betrag > 100
);
```

#### Window Functions
```sql
SELECT name, RANK() OVER (ORDER BY punkte DESC) FROM teilnehmer;
```

#### CTEs (WITH-Klausel)
```sql
WITH kunden_anzahl AS (
  SELECT land, COUNT(*) AS anzahl FROM kunden GROUP BY land
)
SELECT * FROM kunden_anzahl WHERE anzahl > 5;
```

#### JSON / XML Unterstützung (Beispiel PostgreSQL)
```sql
SELECT info->>'email' AS email FROM benutzer WHERE info->>'aktiv' = 'true';
```

#### Time-basierte Abfragen
```sql
SELECT * FROM bestellungen WHERE bestelldatum >= NOW() - INTERVAL '7 days';
```

---

## SQL Aggregatfunktionen Übersicht

Diese Funktionen werden typischerweise mit `GROUP BY` verwendet, um Daten zu gruppieren und auszuwerten. Sie fassen mehrere Zeilen zu einer einzigen Auswertung pro Gruppe zusammen.

---

| Funktion    | Beschreibung                                                                 | Beispielanwendung                                   | Wann anwenden                                                                 |
|-------------|-------------------------------------------------------------------------------|------------------------------------------------------|--------------------------------------------------------------------------------|
| `SUM()`     | Addiert alle Werte einer Spalte                                              | `SELECT SUM(menge) FROM bestellungen;`              | Wenn du die Gesamtsumme z. B. von Bestellmengen, Preisen etc. brauchst         |
| `AVG()`     | Berechnet den Durchschnitt aller Werte in einer Spalte                      | `SELECT AVG(preis) FROM produkte;`                  | Wenn du wissen willst, wie hoch z. B. der Durchschnittspreis ist               |
| `COUNT()`   | Zählt, wie viele Zeilen vorhanden sind (oder wie viele Werte in einer Spalte)| `SELECT COUNT(*) FROM kunden;`                     | Wenn du wissen willst, wie viele Einträge es gibt – mit oder ohne Filter      |
| `MIN()`     | Gibt den kleinsten Wert in einer Spalte zurück                              | `SELECT MIN(preis) FROM produkte;`                  | Wenn du den niedrigsten Preis, das früheste Datum etc. brauchst                |
| `MAX()`     | Gibt den größten Wert in einer Spalte zurück                                | `SELECT MAX(preis) FROM produkte;`                  | Wenn du den höchsten Preis, das späteste Datum etc. brauchst                   |
| `COUNT(DISTINCT)` | Zählt eindeutige Werte in einer Spalte                                 | `SELECT COUNT(DISTINCT kunde_id) FROM bestellungen;`| Wenn du wissen willst, wie viele verschiedene Kunden bestellt haben            |

---

## Beispiel mit `GROUP BY`

```sql
SELECT kunde, SUM(menge) AS gesamt_menge
FROM bestellungen
GROUP BY kunde;
```

⟶ Gibt die Gesamtmenge pro Kunde zurück.

---

## Hinweise
- Aggregatfunktionen ignorieren `NULL`, außer bei `COUNT(*)`
- `GROUP BY` kommt **nach** `WHERE`, aber **vor** `ORDER BY`
- Du kannst mehrere Aggregatfunktionen gleichzeitig nutzen:

```sql
SELECT kunde,
       COUNT(*) AS anzahl_bestellungen,
       SUM(menge) AS gesamt,
       AVG(menge) AS durchschnitt
FROM bestellungen
GROUP BY kunde;
```

---
