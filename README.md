# 🌈 DiversoLAB – CONECTA  
**Centro de Observación, Evaluación y Coordinación para la Transformación Accesible**  
*(Sistema de monitoreo y visualización accesible desarrollado para DiversoLAB, Medellín – Colombia)*

---

## 🧭 Descripción general

**CONECTA** es el sistema digital de DiversoLAB para el **monitoreo, evaluación y visualización accesible de información social**, con especial foco en la **inclusión de personas con discapacidad intelectual**.

El objetivo es **observar, analizar y coordinar acciones transformadoras accesibles**, integrando herramientas de Google (Drive, Forms, Sheets, Looker)  y una interfaz web simple alojada en **GitHub** desarrollada en **HTML/CSS/JS**.

---

## 🧩 Arquitectura general

👥 Usuarios (participantes, familias, aliados).

📋 Google Forms (captura accesible).

📊 Google Sheets (base de datos en hojas vinculadas).

⚙️ Google Apps Script (limpieza, transformación y automatización).

📁 Google Drive (almacenamiento de formularios y respuestas, respaldo de reportes, CSV y PDFs de salida).

📈 Google Looker Studio (visualización de datos).

🌐 GitHub Pages (interfaz accesible de navegación y difusión).

## 🗂️ Estructura del repositorio

/ (root)

├── index.html # Página de entrada (redirige a formularios.html)

├── formularios.html # Portal de registros, evaluaciones y checklists

├── visualizaciones.html # Portal de dashboards (Looker Studio o Power BI)

├── assets/ # Recursos visuales (logo, íconos)

│ └── logo.gif

├── scripts/ # Scripts Apps Script (.gs)

│ ├── limpieza.gs

│ ├── transformaciones.gs

│ └── indicadores.gs

└── README.md # Este documento

## 📋 Formularios (fuentes de datos)

Cada formulario alimenta una hoja de cálculo con datos crudos (`RAW_`), transformados posteriormente en tablas limpias (`T_`) para visualización.

| Categoría | Formulario | Finalidad | Hoja base |
|------------|-------------|------------|------------|
| **Registros** | Registro de participantes | Captura de datos personales y consentimiento | `RAW_REGISTRO` |
| | Consentimiento de uso de datos (PDF) | Políticas y autorización de tratamiento | — |
| **QoL - INICO FEAPS** | Autoinforme | Autoevaluación de calidad de vida (línea base y post) | `RAW_QOL_AUTO` |
| | Otros | Evaluación por cuidadores o facilitadores | `RAW_QOL_OTROS` |
| **Evaluaciones** | Experiencias | Satisfacción e inclusión en actividades | `RAW_EXP` |
| | Cursos | Evaluación docente y accesibilidad pedagógica | `RAW_CURSOS` |
| | Proveedores | Evaluación de aliados y calidad de servicios | `RAW_PROV` |
| **Checklist** | Accesibilidad | Cumplimiento físico, comunicativo y cognitivo | `RAW_ACC` |
| | Ambiental | Prácticas sostenibles y ambientales | `RAW_AMB` |

> Los datos se transforman y centralizan en una hoja maestra **`BASE_CONECTA`**, desde donde se alimentan las visualizaciones.

---

## ⚙️ Flujo de transformación de datos

**1️⃣ Captura:**  
Cada Google Form escribe automáticamente en su hoja vinculada.

**2️⃣ Limpieza:**  
Scripts de Apps Script eliminan duplicados, normalizan texto y generan claves (`participant_id`, `exp_id`, `fecha_iso`).

**3️⃣ Cálculo:**  
Se generan indicadores por dimensión (QoL, Accesibilidad, Ambiental, Satisfacción).

**4️⃣ Consolidación:**  
Los resultados se unifican en `BASE_CONECTA` con funciones `IMPORTRANGE()` o scripts programados.

