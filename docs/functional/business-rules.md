# Especificación de Reglas de Negocio (BRD)
**Proyecto:** Sistema de Gestión de Turnos y Agenda para PyMEs  
**Autor:** Magalí Godoy  
**Rol:** Functional Analyst / Product Manager  

---

## 1. Introducción
El presente documento define las Reglas de Negocio (Business Rules) duras que gobiernan el comportamiento operativo, el control de concurrencia y la lógica del Sistema de Gestión de Turnos. Estas reglas deben ser respetadas obligatoriamente tanto por la capa de Backend (API REST) como por la interfaz de usuario (Frontend).

---

## 2. Reglas de Negocio

### BR-001: Reserva Temporal y Expiración de Confirmación
* **Descripción:**  
  Al enviar el formulario de reserva, el sistema registrará el turno en estado `PENDIENTE` y bloqueará el slot horario seleccionado por un periodo máximo de 15 minutos. Simultáneamente, el sistema enviará un correo electrónico de verificación al usuario con un enlace de confirmación.
* **Flujos de Comportamiento:**
  * **Flujo de Éxito:** Si el usuario confirma la reserva mediante el enlace dentro del plazo de 15 minutos, el sistema cambiará el estado del turno a `CONFIRMADO` y desplegará la confirmación en la interfaz.
  * **Flujo de Expiración:** Si transcurren los 15 minutos sin confirmación por parte del cliente, un proceso automático del sistema cambiará el estado del turno a `EXPIRADO`, liberando el slot horario en la agenda pública.

---

### BR-002: Validación de Disponibilidad Horaria y Control de Concurrencia
* **Descripción:**  
  El sistema únicamente permitirá seleccionar turnos dentro de los rangos de atención configurados previamente por el Administrador, excluyendo de la grilla pública los días no laborables, feriados y recesos intermedios.
* **Flujos de Comportamiento:**
  * **Regla de Concurrencia:** En caso de que dos o más usuarios intenten reservar el mismo slot horario de forma simultánea, el sistema otorgará la reserva temporal (estado `PENDIENTE`) a la primera petición que impacte con éxito en la base de datos (criterio por *timestamp* de transacción).
  * **Manejo de Excepción:** Para los demás usuarios que intenten confirmar el mismo slot, el backend rechazará la solicitud devolviendo un código de error de conflicto (`409 Conflict`). La interfaz de usuario mostrará una alerta modal (*"El horario seleccionado acaba de ser reservado"*), deshabilitará la opción ocupada y solicitará la selección de un nuevo horario.

---

### BR-003: Política de Cancelación Configurable
* **Descripción:**  
  El sistema permitirá a los clientes cancelar sus turnos confirmados únicamente si la solicitud se realiza con una antelación mayor o igual a $X$ horas respecto a la fecha y hora programada del turno (donde $X$ es un parámetro configurable por el Administrador, valor por defecto: 24 horas).
* **Flujos de Comportamiento:**
  * **Flujo de Éxito:** Al cancelar dentro del plazo estipulado, el estado del turno cambiará a `CANCELADO_CLIENTE` y el slot horario quedará disponible inmediatamente en la agenda pública.
  * **Flujo de Excepción:** Si el usuario intenta cancelar fuera del plazo límite, el sistema rechazará la acción a nivel de API, mantendrá el turno en estado `CONFIRMADO` y desplegará un mensaje explicativo: *"No es posible cancelar el turno con menos de $X$ horas de anticipación. Por favor, póngase en contacto directo con el establecimiento."*

---

### BR-004: Protección Anti-Spam y Penalización por Inasistencia (*No-Show*)
* **Descripción:**  
  El sistema implementará mecanismos de control operacional y prevención de fraude para resguardar la agenda de la PyME.
* **Flujos de Comportamiento:**
  * **Control Anti-Spam (Rate Limiting):** El backend restringirá un máximo de 3 solicitudes de reserva pendientes por hora desde una misma dirección IP o cuenta de correo electrónico.
  * **Bloqueo por Inasistencia Configurable:** Si el Staff o Administrador marca un turno con el estado `NO_PRESENTADO`, el sistema incrementará el contador de inasistencias del cliente. Al alcanzar el número máximo de inasistencias permitido ($N$ ausencias en un periodo de $M$ días, parámetros configurables por el Administrador), el estado de la cuenta cambiará a `BLOQUEADO_TEMPORAL`, impidiendo la creación de nuevas reservas por un periodo de $P$ días.

---

### BR-005: Restricción de Reservas Múltiples Activas por Cliente
* **Descripción:**  
  El sistema limitará la cantidad de turnos activos (estados `PENDIENTE` o `CONFIRMADO`) que un mismo cliente puede mantener de forma simultánea en la plataforma para evitar acaparamiento de agenda.
* **Flujos de Comportamiento:**
  * **Límite Parametrizable:** Un cliente solo podrá mantener un máximo de $K$ turnos activos en la agenda (parámetro configurable por el Administrador, valor por defecto: 2 turnos).
  * **Control de Validación:** Al intentar iniciar un nuevo proceso de reserva, el sistema verificará el historial del cliente. Si alcanza el límite $K$, la plataforma impedirá el envío del formulario notificando al usuario: *"Ha alcanzado el límite máximo de reservas activas simultáneas ($K$). Para agendar una nueva cita, debe completar o cancelar alguno de sus turnos vigentes."*