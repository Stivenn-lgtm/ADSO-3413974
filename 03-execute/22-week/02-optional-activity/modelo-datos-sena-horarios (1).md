# Modelo de Datos — Sistema de Gestión de Horarios SENA
### Construido por ingeniería inversa a partir del mockup, los diagramas BPMN y el documento de arquitectura `models.md`

---

## 1. Fuentes analizadas y método de trabajo

Para construir este modelo se revisaron tres tipos de evidencia:

1. **El mockup del sistema** (`compressed.zip`, 10 archivos HTML exportados desde un PDF de diseño). De ese paquete solo dos pantallas quedaron con su contenido completo y legible: el **dashboard "Inicio" del Coordinador Académico** y la pantalla **"Disponibilidad"** (consulta de ambientes e instructores). El resto de páginas del PDF quedaron corruptas en la exportación, pero dentro de ese mismo archivo sobrevivió un **mapa de navegación (sitemap)** con las 53 pantallas del sistema, cada una con su módulo técnico y el rol de usuario al que pertenece. Ese sitemap fue la guía principal para saber qué entidades debían existir aunque su pantalla puntual no se pudiera ver.
2. **Los diagramas BPMN ya elaborados** (`Diagramas.zip`): Coordinador Académico, Instructor, Aprendiz, Director de Centro e Inicio de sesión/Recuperación de contraseña. Estos diagramas muestran el orden real de las acciones y las decisiones del sistema, lo cual ayuda a decidir qué atributos de estado y qué validaciones debe soportar cada tabla.
3. **`models.md`**: un documento de arquitectura que ya existía para este mismo proyecto, organizado por microservicio ("bounded context"). Sus nombres de módulo (`iam-service`, `scheduling-service`, `actors-service`, etc.) coinciden exactamente con los módulos que aparecen etiquetados en el sitemap del mockup, así que se usó como fuente secundaria para completar atributos que ninguna pantalla mostró directamente (tipos de dato exactos, longitudes, columnas de auditoría).

**Regla seguida:** cuando un dato viene literalmente de una pantalla o de un diagrama BPMN, se redactó sin marca especial. Cuando una tabla, atributo o relación no aparece en ninguna pantalla ni en ningún diagrama y se agregó solo porque el sistema no funcionaría sin ella, se marca explícitamente como:

> 🔧 **Supuesto de diseño** (basado en ingeniería inversa) — explica por qué es necesario.

No se copió ninguna tabla, atributo, relación ni redacción de otros documentos de referencia del curso; el análisis parte del mockup, los diagramas BPMN y models.md.

---

## 2. Procesos identificados que sustentan el modelo

Antes de definir tablas conviene recordar, en una frase, qué hace cada actor — porque cada tabla del modelo existe para soportar una de estas acciones:

| Actor | Qué hace (según el mockup y los BPMN ya diagramados) |
|---|---|
| **Coordinador Académico** | Consulta disponibilidad de ambientes e instructores, crea horarios en borrador, agrega sesiones de clase, valida y resuelve conflictos, y publica el horario. |
| **Instructor** | Consulta su horario semanal, marca cada sesión como ejecutada o no ejecutada, registra seguimiento de ficha, y registra excepciones de disponibilidad (incapacidad, permiso, etc.). |
| **Aprendiz** | Consulta su horario semanal y sus notificaciones. |
| **Director de Centro** | Consulta indicadores (KPI), administra usuarios y sus roles, revoca sesiones activas, y edita parámetros/datos de referencia del centro. |
| **Cualquier usuario** | Inicia sesión, recupera su contraseña, y puede recibir un error 403 si su rol no tiene permiso sobre una sección. |

Estos cinco flujos ya están representados en los diagramas BPMN entregados aparte; el modelo de datos que sigue está diseñado para que cada paso de esos diagramas tenga dónde guardarse.

---

## 3. Organización del modelo

El sitemap del mockup etiqueta cada una de sus 53 pantallas con un módulo técnico. Esos mismos módulos se usan aquí como los nueve dominios del modelo, en el orden en que uno depende del otro:

