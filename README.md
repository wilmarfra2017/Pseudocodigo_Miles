#  Sistema de Gestión de Reservas de Vehículos - Miles Car Rental

#Este documento describe las funciones principales del sistema de gestión de reservas de vehículos.

## Funciones del Sistema

A continuación se presentan las principales funciones del sistema y su lógica de operación en pseudocódigo.

```plaintext
** Función para registrar un nuevo cliente en el sistema **

Funcion RegistrarCliente(nombre, dirección, teléfono, email)
    //Validar datos de entrada
    Si nombre es vacío o dirección es vacía o teléfono es vacío o email es vacío
        Retornar error "Todos los campos son obligatorios"
    Fin Si

    Si no EsEmailValido(email)
        Retornar error "Formato de email inválido"
    Fin Si

    idCliente = GenerarID()
    Cliente = {id: idCliente, nombre: nombre, dirección: dirección, teléfono: teléfono, email: email}
    Clientes[idCliente] = Cliente
    Retornar idCliente
Fin Función


** Función para agregar preferencias a un cliente existente **
Función AgregarPreferencia(idCliente, tipoVehiculoPreferido, accesoriosExtras)
    Si idCliente no está en Clientes
        Retornar error "Cliente no encontrado"
    Fin Si

    // Asumir que tipoVehiculoPreferido y accesoriosExtras son validados por la lógica de la interfaz de usuario

    idPreferencia = GenerarID()
    Preferencia = {id: idPreferencia, cliente: idCliente, tipoVehiculo: tipoVehiculoPreferido, accesorios: accesoriosExtras}
    Preferencias[idPreferencia] = Preferencia
    Retornar idPreferencia
Fin Función

** Función para crear una reserva de vehículo **
Función CrearReserva(idCliente, idVehiculo, fechaInicio, fechaFin)
    Si idCliente no está en Clientes
        Retornar error "Cliente no encontrado"
    Fin Si

    Si idVehiculo no está en Vehiculos o Vehiculos[idVehiculo]['estado'] != "disponible"
        Retornar error "Vehículo no disponible"
    Fin Si

    Si no ValidarFechas(fechaInicio, fechaFin)
        Retornar error "Fechas de reserva inválidas"
    Fin Si

    idReserva = GenerarID()
    Reserva = {id: idReserva, cliente: idCliente, vehiculo: idVehiculo, inicio: fechaInicio, fin: fechaFin, estado: "activa"}
    Reservas[idReserva] = Reserva
    Vehiculos[idVehiculo]['estado'] = "rentado"

    Retornar "Reserva creada exitosamente con ID: " + idReserva
Fin Función

** Función para finalizar una reserva existente **
Función FinalizarReserva(idReserva)
    Si idReserva no está en Reservas
        Retornar error "Reserva no encontrada"
    Fin Si

    reserva = Reservas[idReserva]
    
    Si reserva['estado'] != "activa"
        Retornar error "La reserva ya ha sido finalizada o cancelada"
    Fin Si

    idVehiculo = reserva['vehiculo']
    
    Si idVehiculo no está en Vehiculos
        Retornar error "Vehículo asociado a la reserva no encontrado"
    Fin Si

    Si Vehiculos[idVehiculo]['estado'] != "rentado"
        Retornar error "Inconsistencia de datos: el vehículo no está marcado como rentado"
    Fin Si

    // Proceso de finalización de la reserva
    Reservas[idReserva]['estado'] = "finalizada"
    Vehiculos[idVehiculo]['estado'] = "disponible"

    Retornar "Reserva finalizada exitosamente"
Fin Función

** Función para cancelar una reserva activa **
Función CancelarReserva(idReserva)
    Si idReserva no está en Reservas
        Retornar error "Reserva no encontrada"
    Fin Si

    reserva = Reservas[idReserva]
    
    Si reserva['estado'] != "activa"
        Retornar error "La reserva ya ha sido finalizada o cancelada"
    Fin Si

    // Proceso de cancelación de la reserva
    Reservas[idReserva]['estado'] = "cancelada"
    idVehiculo = reserva['vehiculo']
    Vehiculos[idVehiculo]['estado'] = "disponible"

    Retornar "Reserva cancelada exitosamente"
Fin Función

** Funciones de Utilidad **

Función GenerarID()
    // Generar un GUID, que es un identificador único global
    GUID = GenerarGUID()
    Retornar GUID
Fin Función


Función EsEmailValido(email)
    // Usar una expresión regular simple para validar el formato del email
    ExpresiónRegular = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"
    Si email coincide con ExpresiónRegular
        Retornar verdadero
    Fin Si
    Retornar falso
Fin Función


Función ObtenerFechaActual()
    // Utilizar la funcionalidad del sistema para obtener la fecha y hora actuales
    Retornar FechaYHoraActual del sistema
Fin Función


Función ValidarFechas(fechaInicio, fechaFin)
    Si fechaInicio < ObtenerFechaActual() o fechaInicio >= fechaFin
        Retornar falso
    Fin Si
    Retornar verdadero
Fin Función
