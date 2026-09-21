# 📄 Especificación Funcional del Sistema - Portfolio Interactivo

**Versión:** 1.0.0  
**Autora:** Magalí Godoy  
**Roles:** Functional Analyst | QA Specialist | Project Management  

---

## 1. Descripción General del Producto
El portfolio interactivo simula un entorno de gestión de tareas (estilo Jira Kanban Board), permitiendo a los reclutadores y pares técnicos evaluar las competencias de la candidata a través de un entorno familiar para la industria IT.

---

## 2. Casos de Uso Principales (Use Cases)

### UC-01: Filtrado Dinámico de Tarjetas por Rol
* **Descripción:** Permite al usuario filtrar las tarjetas del tablero según el rol profesional seleccionado (FA, QA, PM, Dev) para enfocar la lectura en una competencia específica.
* **Actor:** Reclutador / Visitante del sitio.
* **Precondiciones:** El tablero debe estar cargado completamente con los datos del archivo `boardData.json`.
* **Flujo Principal:**
  1. El usuario visualiza el tablero Kanban con todas las tarjetas activas.
  2. El usuario hace clic en un filtro de rol específico en la barra de navegación o sidebar.
  3. El sistema evalúa el estado del filtro y oculta dinámicamente las tarjetas que no coinciden con el rol seleccionado.
  4. El tablero se re-renderiza mostrando únicamente los tickets relevantes.
* **Postcondiciones:** El usuario visualiza un subconjunto de tickets filtrados sin necesidad de recargar la página.

### UC-02: Visualización de Modales de Información (Sobre Mí, Core Skills, Educación)
* **Descripción:** Despliegue de ventanas modales interactivas que centralizan la información complementaria de la profesional.
* **Actor:** Visitante del sitio.
* **Flujo Principal:**
  1. El usuario hace clic en una opción del menú lateral (Ej: "Core Skills" o "Sobre Mí").
  2. El sistema intercepta el evento e invoca el componente modal correspondiente.
  3. Se despliega una capa de fondo con efecto *backdrop-blur* y la ventana modal con la información estructurada.
  4. El usuario hace clic en el botón de cierre ("✕") o fuera del contenedor del modal.
  5. El sistema oculta el modal y restaura la vista principal.

---

## 3. Matriz de Trazabilidad y Criterios de Aceptación (BDD)

| ID Ticket | Módulo / Característica | Criterio de Aceptación (Given-When-Then) |
| :--- | :--- | :--- |
| **DEV-401** | UI / Filtros | **Given** que el usuario visita el portfolio, <br>**When** selecciona un rol en los filtros, <br>**Then** el tablero muestra exclusivamente las tarjetas asociadas. |
| **UX-101** | Prototipado | **Given** un flujo crítico de usuario, <br>**When** se diseñan los wireframes en Figma, <br>**Then** se asegura la coherencia con los requerimientos funcionales documentados. |
| **QA-302** | API Testing | **Given** un endpoint de backend, <br>**When** se ejecutan las aserciones en Postman, <br>**Then** el código de estado y el esquema JSON coinciden con el contrato. |

---

### Archivo de Licencia (`LICENSE`)
Creá un archivo llamado exactamente **`LICENSE`** en la raíz de tu proyecto (sin ninguna extensión como `.txt`) y pegá el siguiente texto estándar de la Licencia MIT:

```text
MIT License

Copyright (c) 2026 Magalí Godoy

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.