```
1. IAM                 → usuarios, roles, sesiones
2. REFERENCE-DATA      → centro, sedes, catálogos, parámetros
3. ACADEMIC            → programas, competencias, fichas
4. ENVIRONMENT         → ambientes, mantenimiento
5. ACTORS              → instructores, aprendices, excepciones de disponibilidad
6. SCHEDULING          → horarios, sesiones de clase, conflictos
7. MONITORING          → seguimiento de ficha, KPI, alertas
8. DOCUMENT            → documentos y plantillas
9. AUDIT               → registro de auditoría
```

---

## 4. Dominio IAM — Usuarios, roles y sesiones

### 4.1 Tabla `usuario`

Se observa en la esquina superior derecha de toda pantalla (avatar "MG", nombre "María García", rol "Coordinador Académico") y es indispensable para el error 403, que niega el acceso según el rol del usuario que inició sesión.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_usuario | INT / UUID | PK | Identificador único del usuario |
| correo | VARCHAR(255) | — | Correo institucional, único; visto como identificador de inicio de sesión en el flujo de login |
| clave_hash | VARCHAR(255) | — | Contraseña almacenada de forma cifrada (nunca en texto plano) |
| nombres | VARCHAR(100) | — | Nombres del usuario |
| apellidos | VARCHAR(100) | — | Apellidos del usuario |
| id_rol | INT | FK → `rol` | Rol activo con el que el usuario navega el sistema |
| activo | BOOLEAN | — | Permite deshabilitar un usuario sin borrar su historial |
| fecha_creacion | DATETIME | — | Auditoría de creación |
| ultimo_acceso | DATETIME | — | 🔧 Supuesto de diseño: necesario para que el Director de Centro pueda auditar accesos, coherente con la pantalla "Detalle de usuario" del sitemap |

### 4.2 Tabla `rol`

El diagrama BPMN del Director de Centro muestra explícitamente la acción "Llenar formulario Asignar Rol", lo que confirma que los roles son un catálogo administrable, no un valor fijo en código.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_rol | INT | PK | Identificador del rol |
| nombre_rol | VARCHAR(50) | — | Ej.: Coordinador Académico, Instructor, Aprendiz, Director de Centro, Soporte (los cinco confirmados en el sitemap por sus secciones "01" a "06") |
| descripcion | VARCHAR(255) | — | Explicación del alcance del rol |

### 4.3 Tabla `sesion_activa`

El diagrama BPMN "Director de Centro" incluye la acción "Revocar Sesión Activa" desde el detalle de un usuario ("Perfil, Roles, Sesiones"), lo que exige una tabla propia — distinta de `sesion_clase`, para no confundir ambos significados de la palabra "sesión" dentro del sistema.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_sesion | INT | PK | Identificador de la sesión de acceso |
| id_usuario | INT | FK → `usuario` | Usuario dueño de la sesión |
| dispositivo | VARCHAR(150) | — | 🔧 Supuesto de diseño: dato típico mostrado en pantallas "Sesiones" de perfil, necesario para que el director identifique cuál sesión revocar |
| direccion_ip | VARCHAR(45) | — | 🔧 Supuesto de diseño: mismo motivo anterior |
| fecha_inicio | DATETIME | — | Momento en que se creó la sesión |
| fecha_expiracion | DATETIME | — | Vigencia del token de sesión |
| revocada | BOOLEAN | — | Se marca en `true` cuando el Director ejecuta "Revocar Sesión Activa" |

### 4.4 Tabla `intento_acceso`

El flujo BPMN "Inicio de Sesión / Recuperación" contempla explícitamente los caminos "¿Es válido?" y "¿Permiso?" con salida a "Error 403", lo que implica que el sistema registra tanto los intentos fallidos como el motivo del rechazo.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_intento | INT | PK | Identificador del intento |
| correo_ingresado | VARCHAR(255) | — | Correo usado en el intento (puede no existir) |
| resultado | VARCHAR(30) | — | `EXITOSO`, `CLAVE_INVALIDA`, `USUARIO_NO_EXISTE`, `SIN_PERMISO` — este último corresponde literalmente a la pantalla "403" |
| fecha_hora | DATETIME | — | Momento del intento |

---

## 5. Dominio REFERENCE-DATA — Centro y catálogos

### 5.1 Tabla `centro_formacion`

