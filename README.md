# Sistema de Gestión Integral para Iglesias (Multiplataforma)

Sistema full-stack completo y funcional para la administración integral de iglesias y congregaciones religiosas, diseñado con arquitectura modular limpia y adaptabilidad nativa para dispositivos móviles, tabletas y computadoras de escritorio.

---

## 🚀 Arquitectura y Tecnologías

- **Frontend Multiplataforma:** React 18, Vite, Tailwind CSS, Lucide Icons, Chart.js, jsPDF & jsPDF-AutoTable, XLSX SheetJS.
  - Diseño *Mobile-First* con barra inferior táctil (*touch-friendly*) para smartphones y barra lateral para pantallas grandes.
  - Conexión vía proxy inverso local para eliminar problemas de CORS.
- **Backend RESTful:** Node.js, Express, JWT (JSON Web Tokens), Bcryptjs.
- **Base de Datos Relacional:** SQLite nativo integrado (`node:sqlite` con modo WAL e integridad referencial por claves foráneas). Sin dependencias de compiladores externos C++.
- **Esquema Relacional:** 10 tablas estructuradas con borrado lógico (*soft-delete*), cascadas y transacciones atómicas seguras.

---

## 📋 Módulos Funcionales Implementados (100%)

### 1. RF-01: Inicio de sesión y usuarios
- Registro de usuarios con validaciones de formulario.
- Autenticación segura mediante contraseñas encriptadas con `bcrypt` (10 rondas de salt) y tokens JWT con expiración.
- Flujo completo de recuperación y restablecimiento de contraseña mediante código de verificación de 6 dígitos.
- **Gestión estricta de 5 roles de usuario:**
  1. `Administrador`: Acceso y control total de usuarios, configuraciones, finanzas y reportes.
  2. `Pastor`: Supervisión espiritual, eventos, miembros, reportes y módulo financiero completo.
  3. `Secretario`: Gestión de miembros, eventos y asistencia.
  4. `Tesorero`: Control contable de aportaciones, donaciones y reportes financieros.
  5. `Coordinador`: Gestión de ministerios, actividades y asistencia a cultos.
- Middleware de autorización granular en backend (`autorizarRoles`) y guards de navegación en frontend.

### 2. RF-02: Gestión de miembros
- Formulario de alta y edición con validación de campos obligatorios y opcionales (Nombre, Teléfono, Correo, Dirección, Fecha de Ingreso, Fecha de Nacimiento, Notas).
- Directorio de miembros paginado con búsqueda en tiempo real (debounce 300ms).
- Filtros rápidos por estado: Activos, Inactivos o Todos.
- **Desactivación lógica (Soft-Delete):** Permite retirar temporalmente o dar de baja a un miembro sin destruir su historial ni registros pasados de asistencia o aportaciones.

### 3. RF-03: Control de asistencia
- Registro dinámico de asistencia a cultos dominicales, estudios bíblicos, vigilias y reuniones especiales.
- Selector de evento y fecha correspondiente.
- Hoja interactiva tipo lista para alternar entre "Presente" y "Ausente" miembro por miembro con un solo toque.
- Acciones masivas: "Marcar Todos Presentes" y "Marcar Todos Ausentes".
- Guardado transaccional atómico (`BEGIN TRANSACTION / COMMIT`).
- Consulta de historial individual por miembro con cálculo de tasa porcentual de asistencia.

### 4. RF-04: Gestión de eventos
- Creación, modificación y cancelación de eventos con registro de motivo.
- Atributos completos: Título, tipo de evento, fecha, hora, lugar, descripción y asignación de miembro responsable.
- **Calendario interactivo mensual:** Navegación por mes y año, conteo de eventos por día, acceso a creación directa con un clic sobre el día.
- Vista alternativa de listado cronológico de eventos.

### 5. RF-05: Administración de ministerios
- Registro de ministerios o departamentos (Alabanza, Jóvenes, Niños, Damas, Ujieres, etc.).
- Asignación de coordinadores de la congregación a cada ministerio.
- Módulo para vincular y desvincular miembros con definición de rol ministerial específico.
- Registro y consulta de cronogramas y actividades internas por grupo.

### 6. RF-06: Gestión de donaciones y aportaciones
- Registro detallado de aportaciones: Monto, fecha, concepto (Diezmo, Ofrenda, Pro-Templo, Misiones), método de recepción (Efectivo, transferencia, tarjeta, cheque) y notas.
- **Soporte para registros anónimos:** Permite capturar colectas u ofrendas generales sin vincularlas a una persona.
- Filtros por rango de fechas (Fecha Inicial y Fecha Final) y por concepto.
- Cálculo automático de resúmenes mensuales y desglose por conceptos y métodos de pago.
- **Seguridad financiera:** Restricción estricta en frontend y backend: la información contable está reservada únicamente para `Administrador`, `Pastor` y `Tesorero`.

