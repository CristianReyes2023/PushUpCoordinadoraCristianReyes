# Cristian Leonardo Reyes Moreno

# README de la Base de Datos

## Resumen

Esta base de datos está diseñada para gestionar diversos aspectos de un sistema de envíos y logística, incorporando autenticación y autorización de usuarios. Incluye entidades como países, estados, ciudades, direcciones, oficinas, posiciones, empleados, clientes, métodos de pago, pagos, detalles de paquetes, estados de pedidos, pedidos, detalles de pedidos, tipos de envío, usuarios, roles y tokens de actualización.

## Tablas

1. **COUNTRY**
   - Almacena información sobre países.

2. **STATE**
   - Representa estados dentro de países.
   - Clave Foránea: `IdCountryFk` referencia `COUNTRY(Id)`.

3. **CITY**
   - Contiene información sobre ciudades.
   - Clave Foránea: `IdStateFk` referencia `STATE(Id)`.

4. **ADDRESS**
   - Gestiona detalles de direcciones, incluyendo direcciones de oficinas y clientes.
   - Claves Foráneas: `IdCityFk` referencia `CITY(Id)`; `Id` referencia `OFFICE(IdAddressFk)` y `CLIENT(IdAddressClienteFk)`.

5. **OFFICE**
   - Almacena detalles sobre ubicaciones de oficinas.

6. **POSITION**
   - Define diferentes posiciones laborales.

7. **EMPLOYEE**
   - Representa empleados y sus detalles.
   - Claves Foráneas: `IdPositionFk` referencia `POSITION(Id)`; `IdOfficeFk` referencia `OFFICE(Id)`.

8. **CLIENT**
   - Contiene información sobre clientes.

9. **PAYMENTMETHOD**
   - Describe diferentes métodos de pago.

10. **PAYMENT**
    - Gestiona transacciones de pago.
    - Clave Foránea: `IdPaymentMethodFk` referencia `PAYMENTMETHOD(Id)`.

11. **PACKAGEDETAIL**
    - Almacena detalles sobre paquetes de envío.

12. **STATEORDER**
    - Describe los estados en los que puede encontrarse un pedido.

13. **ORDER**
    - Representa pedidos de clientes.
    - Claves Foráneas: `IdClienteOrderFk` referencia `CLIENT(Id)`; `IdEmpleadoOrderFk` referencia `EMPLOYEE(Id)`; `IdTypeOfShipmentOrderFk` referencia `PACKAGEDETAIL(Id)`; `IdPaymentMethodOrderFk` referencia `PAYMENT(Id)`.

14. **DETAILORDER**
    - Gestiona detalles de pedidos.
    - Claves Foráneas: `IdStateOrderFk` referencia `STATEORDER(Id)`; `IdOrderFk` referencia `ORDER(Id)`; `IdTypeShipmentFk` referencia `TYPESHIPMENT(Id)`.

15. **TYPESHIPMENT**
    - Describe diferentes tipos de envíos.

16. **User**
    - Representa detalles de autenticación de usuarios.

17. **Rol**
    - Define roles de usuario.

18. **RefreshToken**
    - Gestiona tokens de actualización para la autenticación de usuarios.
    - Clave Foránea: `RefreshTokenId` referencia `User(UserId)`.

19. **UserRol**
    - Establece la relación entre usuarios y roles.
    - Claves Foráneas: `UsuarioId` referencia `User(UserId)`; `RolId` referencia `Rol(RolId)`.

**SQL**

```sql

CREATE TABLE `COUNTRY` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameCountry` VARCHAR(50)
);

CREATE TABLE `STATE` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameState` VARCHAR(50),
  `IdCountryFk` INT(11)
);

CREATE TABLE `CITY` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameCity` VARCHAR(50),
  `IdStateFk` INT(11)
);

CREATE TABLE `ADDRESS` (
  `Id` int PRIMARY KEY,
  `path_type` varchar(50),
  `primary_number` smallint,
  `main_letter` char(2),
  `bis` char(10),
  `secondary_letter` char(2),
  `primary_cardinal` char(10),
  `secondary_number` smallint,
  `secondary_cardinal` char(10),
  `complement` varchar(50),
  `IdCityFk` int(11)
);

CREATE TABLE `OFFICE` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameOffice` VARCHAR(50),
  `IdAddressFk` INT(11)
);

CREATE TABLE `POSITION` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NamePosition` VARCHAR(50)
);

CREATE TABLE `EMPLOYEE` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameEmployee` VARCHAR(50) NOT NULL,
  `LastNameOne` VARCHAR(50) NOT NULL,
  `LastNameSecond` VARCHAR(50) NOT NULL,
  `Email` VARCHAR(50) NOT NULL,
  `IdOfficeFk` INT(11),
  `IdPositionFk` INT(11)
);

CREATE TABLE `CLIENT` (
  `Id` INT PRIMARY KEY NOT NULL,
  `IdNumber` VARCHAR(50),
  `NameCliente` VARCHAR(50),
  `LastNameOne` VARCHAR(50),
  `Email` VARCHAR(100),
  `PhoneNumber` VARCHAR(50),
  `IdAddressClienteFk` INT(11)
);

CREATE TABLE `PAYMENTMETHOD` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameMethod` VARCHAR(50) NOT NULL
);

