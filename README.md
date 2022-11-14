class Cliente: id = 0

def asignarId(self):
Cliente.id = Cliente.id + 1 self.id = Cliente.id

def init (self, nombre): self.asignarId() self.nombre = nombre


class Evento: id = 0

def asignarId(self): Evento.id = Evento.id + 1 self.id = Evento.id

def init (self, nombre, cliente) : self.nombre = nombre self.cliente = cliente self.asignarId()

class Sala:
id = 0

def asignarId(self): Sala.id = Sala.id + 1 self.id = Sala.id

def init (self, nombre, cupo): self.nombre = nombre self.cupo = cupo self.asignarId()

class Turno:

def init (self, nombre): self.nombre = nombre self.evento = Evento("NA",-1) self.sala = Sala("",0) self.reservado = False
 
def agregarEvento(self, sala, evento): self.reservado = True
self.evento = evento self.sala = sala


class Slot:

def inicializaTurnos(self): self.turnos.append(Turno("matutino")) self.turnos.append(Turno("vespertino")) self.turnos.append(Turno("nocturno"))

def init (self, fecha): self.turnos = [] self.inicializaTurnos() self.fecha=fecha

def agregarEvento(self, turno, sala, evento): for x in self.turnos:
if x.nombre == turno:
if x.reservado == False: x.evento = evento x.reservado = True x.sala = sala return True
else:
print("=================================================
=============================================")
print("La Sala ",x.sala.nombre," no esta disponible en la fecha ",self.fecha," en el turno ",turno )
return False

def editarEvento(self, idEvento, nombreNuevo): for x in self.turnos:
if x.evento.id == idEvento: x.evento.nombre = nombreNuevo

def busqueda(p_lista, p_id): result = None
for c in p_lista: if(c.id == p_id):
result = c break
return result
 

def busqueda_fecha(p_lista, p_fecha): result = -1
for index, item in enumerate(p_lista): if item.fecha == p_fecha:
result = index break
else:
result = -1 return result

class Reservacion: id = 0

def asignarId(self):
Reservacion.id = Reservacion.id + 1 self.id = Reservacion.id

def init (self, nombre): self.asignarId() self.nombre = nombre
import P as p import datetime import pandas as pd import numpy as np
from tabulate import tabulate

calendario = [] clientes = [] salas = [] cupo = []
clientes.clear() salas.clear() calendario.clear()

def mostrar_menu(nombre, opciones): # incorporamos el parámetro para mostrar el nombre del menú
print('\033[1m',f'{nombre.center(50,"=",)}\n') for clave in sorted(opciones):
print(f' {clave}) {opciones[clave][0]}')

def leer_opcion(opciones):
while (a := input('Opción a elegir:')) not in opciones: print('Opción incorrecta, vuelva a intentarlo.')
return a
 


def ejecutar_opcion(opcion, opciones): opciones[opcion][1]()

def generar_menu(nombre, opciones, opcion_salida): # incorporamos el parámetro para mostrar el nombre del menú
opcion = None
while opcion != opcion_salida: mostrar_menu(nombre, opciones) opcion = leer_opcion(opciones) ejecutar_opcion(opcion, opciones) print()


def menu_principal(): opciones = {
'1': ('REGISTRAR RESERVACION DE SALA', submenu_reservar), '2': ('REPORTES', submenu_reportes),
'3': ('REGISTRAR A UN NUEVO CLIENTE', submenu_cliente), '4': ('REGISTRAR UNA SALA', submenu_sala),
'5': ('SALIR\n', salir)
}

generar_menu('Menú principal', opciones, '5') #indicamos el nombre del
menú

# A partir de aquí creamos las funciones que ejecutan las acciones de los menús

def submenu_cliente():
print("	") print("Has elegido la opcion de Registro del cliente") print("	") opciones = {
'a': ('Inserta el Nombre del cliente', nombre_cliente), 'b': ('Volver al menú principal\n', salir1)
}

generar_menu('Registro del cliente', opciones, 'b') # indicamos el nombre del submenú


def submenu_sala():
print("	") print("Has elegido la opcion de Registro de Sala")
 
print("	") opciones = {
'a': ('Capturar el nombre y cupo de una sala', captura_sala), 'b': ('Volver al menú principal\n', salir1)
}

generar_menu('Registro de sala', opciones, 'b') # indicamos el nombre del submenú


def submenu_reportes():
print("	") print("Has elegido la opcion de Reportes")
print("	") opciones = {
'a': ('Reporte en pantalla de reservaciones para una fecha', reporte_fechas),
'b': ('Exportar reporte tabular en ecxel', reporte_excel), 'c': ('Volver al menú principal\n', salir1)
}

generar_menu('Reportes', opciones, 'c') # indicamos el nombre del submenú

def submenu_reservar():
print("	") print("Has elegido la opcion de Reservar una sala") print("	") opciones = {
'a': ('REGISTAR NUEVA RESERVACION', registar_reservacion), 'b': ('MODIFICAR DESCRIPCION DE UNA RESERVACION',
submenu_editar_nom),
'c': ('CONSULTAR DISPONIBILIDAD DE SALAS PARA UNA FECHA',
submenu_salas_disponibles),
'd': ('Volver al menú principal\n', salir1)
}

generar_menu('Reservar una Sala', opciones, 'd') # indicamos el nombre del submenú

def captura_sala():
print("	") print('Has elegido la opción capturar sala')
print("	") s=input("Ingresa el nombre de una sala: ")
 
cu =input("Captura el cupo de la sala: ") s1=p.Sala(s,cu)
salas.append(s1)
print('\n'"ID Sala: ",s1.id,'\n'"Sala Registrada: ",s1.nombre,'\n'"Cupo de la sala: ",s1.cupo,"personas")

def nombre_cliente():
print("	") print('Has elegido la opción nombre del cliente') print("	") c = input("Ingresa el nombre del cliente: ") c1=p.Cliente(c)
clientes.append(c1)
print('\n'"Clave de cliente: ",c1.id,'\n'"Nombre del cliente: ",c1.nombre)


def lista_salas():
print("	") print("Has elegido la opcion de listado de salas") print("	") t = len(salas)
if t > 0:
for s1 in salas:
print('\t'"ID Sala: ",s1.id,'\t'"Sala Registrada: ",s1.nombre,'\t'"Cupo de la sala: ",s1.cupo,"Personas")
else:
print("	") print("No hay salas disponibles")
print("	")

def list_salas(): t = len(salas) if t > 0:
for s1 in salas:
print('\t'"ID Sala: ",s1.id,'\t'"Sala Registrada: ",s1.nombre,'\t'"Cupo de la sala: ",s1.cupo,"Personas")
else:
print("	") print("No hay salas disponibles")
print("	")

def lista_clientes():
print("	") print("Has elegido la opcion de listado de clientes")
 
print("	") t = len(clientes)
if t > 0:
for c1 in clientes:
print('\t'"ID Cliente: ",c1.id,'\t'"Nombre del Clinte:
",c1.nombre) else:
print("	") print("No hay clientes disponibles")
print("	")

def list_clientes(): t = len(clientes) if t > 0:
for c1 in clientes:
print('\t'"ID Cliente: ",c1.id,'\t'"Nombre del Clinte:
",c1.nombre) else:
print("	") print("No hay clientes disponibles")
print("	")

def registar_reservacion():
print(" 	
 	")
print("Has elegido la opcion de Reservacion de una Sala")
print(" 	
 	\n")
cadena = "Listado de Salas".capitalize() print(cadena.center(90, "="),"\n") list_salas()
print("\n")
cadena = "Listado de Clientes".capitalize() print(cadena.center(90, "="),"\n") list_clientes()
print(" 	
 		\n") while True:
fs = input("En que fecha deseas reservar (DD-MM-YYYY): ") hoy = datetime.datetime.today()
fecha = datetime.datetime.strptime(fs,'%d-%m-%Y') dif = fecha-hoy
dias = (dif.total_seconds()/60/60/24) if dias >=2:
s = input("Ingresa el ID de la Sala: ")
 
sala = p.busqueda(salas,int(s)) if sala is not None:
print("Seleccionaste la sala de ",sala.nombre) print("	\n") sel = input("Selecciona un cliente: ")
cliente = p.busqueda(clientes,int(sel)) if cliente is not None:
print("Seleccionaste el cliente ",cliente.nombre) print("	\n") e = input("Ingresa el nombre del Evento: ")
print("	\n") indice = p.busqueda_fecha(calendario,fs)
if indice == -1: slot = p.Slot(fs)
else:
slot = calendario[indice]
trn = input("En que turno desea reservar (matutino/vespertino/nocturno) : ")
reserva = slot.agregarEvento(trn,sala,p.Evento(e,cliente))
if reserva == True and indice != -1: calendario[indice] = slot print("\nSe realizo la reserva")
elif reserva == True and indice == -1:
print(" 	
 	")
print("******************Se realizo la
reserva****************")
print(" 	
 	")
calendario.append(slot)

preguntar = input ("\nSeleccione [S o s] para realizar otra reservacion, [N o n] para regresar a menu de reservaciones: ")
if preguntar == "S" or preguntar == "s": continue
elif preguntar == "N" or preguntar == "n": break


else:
print(" 	
 	")
print("******************No se realizo la
reserva******************")
 
print(" 	
 	\n")
else:

 	  print("
")
print("******************No existe el Cliente******************") 	
 	  print("
")
else:	 	
 	  print("
")
print("******************No existe la Sala******************")	
 	  print("
")
else:	 	
 	print("	
")
print("********Debe Reservar al menos con 2 dias de anticipacion********")	
 	print("	
")

def submenu_salas_disponibles(): while True:
for sl in calendario: turnos = sl.turnos for t in turnos:
sl = (input("\nIngresa una fecha para Buscar una sala
disponible : "))
print("\n*******************REPORTE DE SALAS DISPONIBLES
PARA LA FECHA",sl,"******************")
d = [ [t.sala.id,t.sala.nombre,t.nombre] ] print(tabulate(d, tablefmt='fancy_grid', stralign='center',
floatfmt='.0f', headers=["ID Sala","Sala", "Turno"]))
print("\n***************************FIN DEL REPORTE***********************")
preguntar = input ("\nSeleccione [S o s] para realizar otra busqueda, [N o n] para concluir: ")
if preguntar == "S" or preguntar == "s": continue
elif preguntar == "N" or preguntar == "n": return
 
if sl not in calendario:
print("	") print("La sala ",t.sala.nombre," no se encuentra disponible
para la fecha ",sl)
print("	") else:
continue

def reporte_fechas(): while True:
for sl in calendario: turnos = sl.turnos for t in turnos:
sl = (input("\nIngresa una fecha para Buscar una reservacion de sala disponible : "))
print("\n*******************REPORTE DE RESERVACIONES EL DIA",sl,"******************")
d = [ [t.sala.id,t.evento.cliente.nombre,t.evento.nombre,t.nombre] ]
print(tabulate(d, tablefmt='fancy_grid', stralign='center', floatfmt='.0f', headers=["ID Sala", "Cliente", "Evento", "Turno"]))
print("\n***************************FIN DEL REPORTE***********************")
preguntar = input ("\nSeleccione [S o s] para realizar otra busqueda, [N o n] para concluir: ")
if preguntar == "S" or preguntar == "s": continue
elif preguntar == "N" or preguntar == "n": return

if sl not in calendario:
print("	") print("La ",sl," no existe para una reservacion") print("	")
else:
continue

def reporte_excel():

preguntar = input ("\nSeleccione [S o s] para realizar la exportacion a excel, [N o n] para concluir: ")
if preguntar == "S" or preguntar == "s": #Exportaccion para un excel
libro = openpyxl.Workbook() #libro.iso_dates = True #Acepte fechas
 
 
 