Visto en el encabezado lateral de toda pantalla ("Centro de Formación SENA") y como filtro implícito de todos los datos que consulta un Coordinador (solo ve fichas y ambientes de su propio centro).

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_centro | INT | PK | Identificador del centro |
| codigo_centro | VARCHAR(10) | — | Código institucional del centro |
| nombre | VARCHAR(200) | — | Nombre del centro de formación |
| direccion | VARCHAR(255) | — | Dirección física |
| activo | BOOLEAN | — | Habilita/deshabilita el centro |

### 5.2 Tabla `parametro_sistema`

El diagrama BPMN del Director de Centro incluye explícitamente "Editar Parámetro (ej. MAX_HOURS)" con validación "¿Código ya existe?", lo que confirma un catálogo de parámetros configurables tipo clave-valor.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_parametro | INT | PK | Identificador del parámetro |
| codigo | VARCHAR(50) | — | Único; ej. `MAX_HOURS` visto literalmente en el diagrama BPMN |
| valor | VARCHAR(100) | — | Valor almacenado como texto |
| descripcion | VARCHAR(255) | — | Explicación de para qué sirve el parámetro |

### 5.3 Tabla `catalogo` y `catalogo_detalle`

🔧 **Supuesto de diseño:** el sitemap enumera una sección de "Parametrización" con pantallas de "Catálogos de monitoreo (KPI/alertas)" y "CRUD catálogo / valor / parámetro" para el rol de soporte, pero ninguna pantalla capturada muestra su contenido expandido. Se incluye la estructura mínima que cualquier catálogo administrable necesita, siguiendo el patrón agrupador + detalle que es estándar para este tipo de funcionalidad.

| Tabla | Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|---|
| `catalogo` | id_catalogo | INT | PK | Agrupador, ej. "Tipos de ambiente" |
| `catalogo` | nombre | VARCHAR(100) | — | Nombre del catálogo |
| `catalogo_detalle` | id_detalle | INT | PK | Valor individual del catálogo |
| `catalogo_detalle` | id_catalogo | INT | FK → `catalogo` | Catálogo al que pertenece |
| `catalogo_detalle` | etiqueta | VARCHAR(150) | — | Ej. "Laboratorio", "Aula", "Taller" — vistos literalmente en la pantalla "Disponibilidad" |
| `catalogo_detalle` | orden | INT | — | Orden de presentación en listas desplegables |

---

## 6. Dominio ACADEMIC — Programas, competencias y fichas

### 6.1 Tabla `programa_formacion`

Se ve el código y nombre de programa en el dashboard "Inicio" ("ADSO — Trimestre III").

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_programa | INT | PK | Identificador del programa |
| codigo_programa | VARCHAR(20) | — | Ej.: código visible tipo "ADSO" |
| nombre | VARCHAR(200) | — | Nombre completo del programa |
| nivel_formacion | VARCHAR(30) | — | 🔧 Supuesto de diseño: Técnico/Tecnólogo — no visible en las pantallas recuperadas, pero necesario para clasificar el programa; models.md confirma que este dato existe en el sistema real |

### 6.2 Tabla `competencia`

El sitemap incluye "Currículo académico" dentro de la sección de Parametrización con módulo `academic`, y las sesiones de clase del dashboard referencian una competencia por ficha.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_competencia | INT | PK | Identificador de la competencia |
| id_programa | INT | FK → `programa_formacion` | Programa al que pertenece |
| nombre | VARCHAR(300) | — | Nombre de la competencia a formar |

### 6.3 Tabla `ficha`

Entidad central del sistema — confirmada exhaustivamente en el dashboard "Inicio": widget "Fichas activas" con conteo, tabla de "Horarios recientes en borrador" con columnas Ficha/Período/Nombre/Última edición/Acción, y en la pantalla "Disponibilidad" ("Reservado por ficha 123456").

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_ficha | INT | PK | Identificador interno |
| numero_ficha | VARCHAR(20) | — | Número visible al usuario, ej. "2874412", "3011550" — vistos literalmente en el dashboard |
| id_programa | INT | FK → `programa_formacion` | Programa que cursa la ficha |
| id_centro | INT | FK → `centro_formacion` | Centro donde opera la ficha |
| periodo | VARCHAR(10) | — | Ej. "2026-2", visto en la tabla de horarios en borrador |
| nombre_corto | VARCHAR(150) | — | Ej. "ADSO — Trimestre III", "Gestión administrativa", "Soldadura básica" — vistos literalmente |
| cantidad_aprendices | INT | — | 🔧 Supuesto de diseño: necesario para calcular el porcentaje de asistencia mostrado en seguimiento (monitoring), aunque no se ve en las tres pantallas recuperadas |
| estado | VARCHAR(30) | — | 🔧 Supuesto de diseño: `INDUCCION`, `EJECUCION`, `ETAPA_PRODUCTIVA`, `FINALIZADA` — el modelo necesita distinguir fichas activas de finalizadas, tal como filtra el dashboard al mostrar solo "Fichas activas" |

