# Historias de Usuario (User Stories - MVP)
**Proyecto:** Sistema de Gestión de Turnos y Agenda para PyMEs  
**Autor:** Magalí Godoy  
**Rol:** Functional Analyst / Product Manager  

---

## US-001: Reserva de Cita por parte del Cliente

**Frase de Rol:**
> **Como** cliente registrado  
> **Quiero** seleccionar una fecha y horario disponible en la plataforma  
> **Para** agendar una cita de forma autogestionada sin necesidad de llamar por teléfono.

### Criterios de Aceptación (Escenarios BDD)

* **Escenario 1: Solicitud de reserva exitosa (Pendiente)**
  * **Dado que** el cliente ha iniciado sesión y se encuentra en la pantalla de selección de turnos,
  * **Cuando** selecciona una fecha y horario disponible y presiona el botón "Reservar",
  * **Entonces** el sistema registra el turno en estado `PENDIENTE`, bloquea el slot por 15 minutos (según `BR-001`) y envía un correo electrónico de verificación con los datos del turno (fecha, hora, lugar) y el botón de confirmación.

* **Escenario 2: Confirmación del turno vía Email**
  * **Dado que** el cliente recibió el correo electrónico de verificación dentro del plazo de 15 minutos,
  * **Cuando** hace clic en el botón de confirmación,
  * **Entonces** el sistema actualiza el estado del turno a `CONFIRMADO`, muestra la pantalla de éxito y envía un segundo correo de confirmación definitiva que incluye el enlace directo para cancelar la cita (según `BR-003`).

---

## US-002: Cancelación de Cita por parte del Cliente

**Frase de Rol:**
> **Como** cliente registrado  
> **Quiero** cancelar un turno previamente confirmado desde la aplicación o desde el correo de confirmación  
> **Para** liberar mi horario en la agenda cuando no pueda asistir y evitar penalizaciones por inasistencia.

### Criterios de Aceptación (Escenarios BDD)

* **Escenario 1: Cancelación exitosa desde el Panel de la App (A tiempo)**
  * **Dado que** el cliente ha iniciado sesión y navega a la pestaña "Próximos Turnos",
  * **Y** falta un tiempo mayor o igual al parámetro $X$ de cancelación (según `BR-003`),
  * **Cuando** hace clic en el botón "Cancelar" de un turno en estado `CONFIRMADO`,
  * **Entonces** el sistema actualiza el estado del turno a `CANCELADO_CLIENTE`, libera el slot en la agenda pública y muestra un mensaje de confirmación en pantalla.

* **Escenario 2: Cancelación exitosa vía enlace directo en Email (A tiempo)**
  * **Dado que** el cliente abre el correo electrónico de confirmación recibido previamente,
  * **Y** falta un tiempo mayor o igual al parámetro $X$ de cancelación,
  * **Cuando** hace clic en el botón "Cancelar Turno" del correo,
  * **Entonces** la aplicación procesa la solicitud, redirige al usuario a una pantalla de "Cancelación Exitosa" y actualiza el estado del turno a `CANCELADO_CLIENTE`.

* **Escenario 3: Intento de cancelación fuera de término (Excepción)**
  * **Dado que** el cliente intenta cancelar su turno (ya sea por la app o por email),
  * **Pero** falta un tiempo menor al límite configurado $X$ (ej. menos de 24 horas),
  * **Cuando** solicita la cancelación,
  * **Entonces** el sistema rechaza la acción, mantiene el turno en estado `CONFIRMADO` y muestra el mensaje: *"No es posible cancelar el turno con menos de $X$ horas de anticipación. Por favor, póngase en contacto directo con el establecimiento."*

---

## US-003: Configuración de Parámetros Operativos por el Administrador

**Frase de Rol:**
> **Como** administrador del establecimiento  
> **Quiero** parametrizar las reglas de cancelación y los límites de reservas activas  
> **Para** adaptar las políticas del sistema al flujo de trabajo real de mi PyME.

### Criterios de Aceptación (Escenarios BDD)

* **Escenario 1: Modificación de límites de cancelación y reservas**
  * **Dado que** el administrador se encuentra en la sección de "Configuración del Sistema",
  * **Cuando** modifica las horas mínimas de cancelación ($X$) o el límite máximo de reservas simultáneas por cliente ($K$) y guarda los cambios,
  * **Entonces** el sistema persiste los nuevos parámetros en la base de datos y los aplica de forma inmediata a las futuras validaciones de los clientes.

* **Escenario 2: Validación de datos de configuración ingresados**
  * **Dado que** el administrador intenta modificar la configuración,
  * **Cuando** ingresa valores negativos o no válidos (ej. horas de cancelación en $0$ o texto),
  * **Entonces** el sistema bloquea el guardado, resalta los campos con error y solicita ingresar valores numéricos válidos.