```sql
CREATE DATABASE aerolinea;
USE aerolinea;

CREATE TABLE pais(
  Id INT(5) AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

CREATE TABLE Ciudad(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  Idpais_fk INT,
  CONSTRAINT FK_PaisCiudad FOREIGN KEY (Idpais_fk) REFERENCES pais(Id)
);

CREATE TABLE Aeropuerto(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  Idciudad_fk INT,
  CONSTRAINT FK_CiudadAeropuerto FOREIGN KEY (Idciudad_fk) REFERENCES Ciudad(Id)
);

CREATE TABLE Gates(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nropuerta INT,
  IdAeropuerto_fk INT,
  CONSTRAINT FK_AeropuertoGates FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(Id)
);

CREATE TABLE Aerolinea(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

CREATE TABLE RolTripulacion(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

CREATE TABLE Empleado (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdRol_fk INT, -- Especifica el tipo de datos
  CONSTRAINT FK_RolEmpleados FOREIGN KEY (IdRol_fk) REFERENCES RolTripulacion(Id),
  IdAero INT,
  CONSTRAINT FK_AeroEmpleados FOREIGN KEY (IdAero) REFERENCES Aerolinea(id),
  IdAeropuerto_fk INT,
  CONSTRAINT FK_AeropuertoEmpleados FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(id)
);

CREATE TABLE Trayecto(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  FechaTrayecto DATE,
  Valor DOUBLE,
  CiudadOrigen VARCHAR(30),
  CiudadDestino VARCHAR(30)
);

CREATE TABLE Fabricante(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(40)
);

CREATE TABLE Modelo(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdFabricante_fk INT,
  CONSTRAINT FK_FabricanteModelo FOREIGN KEY (IdFabricante_fk) REFERENCES Fabricante(Id)
);

CREATE TABLE Estado(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

CREATE TABLE Avion (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nromatricula VARCHAR(30),
  Capacidad INT(3),
  FechaFabricacion DATE,
  IdEstado_fk INT,
  CONSTRAINT FK_EstadoAvion FOREIGN KEY (IdEstado_fk) REFERENCES Estado(Id),
  IdModelo_fk INT, -- Cambiado a INT si Modelo(Id) es INT
  CONSTRAINT FK_ModeloAvion FOREIGN KEY (IdModelo_fk) REFERENCES Modelo(Id),
  IdAero_fk INT,
  CONSTRAINT FK_AeroAvion FOREIGN KEY (IdAero_fk) REFERENCES Aerolinea(Id)
);


CREATE TABLE Revision (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Fecha DATE,
  IdAvion_Fk INT, -- Especifica el tipo de datos
  CONSTRAINT FK_AvionRevision FOREIGN KEY (IdAvion_Fk) REFERENCES Avion(Id)
);

CREATE TABLE Revempleado(
  IdEmpleado_Fk INT,
  CONSTRAINT FK_EmpledoRevempleado FOREIGN KEY (IdEmpleado_Fk) REFERENCES Empleado(Id),
  IdRevision_Fk INT,
  CONSTRAINT FK_RevisionEmpleado FOREIGN KEY (IdRevision_Fk) REFERENCES Revision(Id)
);

CREATE TABLE Reservaviaje(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Fecha DATE,
  IdTrayecto INT,
  CONSTRAINT FK_TrayectoReserva FOREIGN KEY (IdTrayecto) REFERENCES Trayecto(Id)
);

CREATE TABLE TipoDocumento(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);



CREATE TABLE Clientes (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Edad INT,
  IdTipo_fk INT,
  CONSTRAINT FK_TipoClientes FOREIGN KEY (IdTipo_fk) REFERENCES TipoDocumento(Id)
);

CREATE TABLE TarifasVuelo(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Descricion VARCHAR(100),
  Valor INT(200)
);


CREATE TABLE DetalleReserva(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  IdClientes_fk INT,
  CONSTRAINT FK_DEtalleClientes FOREIGN KEY (IdClientes_fk) REFERENCES Clientes(Id),
  IdTarifa_fk INT,
  CONSTRAINT FK_DetalleTarifa FOREIGN KEY (IdTarifa_fk) REFERENCES TarifasVuelo(Id),
  ValorTarifa DOUBLE
);

ALTER TABLE Trayecto
  ADD IdCiudad_fk INT,
  ADD CONSTRAINT FK_TrayectoCiudad FOREIGN KEY (IdCiudad_fk) REFERENCES Ciudad(Id);


ALTER TABLE Reservaviaje
  ADD IdTrayecto_fk INT,
  ADD CONSTRAINT FK_TrayectoReservas FOREIGN KEY (IdTrayecto_fk) REFERENCES Trayecto(Id);



ALTER TABLE DetalleReserva
  ADD IdReserva_fk INT,
  ADD CONSTRAINT FK_Reservas FOREIGN KEY (IdReserva_fk) REFERENCES  Reservaviaje(Id);


CREATE TABLE Escala(
  Id INT AUTO_INCREMENT PRIMARY KEY,
  IdTrayecto_fk INT,
  CONSTRAINT FK_Trayecto FOREIGN KEY (IdTrayecto_fk) REFERENCES Trayecto(Id),
  IdAvion_fk INT,
  CONSTRAINT FK_EscalaAvion FOREIGN KEY (IdAvion_fk) REFERENCES Avion(Id),
  IdGates_fk INT,
  CONSTRAINT FK_GatesEscala FOREIGN KEY (IdGates_fk) REFERENCES Gates(Id),
  IdAeropuerto_fk INT,
  CONSTRAINT FK_AeropuertoEscala FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(Id),
  Nrovuelo VARCHAR(20)
);

CREATE TABLE TrayectoTripulacion(
  IdEmpleado_fk INT,
  CONSTRAINT FK_EmpleadosTratecto FOREIGN KEY (IdEmpleado_fk) REFERENCES Empleado(Id),
  IdEscala_fk INT,
  CONSTRAINT FK_EscalaTratecto FOREIGN KEY (IdEscala_fk) REFERENCES Escala(Id)
);

CREATE TABLE DetalleRevision (
  IdRevision_fk INT,
  CONSTRAINT FK_RevisionDetalle FOREIGN KEY (IdRevision_fk) REFERENCES Revision(Id),
  Descripcion VARCHAR(50),
  Fecharev DATE,
  IdEmpleado_fk INT,
  CONSTRAINT FK_RevisionEmpleaddo FOREIGN KEY (IdEmpleado_fk) REFERENCES Empleado(Id),
  Id INT AUTO_INCREMENT PRIMARY KEY
);