---

## 7. Dominio ENVIRONMENT — Ambientes de formación

### 7.1 Tabla `ambiente`

Confirmada en detalle en la pantalla "Disponibilidad": estado (Disponible/No disponible), tipo (Laboratorio/Aula/Taller), capacidad en personas, ubicación por bloque y piso.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_ambiente | INT | PK | Identificador del ambiente |
| nombre | VARCHAR(100) | — | Ej. "Laboratorio A-204", "Aula B-105", "Taller de soldadura" — vistos literalmente |
| tipo_ambiente | VARCHAR(30) | — | "Laboratorio", "Aula", "Taller" — vistos literalmente como filtro/etiqueta |
| capacidad | INT | — | Número de personas, ej. "30 personas" |
| ubicacion | VARCHAR(150) | — | Ej. "Bloque A, piso 2", "Zona industrial" |
| id_centro | INT | FK → `centro_formacion` | Centro al que pertenece el ambiente |
| activo | BOOLEAN | — | Habilita/deshabilita el ambiente |

### 7.2 Tabla `mantenimiento_ambiente`

🔧 **Supuesto de diseño:** la pantalla "Disponibilidad" muestra ambientes como "No disponible → Reservado por ficha 123456", que se explica solo con una sesión de clase ya asignada. Pero un ambiente también puede estar fuera de servicio por mantenimiento — esto no aparece en pantalla, pero es indispensable para que el estado "No disponible" tenga una segunda causa posible además de estar reservado, evitando que el sistema muestre como libre un ambiente dañado.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_mantenimiento | INT | PK | Identificador del periodo de mantenimiento |
| id_ambiente | INT | FK → `ambiente` | Ambiente afectado |
| fecha_inicio | DATETIME | — | Inicio del bloqueo |
| fecha_fin | DATETIME | — | Fin del bloqueo |
| motivo | VARCHAR(255) | — | Razón del mantenimiento |

---

## 8. Dominio ACTORS — Instructores y aprendices

### 8.1 Tabla `instructor`

Confirmada en detalle en la tabla de la pantalla "Disponibilidad": nombre, documento, área, horas máximas por semana y disponibilidad.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_instructor | INT | PK | Identificador del instructor |
| id_usuario | INT | FK → `usuario` | Cuenta de acceso del instructor |
| nombre_completo | VARCHAR(200) | — | Ej. "Juan Pérez", "Carolina Rojas" — vistos literalmente |
| numero_documento | VARCHAR(20) | — | Ej. "1.075.421.336" — visto literalmente en la tabla |
| area | VARCHAR(100) | — | Ej. "Software", "Bases de datos", "Calidad", "Electricidad" — vistos literalmente |
| horas_max_semana | INT | — | Ej. "40", "36" — vistos literalmente en la columna "Horas máx./semana" |
| activo | BOOLEAN | — | Habilita/deshabilita el instructor |

### 8.2 Tabla `aprendiz`

El sitemap confirma pantallas propias del rol "Aprendiz" (Mi horario, Notificaciones, Detalle de clase, Detalle de notificación), y el BPMN de Aprendiz muestra que consulta su propio horario, lo que exige vincular al aprendiz con una ficha.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_aprendiz | INT | PK | Identificador del aprendiz |
| id_usuario | INT | FK → `usuario` | Cuenta de acceso del aprendiz |
| id_ficha | INT | FK → `ficha` | Ficha a la que pertenece |
| nombre_completo | VARCHAR(200) | — | Nombre del aprendiz |
| numero_documento | VARCHAR(20) | — | Documento de identidad |
| estado_matricula | VARCHAR(30) | — | 🔧 Supuesto de diseño: `ACTIVO`, `RETIRADO`, `TRASLADADO`, `FINALIZADO` — necesario para que el sistema deje de mostrarle horario a un aprendiz que ya no está activo |