**5️⃣ Visualización:**  
Dashboards en Looker Studio o Power BI consultan los datos y los muestran en `visualizaciones.html`.

---

## 📈 Visualizaciones previstas

| Panel | Fuente | Indicadores principales |
|--------|--------|-------------------------|
| **General DiversoLAB** | `BASE_CONECTA` | Participación, accesibilidad, sostenibilidad |
| **QoL FEAPS** | `T_QOL_AUTO`, `T_QOL_OTROS` | Autodeterminación, bienestar, relaciones, comparación pre/post |
| **Experiencias** | `T_EXP` | Inclusión, satisfacción, accesibilidad |
| **Cursos** | `T_CURSOS` | Evaluación docente, claridad, pertinencia |
| **Proveedores** | `T_PROV` | Calidad, cumplimiento, accesibilidad |
| **Accesibilidad** | `T_ACC` | % cumplimiento físico, comunicativo, cognitivo |
| **Ambiental** | `T_AMB` | Prácticas verdes, energía, residuos, movilidad |

---

## 🌐 Interfaz Web

| Archivo | Rol | Contenido principal |
|----------|-----|---------------------|
| **index.html** | Portada / Redirección | Carga inicial y enlace hacia `formularios.html`. |
| **formularios.html** | Módulo operativo | Botonera de acceso a formularios, PDFs y checklists, con visor central `<iframe>`. |
| **visualizaciones.html** | Módulo analítico | Panel de dashboards embebidos desde Looker Studio o Power BI. |

Características de la interfaz:
- Navegación accesible por teclado.  
- Fondo de alto contraste y colores institucionales.  
- Botones de gran tamaño, legibles.  
- `iframe` adaptable (`min-height: 70vh`) para móviles.  
- Botón **“Abrir en pestaña nueva”** para usuarios con lectores de pantalla.

---

## 🔐 Privacidad y protección de datos

- Toda recolección de datos requiere **consentimiento informado** (formulario o PDF).  
- Los dashboards públicos solo muestran **información agregada o anónima**.  
- Los datos personales se almacenan en Google Sheets con acceso restringido.  
- Los participantes pueden solicitar eliminación o modificación de sus registros.

---

## 🚫 Límites técnicos y operativos

| Elemento | Límite recomendado | Síntoma de saturación | Escalamiento sugerido |
|-----------|--------------------|------------------------|------------------------|
| **Forms** | ≤ 2 000 respuestas por formulario | Lentitud al cargar o enviar | Dividir formularios por año o región |
| **Sheets** | ≤ 10 000 filas por hoja | Retardo o errores al abrir | Migrar a BigQuery o Airtable |
| **Apps Script** | 6 min ejecución / 20k ejecuciones diarias | Scripts interrumpidos | Reescribir funciones en Cloud Functions |
| **Looker / Power BI** | ≤ 50k filas / dashboard | Carga > 15s | Conexión directa a BigQuery o Data Warehouse |
| **GitHub Pages** | 1 GB repo / 100 GB tráfico | Página no carga, iframes lentos | Migrar a Netlify o Vercel |

---

## 🚀 Criterios de escalabilidad

**Escalar cuando:**
- Los formularios superen **10 000 respuestas**.  
- Apps Script muestre errores de tiempo o cuota.  
- Los dashboards demoren más de **15 segundos** en cargar.  
- Se requieran múltiples roles o autenticación de usuarios.  
- El tráfico web supere las 100 000 visitas mensuales.  

**Opciones de crecimiento:**
- **Base de datos:** migrar de Sheets → BigQuery o Supabase.  
- **Automatización:** Apps Script → Cloud Functions / ETL Python.  
- **Visualización:** Looker / Power BI → Looker Pro o Metabase.  
- **Hosting:** GitHub Pages → Netlify / Vercel.  
- **Autenticación:** integración con Firebase Auth.  

---

## 🧠 Qué **NO** es CONECTA (y qué sí es)

