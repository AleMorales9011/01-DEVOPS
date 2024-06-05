# Design, modelling and deployment of a MySql Database.

10 minutes

For the vast majority of SAAS products, a database is essential for storing, managing, and utilizing data effectively to deliver a valuable service to their users. 

## 1. Connecting to a MySQL database via CLI

```ruby
mysql -u root -p
```

## 2. Exploring Databases

```ruby
SHOW DATABASES;
```

## Modeling and designing a database with MySql Workbench

## Deploying a database via Script

```sql
DROP DATABASE Loja;
CREATE DATABASE Loja;

USE Loja;

CREATE TABLE Municipio (
    ID_Municipio INT NOT NULL AUTO_INCREMENT,
    Nome VARCHAR(80) NOT NULL,
    CodIBGE INT NOT NULL,
    PRIMARY KEY (ID_Municipio)
);

CREATE TABLE Estado (
ID_Estado INT NOT NULL AUTO_INCREMENT,
Nome VARCHAR(50) NOT NULL,
UF CHAR(2) NOT NULL,
PRIMARY KEY (ID_Estado)
);

CREATE TABLE Cliente (
ID_Cliente INT NOT NULL AUTO_INCREMENT,
Nome VARCHAR(80) NOT NULL,
CPF CHAR(11) NOT NULL,
Celular CHAR(11) NOT NULL,
EndLogradouro VARCHAR(100) NOT NULL,
EndNumero VARCHAR(10) DEFAULT NULL,
EndMunicipio INT NOT NULL,
EndCEP CHAR(8) NOT NULL,
Estado_ID INT NOT NULL,
PRIMARY KEY (ID_Cliente),
KEY fk_Cliente_Municipio1_idx (EndMunicipio),
CONSTRAINT fk_Cliente_Municipio1_idx FOREIGN KEY (EndMunicipio) REFERENCES Municipio (ID_Municipio)
);

CREATE TABLE ContaReceber (
  ID_Conta INT NOT NULL AUTO_INCREMENT,
  Cliente_ID INT NOT NULL,
  FaturaVenda_ID INT NOT NULL,
  DataConta DATE NOT NULL,
  DataVencimento DATE NOT NULL,
  Valor DECIMAL(18,2) NOT NULL,
  Situacao ENUM('1', '2', '3') NOT NULL,
  PRIMARY KEY (ID_Conta),
  KEY fk_ContaReceber_Cliente_idx (Cliente_ID),
  CONSTRAINT fk_ContaReceber_Cliente_idx FOREIGN KEY (Cliente_ID) REFERENCES Cliente (ID_Cliente)
);
```

##  Inserting data via Script

```sql
USE Loja;
INSERT INTO Estado (Nome, UF)
VALUES ('São Paulo', 'SP'),
       ('Minas Gerais', 'MG'),
       ('Rio de Janeiro', 'RJ'),
       ('Bahia', 'BA'),
       ('Paraná', 'PR');

INSERT INTO Municipio (Nome, CodIBGE, ID_Municipio)
VALUES ('São Paulo', 3550308, (SELECT ID_Estado FROM Estado WHERE UF = 'SP')),
       ('Belo Horizonte', 3106400, (SELECT ID_Estado FROM Estado WHERE UF = 'MG')),
       ('Rio de Janeiro', 3300500, (SELECT ID_Estado FROM Estado WHERE UF = 'RJ')),
       ('Salvador', 2927408, (SELECT ID_Estado FROM Estado WHERE UF = 'BA')),
       ('Curitiba', 4106900, (SELECT ID_Estado FROM Estado WHERE UF = 'PR'));
       
INSERT INTO Cliente (Nome, CPF, Celular, EndLogradouro, EndNumero, EndMunicipio, EndCEP, Estado_ID)
VALUES ('Fulano da Silva', '12345678900', '1199887766', 'Rua das Flores', '123', (SELECT ID_Municipio FROM Municipio WHERE Nome = 'São Paulo'), '01001000', (SELECT ID_Estado FROM Estado WHERE UF = 'SP')),
       ('Beltrana Santos', '98765432100', '2199554433', 'Avenida Paulista', '500', (SELECT ID_Municipio FROM Municipio WHERE Nome = 'Rio de Janeiro'), '20730000', (SELECT ID_Estado FROM Estado WHERE UF = 'RJ')),
       ('Ciclano Pereira', '00011122233', '3198443322', 'Rua da Bahia', '789', (SELECT ID_Municipio FROM Municipio WHERE Nome = 'Salvador'), '40000000', (SELECT ID_Estado FROM Estado WHERE UF = 'BA'));

INSERT INTO ContaReceber (Cliente_ID, FaturaVenda_ID, DataConta, DataVencimento, Valor, Situacao)
VALUES ((SELECT ID_Cliente FROM Cliente WHERE Nome = 'Fulano da Silva'), 1, '2024-06-01', '2024-06-30', 1000.00, '1'), 
       ((SELECT ID_Cliente FROM Cliente WHERE Nome = 'Beltrana Santos'), 2, '2024-06-02', '2024-07-15', 500.50, '2'),
       ((SELECT ID_Cliente FROM Cliente WHERE Nome = 'Ciclano Pereira'), 3, '2024-06-03', '2024-07-31', 2500.75, '3');

```






