"# Porfolio" 
Arquitectura 
[ CAPA DE DATOS ]
  └── src/data/boardData.json  --->  (Centraliza columnas, tarjetas, tags y modales)
          │
[ CAPA DE COMPONENTES UI (Astro / React + Tailwind CSS) ]
  ├── Header & Filters Bar     --->  (Filtros dinámicos: QA, Functional Analyst, PM, Dev)
  ├── Kanban Board (Layout)    --->  (CSS Grid en Desktop / Tabs-Swiper en Mobile)
  ├── Kanban Column            --->  (To Do, In Progress, Done, Backlog)
  ├── Kanban Card              --->  (Hitos, métricas de impacto, historias de usuario)
  └── Card Modal Detail        --->  (Diálogo accesible para el detalle de cada tarjeta)
          │
[ COMPILACIÓN Y DESPLIEGUE (CI/CD) ]
  └── Build Process            --->  (Generación de HTML/CSS estático puro)
          │
  └── GitHub + Vercel / Netlify --->  (CDN global, carga instantánea y SSL automático)