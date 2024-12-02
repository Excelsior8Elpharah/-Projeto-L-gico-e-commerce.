# -Projeto-L-gico-e-commerce.
 Construindo seu Primeiro Projeto Lógico de Banco de Dados DIO Bootcamp Database Experience

Modelo Lógico (Descrição)
Baseando-se no cenário, o modelo lógico para o banco de dados de e-commerce será projetado para atender aos requisitos de negócio, com as seguintes tabelas:

Cliente

Atributos:
ID_Cliente (PK)
Nome
Tipo (ENUM: 'PJ', 'PF')
CPF_CNPJ (UNIQUE)
Email
Telefone
Endereço
Forma_Pagamento

Atributos:
ID_Pagamento (PK)
Descrição
Cliente_Pagamento (Tabela de Associação)

Atributos:
ID_Cliente (FK)
ID_Pagamento (FK)
Pedido

Atributos:
ID_Pedido (PK)
Data_Pedido
Valor_Total
ID_Cliente (FK)
Entrega

Atributos:
ID_Entrega (PK)
Status (ENUM: 'Pendente', 'Em Transporte', 'Entregue', 'Cancelada')
Codigo_Rastreio
ID_Pedido (FK)
Produto

Atributos:
ID_Produto (PK)
Nome
Descrição
Preço
ID_Fornecedor (FK)
Quantidade_Estoque
Fornecedor

Atributos:
ID_Fornecedor (PK)
Nome
Telefone
Email
Endereço
Item_Pedido

Atributos:
ID_Pedido (FK)
ID_Produto (FK)
Quantidade
Valor_Unitario

Criação das Tabelas

CREATE TABLE Cliente (
    ID_Cliente INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100),
    Tipo ENUM('PJ', 'PF'),
    CPF_CNPJ VARCHAR(20) UNIQUE,
    Email VARCHAR(100),
    Telefone VARCHAR(15),
    Endereço TEXT
);

CREATE TABLE Forma_Pagamento (
    ID_Pagamento INT AUTO_INCREMENT PRIMARY KEY,
    Descrição VARCHAR(50)
);

CREATE TABLE Cliente_Pagamento (
    ID_Cliente INT,
    ID_Pagamento INT,
    PRIMARY KEY (ID_Cliente, ID_Pagamento),
    FOREIGN KEY (ID_Cliente) REFERENCES Cliente(ID_Cliente),
    FOREIGN KEY (ID_Pagamento) REFERENCES Forma_Pagamento(ID_Pagamento)
);

CREATE TABLE Pedido (
    ID_Pedido INT AUTO_INCREMENT PRIMARY KEY,
    Data_Pedido DATETIME,
    Valor_Total DECIMAL(10,2),
    ID_Cliente INT,
    FOREIGN KEY (ID_Cliente) REFERENCES Cliente(ID_Cliente)
);

CREATE TABLE Entrega (
    ID_Entrega INT AUTO_INCREMENT PRIMARY KEY,
    Status ENUM('Pendente', 'Em Transporte', 'Entregue', 'Cancelada'),
    Codigo_Rastreio VARCHAR(50),
    ID_Pedido INT,
    FOREIGN KEY (ID_Pedido) REFERENCES Pedido(ID_Pedido)
);

CREATE TABLE Fornecedor (
    ID_Fornecedor INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100),
    Telefone VARCHAR(15),
    Email VARCHAR(100),
    Endereço TEXT
);

CREATE TABLE Produto (
    ID_Produto INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100),
    Descrição TEXT,
    Preço DECIMAL(10,2),
    ID_Fornecedor INT,
    Quantidade_Estoque INT,
    FOREIGN KEY (ID_Fornecedor) REFERENCES Fornecedor(ID_Fornecedor)
);

CREATE TABLE Item_Pedido (
    ID_Pedido INT,
    ID_Produto INT,
    Quantidade INT,
    Valor_Unitario DECIMAL(10,2),
    PRIMARY KEY (ID_Pedido, ID_Produto),
    FOREIGN KEY (ID_Pedido) REFERENCES Pedido(ID_Pedido),
    FOREIGN KEY (ID_Produto) REFERENCES Produto(ID_Produto)
);

Queries
Recuperações simples com SELECT:

SELECT Nome, Email FROM Cliente WHERE Tipo = 'PF';

Filtro com WHERE:

SELECT Nome, Preço FROM Produto WHERE Quantidade_Estoque < 50;

Atributo derivado:

SELECT ID_Pedido, SUM(Quantidade * Valor_Unitario) AS Total_Valor
FROM Item_Pedido
GROUP BY ID_Pedido;

Ordenação de dados:

SELECT Nome, Valor_Total FROM Pedido 
ORDER BY Valor_Total DESC;

Condições de filtros com HAVING:

SELECT ID_Cliente, COUNT(*) AS Total_Pedidos
FROM Pedido
GROUP BY ID_Cliente
HAVING Total_Pedidos > 5;

Junções entre tabelas:

SELECT C.Nome AS Cliente, P.Nome AS Produto, F.Nome AS Fornecedor
FROM Pedido PD
JOIN Item_Pedido IP ON PD.ID_Pedido = IP.ID_Pedido
JOIN Produto P ON IP.ID_Produto = P.ID_Produto
JOIN Fornecedor F ON P.ID_Fornecedor = F.ID_Fornecedor
JOIN Cliente C ON PD.ID_Cliente = C.ID_Cliente;