### 8.3 Tabla `excepcion_disponibilidad`

Se deduce cruzando dos evidencias: la columna "Disponibilidad" de la tabla de instructores en la pantalla "Disponibilidad" (que muestra el valor "Con excepción" junto a "Disponible"), y el diagrama BPMN del Instructor, que incluye explícitamente la actividad "Registrar Excepción de Disponibilidad" cuando una sesión se marca como "No Ejecutada". No hay una pantalla capturada con su nombre literal, así que se nombra de forma descriptiva.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_excepcion | INT | PK | Identificador de la excepción |
| id_instructor | INT | FK → `instructor` | Instructor que registra la excepción |
| fecha_inicio | DATETIME | — | Desde cuándo aplica |
| fecha_fin | DATETIME | — | Hasta cuándo aplica |
| motivo | VARCHAR(255) | — | Razón de la excepción (registrada por el instructor en el BPMN) |
| estado | VARCHAR(30) | — | 🔧 Supuesto de diseño: `EN_REVISION`, `APROBADA`, `RECHAZADA` — el diagrama termina en "Novedad en revisión", lo que implica un estado pendiente de aprobación por el coordinador |

---

## 9. Dominio SCHEDULING — Horarios y sesiones de clase

### 9.1 Tabla `horario`

Agregado central, confirmado en el dashboard: contador "Horarios en borrador (3)", tabla "Horarios recientes en borrador" con acción "Continuar edición", y el BPMN del Coordinador que arranca en "Crear Nuevo Horario (Borrador)" y termina en "Horario Publicado".

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_horario | INT | PK | Identificador del horario |
| id_ficha | INT | FK → `ficha` | Ficha para la que se construye el horario |
| id_usuario_creador | INT | FK → `usuario` | Coordinador que lo creó |
| fecha_creacion | DATETIME | — | Momento de creación del borrador |
| ultima_edicion | DATETIME | — | Ej. "Hoy, 08:14", "03/08/2026, 09:30" — vistos literalmente en la tabla del dashboard |
| estado | VARCHAR(20) | — | `BORRADOR`, `PUBLICADO` — corresponde a los dos estados que recorre el BPMN del Coordinador |
| fecha_publicacion | DATETIME | — | Se llena solo cuando pasa a `PUBLICADO` |

**Regla de negocio observada en el BPMN:** una vez publicado, el horario no puede modificarse directamente; cualquier corrección debe pasar de nuevo por el ciclo de creación de borrador. Esto se traduce en que ningún proceso debe hacer `UPDATE` sobre un `horario` en estado `PUBLICADO`.

### 9.2 Tabla `franja_horaria`

El formulario de "Disponibilidad" pide explícitamente fecha, hora de inicio y hora de fin, lo que confirma que las sesiones se agendan sobre bloques de tiempo definidos.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_franja | INT | PK | Identificador de la franja |
| dia_semana | INT | — | 1 = lunes … 7 = domingo |
| hora_inicio | TIME | — | Ej. "07:00 AM" — visto literalmente en el formulario de disponibilidad |
| hora_fin | TIME | — | Ej. "10:00 AM" — visto literalmente |

### 9.3 Tabla `sesion_clase`

Es la entidad que agrupa ficha + instructor + ambiente + franja. Se apoya en el BPMN del Coordinador ("Agregar Sesiones") y en el del Instructor ("Consultar Mi Horario", "Revisar Detalle de Sesión").

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_sesion_clase | INT | PK | Identificador de la sesión de clase |
| id_horario | INT | FK → `horario` | Horario al que pertenece |
| id_instructor | INT | FK → `instructor` | Instructor asignado |
| id_ambiente | INT | FK → `ambiente` | Ambiente asignado |
| id_franja | INT | FK → `franja_horaria` | Franja horaria asignada |
| id_competencia | INT | FK → `competencia` | Competencia que se dicta en la sesión |
| fecha_sesion | DATE | — | Fecha puntual de la clase |
| ejecutada | BOOLEAN | — | Se marca según el BPMN del Instructor ("Marcar como Ejecutada" / "Marcar como No Ejecutada") |

### 9.4 Tabla `conflicto_horario`