CREATE TABLE `PAYMENT` (
  `Id` VARCHAR(20) PRIMARY KEY NOT NULL,
  `Total` DOUBLE(15,2),
  `IdPaymentMethodFk` INT(11)
);

CREATE TABLE `PACKAGEDETAIL` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameShipment` VARCHAR(50),
  `Large` VARCHAR(50),
  `Width` VARCHAR(50),
  `Height` VARCHAR(50),
  `Weight` INT(10),
  `Commets` VARCHAR(200)
);

CREATE TABLE `STATEORDER` (
  `Id` INT PRIMARY KEY NOT NULL,
  `StateOrder` VARCHAR(50),
  `LastLocation` VARCHAR(50)
);

CREATE TABLE `ORDER` (
  `Id` INT PRIMARY KEY NOT NULL,
  `OrderDate` DATEONLY,
  `DeadLineDate` DATEONLY,
  `ExpectedDate` DATEONLY,
  `Commets` VARCHAR(250),
  `IdClienteOrderFk` INT(11),
  `IdEmpleadoOrderFk` INT(11),
  `IdTypeOfShipmentOrderFk` INT(11),
  `IdPaymentMethodOrderFk` INT(11)
);

CREATE TABLE `DETAILORDER` (
  `Id` INT(11),
  `IdStateOrderFk` INT(11),
  `IdOrderFk` INT(11),
  `IdTypeShipmentFk` INT(11)
);

CREATE TABLE `TYPESHIPMENT` (
  `Id` INT PRIMARY KEY NOT NULL,
  `NameTypeShipment` VARCHAR(30)
);

CREATE TABLE `User` (
  `UserId` INT PRIMARY KEY,
  `Username` VARCHAR(255) NOT NULL,
  `Email` VARCHAR(255) NOT NULL,
  `Password` VARCHAR(255) NOT NULL
);

CREATE TABLE `Rol` (
  `RolId` INT PRIMARY KEY,
  `Nombre` VARCHAR(255) NOT NULL
);

CREATE TABLE `RefreshToken` (
  `RefreshTokenId` INT PRIMARY KEY,
  `UserId` INT,
  `Token` VARCHAR(255) NOT NULL,
  `Expires` DATETIME NOT NULL,
  `Created` DATETIME NOT NULL,
  `Revoked` DATETIME,
  `IsActive` BOOLEAN
);

CREATE TABLE `UserRol` (
  `UsuarioId` INT,
  `RolId` INT
);

ALTER TABLE `STATE` ADD FOREIGN KEY (`IdCountryFk`) REFERENCES `COUNTRY` (`Id`);

ALTER TABLE `CITY` ADD FOREIGN KEY (`IdStateFk`) REFERENCES `STATE` (`Id`);

ALTER TABLE `ADDRESS` ADD FOREIGN KEY (`IdCityFk`) REFERENCES `CITY` (`Id`);

ALTER TABLE `ADDRESS` ADD FOREIGN KEY (`Id`) REFERENCES `OFFICE` (`IdAddressFk`);

ALTER TABLE `EMPLOYEE` ADD FOREIGN KEY (`IdPositionFk`) REFERENCES `POSITION` (`Id`);

ALTER TABLE `EMPLOYEE` ADD FOREIGN KEY (`IdOfficeFk`) REFERENCES `OFFICE` (`Id`);

ALTER TABLE `ADDRESS` ADD FOREIGN KEY (`Id`) REFERENCES `CLIENT` (`IdAddressClienteFk`);

ALTER TABLE `PAYMENT` ADD FOREIGN KEY (`IdPaymentMethodFk`) REFERENCES `PAYMENTMETHOD` (`Id`);

ALTER TABLE `ORDER` ADD FOREIGN KEY (`IdClienteOrderFk`) REFERENCES `CLIENT` (`Id`);

ALTER TABLE `ORDER` ADD FOREIGN KEY (`IdEmpleadoOrderFk`) REFERENCES `EMPLOYEE` (`Id`);

ALTER TABLE `ORDER` ADD FOREIGN KEY (`IdTypeOfShipmentOrderFk`) REFERENCES `PACKAGEDETAIL` (`Id`);

ALTER TABLE `ORDER` ADD FOREIGN KEY (`IdPaymentMethodOrderFk`) REFERENCES `PAYMENT` (`Id`);

ALTER TABLE `DETAILORDER` ADD FOREIGN KEY (`IdStateOrderFk`) REFERENCES `STATEORDER` (`Id`);

ALTER TABLE `DETAILORDER` ADD FOREIGN KEY (`IdOrderFk`) REFERENCES `ORDER` (`Id`);

ALTER TABLE `DETAILORDER` ADD FOREIGN KEY (`IdTypeShipmentFk`) REFERENCES `TYPESHIPMENT` (`Id`);

ALTER TABLE `RefreshToken` ADD FOREIGN KEY (`RefreshTokenId`) REFERENCES `User` (`UserId`);

ALTER TABLE `UserRol` ADD FOREIGN KEY (`UsuarioId`) REFERENCES `User` (`UserId`);

ALTER TABLE `UserRol` ADD FOREIGN KEY (`RolId`) REFERENCES `Rol` (`RolId`);

```
