
# Aerolinea Database 

Este repositorio contiene el esquema de base de datos para gestionar información relacionada con aerolíneas, incluyendo detalles de aeropuertos, aviones, tripulación, trayectos y reservas.

## Creación de la Base de Datos

```sql


CREATE DATABASE aerolinea;
USE aerolinea;

CREATE TABLE aerolinea(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    CONSTRAINT PK_AEROLINEA_Id PRIMARY KEY (Id)
);

CREATE TABLE pais(
    Id VARCHAR(5),
    Nombre VARCHAR(10),
    CONSTRAINT PK_PAIS_Id PRIMARY KEY (Id)
);

CREATE TABLE ciudad(
    Id VARCHAR(5),
    Nombre VARCHAR(30),
    Idpais_fk VARCHAR(5),
    CONSTRAINT PK_CIUDAD_Id PRIMARY KEY (Id),
    CONSTRAINT FK_CiudadPais_Id FOREIGN KEY (Idpais_fk) REFERENCES pais (Id)
);

CREATE TABLE aeropuerto(
    Id VARCHAR(5),
    Nombre VARCHAR(30),
    idciudad_fk VARCHAR(5),
    CONSTRAINT PK_AEROPUERTO_Id PRIMARY KEY (Id),
    CONSTRAINT FK_AeropuertoCiudad_Id FOREIGN KEY (idciudad_fk) REFERENCES ciudad (Id)
);

CREATE TABLE estado(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    CONSTRAINT PK_ESTADO_Id PRIMARY KEY (Id)
);

CREATE TABLE modelo(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    CONSTRAINT PK_MODELO_Id PRIMARY KEY (Id)
);

CREATE TABLE fabricante(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(40),
    CONSTRAINT PK_FABRICANTE_Id PRIMARY KEY (Id)
);

CREATE TABLE avion(
    Id int NOT NULL AUTO_INCREMENT,
    Nromatricula VARCHAR(30),
    Capacidad int,
    Fechafabricacion DATE,
    idestado_fk int,
    idfabricante_fk int,
    modelo VARCHAR(30),
    idaerolinea_fk int,
    idmodelo_fk int,
    CONSTRAINT PK_AVION_Id PRIMARY KEY (Id),
    CONSTRAINT FK_AvionEstado_Id FOREIGN KEY (idestado_fk) REFERENCES estado (Id),
    CONSTRAINT FK_AvionAerolinea_Id FOREIGN KEY (idaerolinea_fk) REFERENCES aerolinea (Id),
    CONSTRAINT FK_AvionModelo_Id FOREIGN KEY (idmodelo_fk) REFERENCES modelo (Id),
    CONSTRAINT FK_AvionFabricante_Id FOREIGN KEY (idfabricante_fk) REFERENCES fabricante (Id)
);

CREATE TABLE tipodocumento(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    CONSTRAINT PK_TipoDocumento_Id PRIMARY KEY (Id)
);

CREATE TABLE clientes(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    Edad int,
    Email VARCHAR(50),
    Idtipodoc_fk int,
    CONSTRAINT PK_CLIENTES_Id PRIMARY KEY (Id),
    CONSTRAINT FK_TipDocClientes_Id FOREIGN KEY (Idtipodoc_fk) REFERENCES tipodocumento (Id)
);

CREATE TABLE roltripulacion(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    CONSTRAINT PK_RolTripulacion_Id PRIMARY KEY (Id)
);

CREATE TABLE empleado(
    Id int NOT NULL AUTO_INCREMENT,
    Nombre VARCHAR(30),
    idrol_fk int,
    fechaingreso DATE,
    Idaerolinea_fk int,
    Idaeropuerto_fk VARCHAR(5),
    CONSTRAINT PK_EMPLEADO_Id PRIMARY KEY (Id),
    CONSTRAINT FK_EmpleadoRol_Id FOREIGN KEY (idrol_fk) REFERENCES roltripulacion (Id),
    CONSTRAINT FK_EmpleadoAerolinea_Id FOREIGN KEY (Idaerolinea_fk) REFERENCES aerolinea (Id),
    CONSTRAINT FK_EmpleadoAeropuerto_Id FOREIGN KEY (Idaeropuerto_fk) REFERENCES aeropuerto (Id)
);

CREATE TABLE trayecto(
    Id int NOT NULL AUTO_INCREMENT,
    Fechatrayecto DATE,
    Valor DECIMAL (10,2),
    Ciudadorigen VARCHAR(50),
    Ciudaddestino VARCHAR(50),
    CONSTRAINT PK_TRAYECTO_Id PRIMARY KEY (Id)
);

CREATE TABLE escala(
    Id int NOT NULL AUTO_INCREMENT,
    idtrayecto_fk int,
    idavion_fk int,
    Nrovuelo int,
    Idaeropuerto_fk VARCHAR(5),
    CONSTRAINT PK_ESCALA_Id PRIMARY KEY (Id),
    CONSTRAINT FK_EscalaTrayecto_Id FOREIGN KEY (idtrayecto_fk) REFERENCES trayecto (Id),
    CONSTRAINT FK_EscalaAvion_Id FOREIGN KEY (idavion_fk) REFERENCES avion (Id),
    CONSTRAINT FK_EscalaAeropuerto_Id FOREIGN KEY (Idaeropuerto_fk) REFERENCES aeropuerto (Id)
);

CREATE TABLE gates(
    Id int NOT NULL AUTO_INCREMENT,
    Nropuerta int,
    Idaeropuerto_fk VARCHAR(5),
    CONSTRAINT PK_GATES_Id PRIMARY KEY (Id),
    CONSTRAINT FK_GatesAeropuerto_Id FOREIGN KEY (Idaeropuerto_fk) REFERENCES aeropuerto (Id)
);

CREATE TABLE revision(
    Id int NOT NULL AUTO_INCREMENT,
    Fecha DATE,
    idavion_fk int,
    CONSTRAINT PK_REVISION_Id PRIMARY KEY (Id),
    CONSTRAINT FK_RevisionAvion_Id FOREIGN KEY (idavion_fk) REFERENCES avion (Id)
);

CREATE TABLE detallerevision(
    Id int NOT NULL AUTO_INCREMENT,
    idrevision_fk int,
    Descripcion TEXT,
    Fecharev DATE,
    idempleado_fk int,
    CONSTRAINT PK_DETALLEREVISION_Id PRIMARY KEY (Id),
    CONSTRAINT FK_Detallerevision_Id FOREIGN KEY (idrevision_fk) REFERENCES revision (Id),
    CONSTRAINT FK_DetallerivEmpleado_Id FOREIGN KEY (idempleado_fk) REFERENCES empleado (Id)
);

CREATE TABLE tarifasvuelo(
    Id int NOT NULL AUTO_INCREMENT,
    Descripcion TEXT,
    Valor DOUBLE,
    CONSTRAINT PK_TARIFASVUELO_Id PRIMARY KEY (Id)
);

CREATE TABLE detallereservar(
    Id int NOT NULL AUTO_INCREMENT,
    idtarifa_fk int,
    idcliente_fk int,
    Valortarifa DOUBLE,
    CONSTRAINT PK_DETALLERESERVAR_Id PRIMARY KEY (Id),
    CONSTRAINT FK_DetallereservaTarifa_Id FOREIGN KEY (idtarifa_fk) REFERENCES tarifasvuelo (Id),
    CONSTRAINT FK_DetallereservaCliente_Id FOREIGN KEY (idcliente_fk) REFERENCES clientes (Id)
);

CREATE TABLE reservarviaje(
    Id int NOT NULL AUTO_INCREMENT,
    Fecha DATE,
    idtrayecto_fk int,
    CONSTRAINT PK_RESERVARVIAJE_Id PRIMARY KEY (Id),
    CONSTRAINT FK_ReservarViajeTrayecto_Id FOREIGN KEY (idtrayecto_fk) REFERENCES trayecto (Id)
);

CREATE TABLE revempleado(
    Id int NOT NULL AUTO_INCREMENT,
    idempleado_fk int,
    idrevision_fk int,
    CONSTRAINT PK_REVEMPLEADO_Id PRIMARY KEY (Id),
    CONSTRAINT FK_RevEmpleado_Id FOREIGN KEY (idempleado_fk) REFERENCES empleado (Id),
    CONSTRAINT FK_RevempleadoRevision_Id FOREIGN KEY (idrevision_fk) REFERENCES revision (Id)
);

CREATE TABLE trayectotripulacion(
    Id int NOT NULL AUTO_INCREMENT,
    idempleado_fk int,
    idescala_fk int,
    CONSTRAINT PK_TRAYECTOTRIPULACION_Id PRIMARY KEY (Id),
    CONSTRAINT FK_trayectotripulacionEmpleado_Id FOREIGN KEY (idempleado_fk) REFERENCES empleado (Id),
    CONSTRAINT FK_trayectotripulacionEscala_Id FOREIGN KEY (idescala_fk) REFERENCES escala (Id)
);