### 7. RF-07: Comunicados y anuncios
- Muro de anuncios institucionales y convocatorias.
- Capacidad para crear, editar, destacar con estrella y eliminar publicaciones.
- Filtro por categorías temáticas (General, Jóvenes, Niños, Damas, Formación, Servicio).
- Registro automático de fecha de publicación y usuario autor.
- Integración destacada en el Panel Principal (Dashboard).

### 8. RF-08: Panel principal (Dashboard)
- Tarjetas métricas con contadores en vivo: Total de miembros activos, miembros inactivos, eventos próximos, asistencias recientes y ministerios activos.
- Resumen financiero de ingresos mensuales en tiempo real (visible solo para roles autorizados).
- Gráficas estadísticas con Chart.js:
  - Gráfica de barras de asistencia en cultos recientes (Presentes vs Ausentes).
  - Gráfica de dona de distribución de ingresos por concepto (Diezmo, Ofrenda, Pro-templo).
- Acciones rápidas (1-click) para pasar lista o programar eventos.

### 9. RF-09: Reportes
- Generación de los 4 reportes obligatorios con vista previa y resumen:
  1. *Reporte General de Miembros* (filtrable por estado y fechas).
  2. *Reporte de Asistencia por Evento* (con tasa de presencia y conteo de ausentes).
  3. *Reporte de Participación por Ministerio* (con coordinadores e integrantes).
  4. *Reporte de Ingresos por Periodo* (restringido a roles financieros).
- **Exportación a PDF:** Formato corporativo limpio con membrete de la iglesia, fecha de emisión y tabla de datos paginada con `jspdf-autotable`.
- **Exportación a Excel (XLSX):** Archivo descargable compatible con Microsoft Excel, Google Sheets y LibreOffice.

### 10. RF-10: Configuración de la iglesia
- Personalización de datos institucionales: Nombre de la congregación, lema, pastores principales, dirección, teléfono, correo y URL de logotipo.
- Gestor dinámico de horarios habituales de cultos (Día, hora y nombre del servicio) con capacidad de agregar y eliminar reuniones.
- Actualización en tiempo real en la cabecera y barra de navegación de la aplicación.

---

## 🔐 Cuentas de Acceso Demostrativas (Semilla Inicial)

La base de datos se inicializa automáticamente al iniciar el servidor con 5 cuentas listas para probar cada rol:

| Rol | Correo Electrónico | Contraseña | Permisos |
| :--- | :--- | :--- | :--- |
| **Administrador** | `admin@iglesia.org` | `Iglesia2026*` | Acceso total al sistema, usuarios y finanzas |
| **Pastor** | `pastor@iglesia.org` | `Iglesia2026*` | Supervisión general, miembros, eventos, finanzas y reportes |
| **Secretario** | `secretario@iglesia.org` | `Iglesia2026*` | Miembros, eventos, asistencia y reportes básicos |
| **Tesorero** | `tesorero@iglesia.org` | `Iglesia2026*` | Finanzas, donaciones, diezmos y reportes contables |
| **Coordinador** | `coordinador@iglesia.org` | `Iglesia2026*` | Ministerios, actividades y asistencia a eventos |

*Nota: La pantalla de inicio de sesión incluye botones de acceso rápido (1-Click) para alternar instantáneamente entre los 5 roles.*

---

## 💻 Instrucciones para Ejecutar el Proyecto

Puedes iniciar el sistema de dos formas muy sencillas:

### Opción 1: Inicio Rápido Unificado (Recomendado)
Abre una terminal en la raíz del proyecto (`c:\Warner\Iglesia`):
```bash
npm run dev
```
Este comando ejecuta en paralelo y en una sola terminal tanto la API Backend (`http://localhost:4000`) como la interfaz Frontend (`http://localhost:5173`).

---

### Opción 2: Servidor de Producción / Autoservido
Si deseas ejecutar la aplicación completa en un único puerto:
```bash
npm start
```
El servidor backend atenderá tanto las peticiones API como la aplicación web en `http://localhost:4000`.

---

### Opción 3: Terminales Independientes
Si prefieres tener terminales separadas para backend y frontend:

**Terminal 1 (Backend en puerto 4000):**
```bash
cd backend
npm run dev
```

**Terminal 2 (Frontend en puerto 5173):**
```bash
cd frontend
npm run dev
```