| No es | Pero sí es |
|--------|-------------|
| Una base de datos relacional | Un repositorio accesible de información social conectada a Sheets |
| Un sitio web dinámico | Una interfaz de visualización segura y gratuita |
| Un BI corporativo | Un tablero de aprendizaje y transparencia social |
| Un sistema cerrado | Un ecosistema modular, interoperable y abierto |
| Un reemplazo de la intervención humana | Un apoyo técnico para mejorar la observación y evaluación social |

---

## 🧩 Recursos y herramientas utilizadas

| Tipo | Herramienta | Uso |
|------|--------------|-----|
| **Recolección** | Google Forms | Encuestas y checklists |
| **Datos** | Google Sheets | Almacenamiento y cálculos |
| **Automatización** | Google Apps Script | Limpieza, validación, agregados |
| **Visualización** | Google Looker Studio / Power BI | Dashboards interactivos |
| **Front-end** | HTML, CSS, JS | Interfaz accesible |
| **Hosting** | GitHub Pages | Publicación gratuita |
| **Diseño accesible** | Figma / Canva | Lineamientos visuales |
| **Gestión ágil** | GitHub Projects | Backlog y sprints |

---

## 🧰 Roles del equipo DiversoLAB

| Rol | Responsabilidad |
|------|----------------|
| **Coordinación general** | Supervisar estructura, indicadores y propósito social. |
| **Gestión de datos** | Diseñar formularios, bases y scripts de transformación. |
| **Desarrollo web** | Mantener la interfaz, estilos y hosting. |
| **Evaluación de impacto** | Analizar dashboards y generar informes ODS. |
| **Accesibilidad** | Validar diseño cognitivo, colores y lenguaje fácil. |

---

## 🗺️ Plan de mantenimiento

- **Revisión mensual:** limpieza de Sheets, verificación de scripts.  
- **Actualización trimestral:** revisión de formularios y dashboards.  
- **Backup semestral:** exportación de datos en CSV y PDF.  
- **Auditoría anual:** revisión de accesibilidad, seguridad y licencias.

---

## 🧮 Indicadores principales

| Dimensión | Ejemplo de indicador |
|------------|----------------------|
| **Accesibilidad física** | % de lugares con rampas o baños accesibles |
| **Accesibilidad cognitiva** | % de materiales en lectura fácil |
| **Participación social** | Promedio de experiencias participativas por persona |
| **Calidad de vida (QoL)** | Promedio total FEAPS (auto/otros) |
| **Satisfacción** | NPS de experiencias y cursos |
| **Sostenibilidad** | Puntaje ambiental medio por actividad |

---

## 🪄 Principios de diseño y accesibilidad

- Lenguaje claro y lectura fácil.  
- Contraste visual alto (#fdda64 / #cad2e9 / #3f2b56).  
- Navegación simple y coherente.  
- Compatibilidad con lector de pantalla.  
- Sin contenido animado distractor.  
- Diseño inclusivo y cognitivo universal.

---

## 🧭 Roadmap (2025)

| Fase | Objetivo | Entregables |
|------|-----------|-------------|
| **Fase 1** | Formularios activos y base integrada | `formularios.html`, `BASE_CONECTA` |
| **Fase 2** | Dashboards básicos | `visualizaciones.html`, 3 reportes Looker |
| **Fase 3** | Automatización total | Apps Script de limpieza y KPIs |
| **Fase 4** | Accesibilidad avanzada | Revisión AA y manual cognitivo |
| **Fase 5** | Escalabilidad | Integración BigQuery o Power BI |

---

## 🧱 Licencia y apertura

Proyecto educativo y social de uso libre, bajo licencia  
**Creative Commons BY-NC-SA 4.0**  
*(Atribución – No Comercial – Compartir Igual)*  
> Puedes usarlo, adaptarlo y mejorarlo, siempre citando a **DiversoLAB** y manteniendo su propósito social.

---

## ❤️ Créditos