Confirmada literalmente en el dashboard, con tres tipos exactos listados: "Instructor doble-asignado", "Ambiente doble-asignado" y "Sesiones solapadas", cada uno con la ficha/franja afectada y la acción "Ver panel". El BPMN del Coordinador confirma el ciclo: "Validar Conflictos" → "¿Existen Conflictos?" → "Resolver Conflictos" → vuelve a "Agregar Sesiones".

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_conflicto | INT | PK | Identificador del conflicto |
| id_horario | INT | FK → `horario` | Horario donde se detectó |
| tipo_conflicto | VARCHAR(30) | — | `INSTRUCTOR_DOBLE_ASIGNADO`, `AMBIENTE_DOBLE_ASIGNADO`, `SESIONES_SOLAPADAS` — los tres textos vistos literalmente en el dashboard |
| id_sesion_a | INT | FK → `sesion_clase` | Primera sesión en conflicto |
| id_sesion_b | INT | FK → `sesion_clase` (nulo) | Segunda sesión en conflicto, si aplica |
| fecha_deteccion | DATETIME | — | Ej. "06/08/2026", "05/08/2026" — vistos literalmente |
| resuelto | BOOLEAN | — | Se marca en `true` cuando el coordinador ejecuta "Resolver Conflictos" en el BPMN |

---

## 10. Dominio MONITORING — Seguimiento e indicadores

### 10.1 Tabla `seguimiento_ficha`

El BPMN del Instructor termina, en su rama positiva, en "Registrar Seguimiento de Ficha" cuando una sesión sí se dictó. El sitemap confirma dos pantallas propias: "Seguimiento de ficha" y "Registrar seguimiento", ambas de módulo `monitoring`.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_seguimiento | INT | PK | Identificador del registro de seguimiento |
| id_ficha | INT | FK → `ficha` | Ficha objeto del seguimiento |
| id_sesion_clase | INT | FK → `sesion_clase` | Sesión que originó el registro |
| id_instructor | INT | FK → `instructor` | Instructor que lo registró |
| fecha_registro | DATE | — | Fecha del seguimiento |
| observaciones | VARCHAR(500) | — | Notas del instructor |

### 10.2 Tabla `indicador` (KPI) y `medicion_indicador`

El sitemap confirma, para el rol Director, las pantallas "Panel de indicadores" y "Drill-down de KPI", ambas de módulo `monitoring`.

| Tabla | Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|---|
| `indicador` | id_indicador | INT | PK | Ej. "Asistencia", "Avance curricular" |
| `indicador` | nombre | VARCHAR(100) | — | Nombre del indicador |
| `medicion_indicador` | id_medicion | INT | PK | Medición puntual |
| `medicion_indicador` | id_indicador | INT | FK → `indicador` | Indicador medido |
| `medicion_indicador` | id_ficha | INT | FK → `ficha` | Ficha medida |
| `medicion_indicador` | valor | DECIMAL(5,2) | — | Valor calculado, ej. "76%" |
| `medicion_indicador` | umbral | DECIMAL(5,2) | — | Umbral configurado, ej. "80%" |
| `medicion_indicador` | fecha_medicion | DATE | — | Momento de la medición |

### 10.3 Tabla `alerta`

🔧 **Supuesto de diseño:** el "Panel de indicadores" del Director necesita mostrar cuándo un indicador cruza su umbral; el sitemap no capturó el contenido exacto de esa pantalla, pero sin una tabla de alertas no habría forma de explicar el "drill-down" que aparece junto al panel.

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_alerta | INT | PK | Identificador de la alerta |
| id_medicion | INT | FK → `medicion_indicador` | Medición que disparó la alerta |
| severidad | VARCHAR(20) | — | `BAJA`, `MEDIA`, `ALTA` |
| resuelta | BOOLEAN | — | Indica si ya fue atendida |
| fecha_generacion | DATETIME | — | Momento en que se generó |

---

## 11. Dominio DOCUMENT — Documentos

🔧 **Supuesto de diseño general para este dominio:** el sitemap confirma, para el rol de soporte, pantallas de módulo `document` ("Documentos — listado", "Plantillas de documento", "Detalle de documento + versiones", "Modal generar documento"), pero ninguna quedó con su contenido capturado. Se incluye la estructura mínima necesaria para que "generar un documento" y "ver sus versiones" tengan sentido.

