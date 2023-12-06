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