### 11.1 Tabla `documento`

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_documento | INT | PK | Identificador del documento |
| titulo | VARCHAR(200) | — | Título del documento generado |
| ruta_archivo | VARCHAR(300) | — | Ubicación del archivo en almacenamiento (el binario no se guarda en la base de datos) |
| id_usuario_creador | INT | FK → `usuario` | Quién lo generó |
| fecha_creacion | DATETIME | — | Momento de creación |

### 11.2 Tabla `version_documento`

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_version | INT | PK | Identificador de la versión |
| id_documento | INT | FK → `documento` | Documento padre |
| numero_version | INT | — | Consecutivo de versión |
| ruta_archivo | VARCHAR(300) | — | Ubicación de esta versión específica |
| fecha_version | DATETIME | — | Momento de creación de la versión |

---

## 12. Dominio AUDIT — Auditoría

El sitemap confirma, para el rol de soporte, las pantallas "Auditoría" y "Modal detalle de auditoría" (módulo `audit`). Además, varias acciones ya descritas en otros dominios exigen quedar auditadas: el BPMN del Director muestra literalmente que asignar un rol o revocar una sesión dispara una alerta de confirmación, y la tabla `sesion_activa` y `intento_acceso` ya cubren auditoría de acceso; esta tabla cubre la auditoría general de acciones de negocio.

### 12.1 Tabla `registro_auditoria`

| Atributo | Tipo de dato | Clave | Descripción |
|---|---|---|---|
| id_registro | INT | PK | Identificador del evento auditado |
| id_usuario | INT | FK → `usuario` (nulo) | Quién ejecutó la acción; nulo si la acción fue automática del sistema |
| tipo_evento | VARCHAR(100) | — | Ej. "horario.publicado", "rol.asignado" |
| entidad_afectada | VARCHAR(50) | — | Nombre de la tabla/entidad afectada |
| id_entidad_afectada | INT | — | Identificador del registro afectado |
| detalle | VARCHAR(500) | — | Descripción del cambio |
| fecha_evento | DATETIME | — | Momento exacto del evento |

**Invariante:** esta tabla es de solo inserción — ningún proceso del sistema debe actualizar ni borrar un `registro_auditoria` ya creado, para que conserve valor como evidencia.

---

## 13. Relaciones y cardinalidades — resumen general

```
centro_formacion (1) ────────── (N) ambiente
centro_formacion (1) ────────── (N) ficha

programa_formacion (1) ──────── (N) competencia
programa_formacion (1) ──────── (N) ficha

ficha (1) ────────────────────── (N) aprendiz
ficha (1) ────────────────────── (N) horario
ficha (1) ────────────────────── (N) seguimiento_ficha
ficha (1) ────────────────────── (N) medicion_indicador

horario (1) ──────────────────── (N) sesion_clase
horario (1) ──────────────────── (N) conflicto_horario

sesion_clase (N) ─────────────── (1) instructor
sesion_clase (N) ─────────────── (1) ambiente
sesion_clase (N) ─────────────── (1) franja_horaria
sesion_clase (N) ─────────────── (1) competencia
sesion_clase (1) ─────────────── (N) seguimiento_ficha

conflicto_horario (N) ────────── (2) sesion_clase   [id_sesion_a, id_sesion_b]

ambiente (1) ─────────────────── (N) mantenimiento_ambiente

instructor (1) ───────────────── (N) sesion_clase
instructor (1) ───────────────── (N) excepcion_disponibilidad
instructor (1) ───────────────── (1) usuario

aprendiz (N) ─────────────────── (1) ficha
aprendiz (1) ─────────────────── (1) usuario

medicion_indicador (N) ───────── (1) indicador
medicion_indicador (1) ───────── (N) alerta

usuario (1) ──────────────────── (1) rol
usuario (1) ──────────────────── (N) sesion_activa
usuario (1) ──────────────────── (N) registro_auditoria

catalogo (1) ─────────────────── (N) catalogo_detalle

documento (1) ────────────────── (N) version_documento
```

**Tablas que resuelven relaciones muchos a muchos:**
- `sesion_clase` resuelve la relación N:M entre `horario`, `instructor`, `ambiente` y `franja_horaria` (una sesión concreta es la combinación única de los cuatro).
- `conflicto_horario` resuelve la relación N:M entre pares de `sesion_clase` que chocan entre sí.
- `catalogo_detalle` resuelve la relación 1:N entre un `catalogo` y sus valores individuales.

---

## 14. Coherencia entre los procesos BPMN y el modelo de datos

| Paso del BPMN ya diagramado | Tabla(s) que lo soportan |
|---|---|
| Coordinador → "Consultar Fichas y Disponibilidad" | `ficha`, `ambiente`, `instructor`, `excepcion_disponibilidad` |
| Coordinador → "Crear Nuevo Horario (Borrador)" | `horario` (estado `BORRADOR`) |
| Coordinador → "Agregar Sesiones" | `sesion_clase` |
| Coordinador → "Validar Conflictos" / "Resolver Conflictos" | `conflicto_horario` |
| Coordinador → "Confirmar y Publicar" | `horario` (estado `PUBLICADO`, `fecha_publicacion`) |
| Instructor → "Marcar como Ejecutada" / "No Ejecutada" | `sesion_clase.ejecutada` |
| Instructor → "Registrar Seguimiento de Ficha" | `seguimiento_ficha` |
| Instructor → "Registrar Excepción de Disponibilidad" | `excepcion_disponibilidad` |
| Aprendiz → "Consultar Mi Horario" / "Revisar Notificaciones" | `sesion_clase`, (notificaciones — fuera del alcance capturado en el mockup, no se incluyó tabla por falta de evidencia suficiente) |
| Director → "Llenar formulario Asignar Rol" | `usuario`, `rol` |
| Director → "Revocar Sesión Activa" | `sesion_activa` |
| Director → "Editar Parámetro" | `parametro_sistema` |
| Login → "Validar usuario" / "Verificar Rol" / "Error 403" | `usuario`, `rol`, `intento_acceso` |

---

## 15. Supuestos de diseño — resumen consolidado

1. `ficha.cantidad_aprendices` y `ficha.estado`: no se ven en las tres pantallas recuperadas del mockup, pero son necesarios porque el dashboard ya filtra explícitamente "fichas activas".
2. `mantenimiento_ambiente`: la pantalla "Disponibilidad" solo muestra un ambiente "no disponible" por estar reservado; se agregó esta tabla porque un ambiente dañado o en mantenimiento debe poder marcarse como no disponible sin depender de una reserva.
3. `excepcion_disponibilidad.estado`: el BPMN del instructor termina en "Novedad en revisión", lo que obliga a que la excepción tenga un estado de aprobación pendiente, aunque el mockup no muestra la pantalla donde el coordinador la aprueba o rechaza.
4. `catalogo` / `catalogo_detalle`, `documento` / `version_documento` y `alerta`: corresponden a pantallas confirmadas por nombre en el sitemap (secciones de Parametrización, Back-office y Panel de indicadores), pero cuyo contenido detallado no se pudo recuperar del archivo exportado; se dejó la estructura mínima indispensable para que la funcionalidad nombrada en el sitemap sea posible.
5. Las columnas de auditoría estándar (`fecha_creacion`, `fecha_modificacion`, `usuario_creador`, `activo`) se omitieron por brevedad en varias tablas transaccionales, pero se asumen presentes en todas ellas.
6. No se incluyó una tabla de notificaciones: aunque el sitemap confirma pantallas "Notificaciones" y "Detalle de notificación" para el rol Aprendiz, ninguna de las tres pantallas con contenido recuperado muestra su estructura de datos, y se prefirió no inventar atributos sin ningún respaldo visual o de diagrama BPMN. Si el sistema real la requiere, debería agregarse como extensión posterior siguiendo el mismo patrón de `seguimiento_ficha` (entidad + relación con `usuario`).

---

## 16. Notas para el siguiente paso

Si el trabajo continúa hacia el diagrama entidad-relación o el script DDL, el orden de creación por dependencias sería:

1. `centro_formacion`, `rol`
2. `usuario`, `sesion_activa`, `intento_acceso`
3. `programa_formacion`, `competencia`, `ambiente`, `mantenimiento_ambiente`
4. `ficha`, `instructor`, `aprendiz`, `excepcion_disponibilidad`
5. `franja_horaria`, `horario`, `sesion_clase`, `conflicto_horario`
6. `seguimiento_ficha`, `indicador`, `medicion_indicador`, `alerta`
7. `documento`, `version_documento`, `catalogo`, `catalogo_detalle`
8. `registro_auditoria`
