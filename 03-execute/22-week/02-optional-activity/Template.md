# Revisión y oportunidades de mejora - Sistema de Gestión de Horarios SENA

## Introducción

En este documento se presentan diferentes oportunidades de mejora encontradas durante la revisión del prototipo del Sistema de Gestión de Horarios del SENA.

El análisis se enfoca principalmente en la forma en que están organizadas las pantallas, los recorridos que debe realizar cada usuario y la manera en que se presenta la información.

A partir de la revisión realizada se identificaron situaciones que podrían mejorarse para facilitar el uso del sistema, reducir pasos innecesarios y hacer más claras algunas acciones importantes.

Las propuestas planteadas buscan realizar cambios concretos sobre los procesos existentes, evitando agregar funciones que no sean necesarias para solucionar los problemas encontrados.

---

# 1. Forma de realizar el análisis

El prototipo cuenta con diferentes pantallas y recorridos destinados a varios tipos de usuarios. Para realizar la revisión se observaron las tareas que cada rol debe completar y la información que necesita durante el proceso.

Se tuvieron en cuenta principalmente los siguientes aspectos:

- Organización de las pantallas.
- Facilidad para encontrar información.
- Cantidad de pasos necesarios.
- Claridad de las acciones.
- Mensajes y estados mostrados al usuario.
- Relación entre las diferentes funciones.
- Posibles mejoras que puedan realizarse sin modificar completamente el sistema.

Los documentos Markdown revisados anteriormente también sirvieron como apoyo para identificar situaciones repetidas y comparar diferentes propuestas.

El criterio utilizado para seleccionar las mejoras fue:

**Usuario -> Tarea -> Situación encontrada -> Propuesta -> Resultado esperado**

---

# 2. Aspectos utilizados para revisar cada oportunidad

## 2.1 Usuario y actividad

Primero se identifica el usuario que utiliza la función y la actividad que necesita realizar.

De esta manera se puede entender qué espera conseguir el usuario dentro del sistema.

## 2.2 Situación encontrada

Se describe el punto del recorrido donde puede presentarse una dificultad.

La situación debe estar relacionada con una pantalla, elemento o proceso que aparezca dentro del prototipo.

## 2.3 Evidencia

Se toma como referencia lo que está representado en las pantallas y en los recorridos revisados.

Al tratarse de un prototipo, el análisis se concentra en la experiencia que representa la interfaz.

## 2.4 Propuesta de mejora

Se plantea una modificación que permita solucionar la situación encontrada.

La idea principal es aprovechar los elementos que ya existen en el sistema antes de crear nuevas funciones.

## 2.5 Funcionalidad necesaria

Se describe qué debería hacer el sistema si posteriormente estas mejoras fueran implementadas.

Esto permite diferenciar entre lo que actualmente muestra el prototipo y lo que tendría que funcionar en una versión real.

## 2.6 UX/UI

Se revisan elementos relacionados con:

- Organización de la información.
- Navegación.
- Jerarquía.
- Mensajes.
- Estados.
- Retroalimentación.
- Consistencia.

La intención es mejorar el desarrollo de las tareas y no realizar cambios solamente por apariencia.

## 2.7 Límites de la propuesta

También se establece qué funciones no serían necesarias.

Esto permite evitar que una solución sencilla termine convirtiéndose en un proceso demasiado complejo.

## 2.8 Relación con SIGA

Las mejoras pueden relacionarse con aspectos como orientación a las personas, eficiencia, resultados, seguimiento, control y mejora continua.

La intención es mostrar cómo una modificación puede aportar al proceso que realiza el usuario.

## 2.9 Resultado esperado

Cada oportunidad debe producir un resultado que pueda comprobarse posteriormente cuando la mejora sea implementada o probada con usuarios.

---

# 3. Revisión general de los documentos

Los 17 archivos Markdown revisados presentan diferentes formas de analizar el sistema.

Algunos se enfocan en la interfaz, otros describen recorridos, otros plantean necesidades de los usuarios y algunos relacionan las propuestas con SIGA.

Al comparar los documentos se encontraron tres elementos que deben mantenerse separados:

**Observación -> Necesidad -> Solución**

La observación corresponde a lo que se puede identificar en el sistema.

La necesidad representa aquello que el usuario requiere para completar correctamente una tarea.

La solución corresponde al cambio que podría realizarse para atender esa necesidad.

No todas las observaciones requieren crear una nueva función. De la misma manera, no todos los problemas necesitan una pantalla adicional.

Por esta razón, las oportunidades seleccionadas buscan solucionar situaciones concretas sin aumentar innecesariamente la complejidad del sistema.

---

# 4. Matriz de oportunidades identificadas

| Código | Usuario/área | Oportunidad de mejora | Prioridad |
|---|---|---|---|
| H01 | Aprendiz | Mostrar de forma prioritaria la próxima actividad | Alta |
| H02 | Aprendiz | Relacionar las notificaciones con la actividad afectada | Alta |
| H03 | Instructor | Mostrar las sesiones relacionadas con una excepción | Alta |
| H04 | Instructor | Mantener visible el contexto durante el seguimiento | Media |
| H05 | Coordinador | Facilitar la comprensión y solución de conflictos | Alta |
| H06 | Coordinador | Revisar el estado del horario antes de publicarlo | Alta |
| H07 | Coordinador | Consultar disponibilidad durante la asignación | Media |
| H08 | Coordinador | Diferenciar creación y edición de sesiones | Media |
| H09 | Administrador | Informar el impacto de cambios importantes | Media |
| H10 | Back-office | Mostrar la vigencia de las plantillas | Media |
| H11 | Parametrización | Presentar el contexto antes de modificar configuraciones | Media |
| H12 | Transversal | Mejorar los mensajes y estados del sistema | Alta |
| H13 | Transversal | Mantener el contexto entre pantallas relacionadas | Alta |
| H14 | Transversal | Unificar las formas de interacción | Media |

---

# 5. H01 - Aprendiz: identificar la próxima actividad

## Usuario y actividad

**Rol:** Aprendiz.  
**Proceso:** Consulta del horario.  
**Pantallas relacionadas:** Mi horario y detalle de clase.

El aprendiz ingresa al sistema para consultar sus actividades programadas. Cuando necesita más información, puede seleccionar una de las actividades y consultar su detalle.

## Situación encontrada

La vista semanal permite observar las actividades programadas, pero el usuario puede tener dificultad para reconocer rápidamente cuál es la actividad que tiene a continuación.

La necesidad principal es responder de manera sencilla:

**"¿Qué actividad tengo después?"**

## Evidencia

En los análisis revisados se identifica la dificultad para reconocer la próxima formación. También aparecen propuestas relacionadas con la priorización de la información dentro del horario.

El problema está principalmente relacionado con la organización de la información disponible.

## Propuesta

Agregar dentro de la pantalla actual del horario una sección pequeña que indique la próxima actividad.

La información puede utilizar los mismos datos que ya aparecen en el calendario.

## Funcionalidad mínima

El sistema debería:

- Identificar la próxima actividad.
- Mostrar la fecha.
- Mostrar la hora.
- Mostrar la actividad.
- Mostrar el instructor cuando esté disponible.
- Mostrar el lugar cuando esté disponible.
- Permitir acceder al detalle existente.

## UX/UI

La próxima actividad puede aparecer antes del calendario para que sea encontrada rápidamente.

El calendario continuaría siendo el elemento principal de consulta.

## No se propone

No sería necesario crear:

- Un asistente virtual.
- Un módulo independiente de actividades.
- Otro calendario diferente.

## Relación con SIGA

La mejora facilita el acceso a la información que necesita el aprendiz y favorece la orientación hacia el usuario.

## Resultado esperado

El aprendiz puede conocer rápidamente cuál es su siguiente actividad sin revisar toda la semana.

---

# 6. H02 - Aprendiz: notificaciones relacionadas con las actividades

## Usuario y actividad

**Rol:** Aprendiz.  
**Proceso:** Consulta de cambios en el horario.

El aprendiz recibe una notificación, revisa el mensaje y necesita identificar qué actividad fue modificada.

## Situación encontrada

Una notificación general puede informar que existe un cambio, pero si no muestra cuál actividad fue afectada, el usuario tiene que buscarla nuevamente.

## Evidencia

El mockup presenta una sección de notificaciones y una vista de detalle.

Los análisis anteriores también señalan la posibilidad de que cambios importantes no sean identificados fácilmente.

## Propuesta

Relacionar cada notificación con la actividad que originó el cambio.

La información principal sería:

**Qué cambió -> Cuándo aplica -> Qué actividad afecta -> Ver detalle**

## Funcionalidad mínima

La notificación debería:

- Estar relacionada con el elemento afectado.
- Diferenciar las notificaciones pendientes.
- Mostrar claramente qué ocurrió.
- Permitir acceder al detalle correspondiente.

## UX/UI

Las novedades importantes deben tener una jerarquía visual clara.

Por ejemplo, una cancelación o reprogramación debería ser fácil de identificar frente a una notificación informativa.

## No se propone

No es necesario crear para solucionar este problema:

- Envíos por correo.
- SMS.
- Otros canales de comunicación.
- Una nueva bandeja independiente.

## Relación con SIGA

La mejora permite que la información llegue al usuario de forma más clara y oportuna.

## Resultado esperado

El aprendiz puede pasar directamente desde la notificación hasta la actividad que fue modificada.

---

# 7. H03 - Instructor: relacionar una excepción con las sesiones afectadas

## Usuario y actividad

**Rol:** Instructor.  
**Proceso:** Registro de una excepción de disponibilidad.

El instructor revisa su disponibilidad y registra una excepción cuando no puede atender una determinada franja.

## Situación encontrada

Las opciones relacionadas con disponibilidad y horario están separadas. Por esto, el instructor puede no saber inmediatamente si su registro afecta alguna sesión.

## Evidencia

Dentro del mockup aparecen las funciones:

- Mi disponibilidad.
- Crear excepción.
- Mi horario.

Los análisis revisados identifican una oportunidad para relacionar estos elementos.

## Propuesta

Después de guardar una excepción, mostrar las sesiones que coincidan con el periodo registrado.

La propuesta no contempla modificar automáticamente las sesiones.

## Funcionalidad mínima

El sistema debería:

1. Guardar la excepción.
2. Compararla con las sesiones programadas.
3. Identificar las coincidencias.
4. Mostrar las sesiones que podrían verse afectadas.

## UX/UI

El resultado debería aparecer inmediatamente después de guardar.

Cuando no existan coincidencias, se puede informar que no se encontraron sesiones afectadas.

Cuando existan, se debe mostrar un resumen de ellas.

## No se propone

No se plantea implementar:

- Sustitución automática del instructor.
- Reprogramación automática.
- Optimización automática del horario.

## Relación con SIGA

Permite relacionar actividades del mismo proceso y mejora las posibilidades de seguimiento y control.

## Resultado esperado

El instructor puede conocer las sesiones que podrían verse afectadas por la excepción registrada.

---

# 8. H04 - Instructor: conservar el contexto del seguimiento

## Usuario y actividad

**Rol:** Instructor.  
**Proceso:** Registro de seguimiento.

El instructor selecciona una ficha, entra a la sección de seguimiento, registra la información y guarda.

## Situación encontrada

Al pasar de una pantalla a otra puede perderse parte de la referencia sobre la ficha o actividad en la que se está trabajando.

## Evidencia

El mockup contiene las funciones **Seguimiento de ficha** y **Registrar seguimiento** como partes relacionadas del proceso.

## Propuesta

Mantener visible durante el registro la información principal:

**Ficha -> Programa -> Actividad/Sesión**

## Funcionalidad mínima

El registro debe quedar relacionado con el contexto que fue seleccionado inicialmente.

## UX/UI

Se puede utilizar un encabezado contextual que permanezca visible mientras el instructor completa el formulario.

Esto evita tener que volver a la pantalla anterior para comprobar la información.

## No se propone

No es necesario:

- Repetir la información en diferentes lugares.
- Crear un módulo adicional solamente para mostrar el contexto.

## Relación con SIGA

La mejora favorece la trazabilidad y puede disminuir errores o reprocesos.

## Resultado esperado

El instructor puede registrar el seguimiento sabiendo claramente a qué ficha o actividad pertenece.

---

# 9. H05 - Coordinador: conflictos que indiquen cómo actuar

## Usuario y actividad

**Rol:** Coordinador académico.  
**Proceso:** Revisión y modificación del horario.

El coordinador crea o modifica una sesión y puede encontrarse con situaciones que generan conflictos.

## Situación encontrada

Una alerta que solamente indique que existe un conflicto no explica suficientemente qué debe revisar el coordinador.

## Evidencia

Los documentos revisados mencionan conflictos relacionados con instructores y otros recursos.

El hallazgo se refiere a la comunicación del conflicto y no a afirmar que exista un algoritmo específico funcionando dentro del prototipo.

## Propuesta

Los mensajes de conflicto deberían explicar:

**Qué sucede -> Qué elemento está involucrado -> Por qué sucede -> Qué puede revisar el usuario**

## Funcionalidad mínima

La solución debería relacionar el conflicto con la sesión correspondiente y señalar el elemento que necesita atención.

## UX/UI

Los mensajes pueden organizarse por importancia:

- **Crítico:** impide continuar.
- **Advertencia:** necesita revisión.
- **Informativo:** comunica una situación.

## No se propone

No se plantea:

- Crear un asistente inteligente.
- Generar automáticamente otro horario.
- Sustituir recursos automáticamente sin reglas institucionales confirmadas.

## Relación con SIGA

La mejora puede disminuir reprocesos y favorecer una gestión más eficiente.

## Resultado esperado

El coordinador puede entender mejor el conflicto y dirigirse al lugar donde debe realizar la corrección.

---

# 10. H06 - Coordinador: revisar antes de publicar

## Usuario y actividad

**Rol:** Coordinador.  
**Proceso:** Publicación del horario.

El coordinador completa las sesiones, revisa la programación y finalmente publica el horario.

## Situación encontrada

La publicación es una acción importante. Antes de realizarla, sería útil conocer rápidamente si existen problemas pendientes.

## Evidencia

El prototipo contempla funciones relacionadas con la publicación y con la identificación de conflictos.

## Propuesta

Antes de confirmar la publicación, presentar un resumen del estado del horario:

- Sesiones completas.
- Conflictos críticos.
- Advertencias.
- Resultado de la validación.

## Funcionalidad mínima

El sistema debería revisar las condiciones correspondientes y mostrar el resultado antes de confirmar.

El usuario debería poder cancelar o continuar.

## UX/UI

La información debe responder de manera rápida:

**¿Qué voy a publicar?**  
**¿Hay problemas?**  
**¿Puedo continuar?**

## No se propone

No se requiere:

- Un dashboard adicional.
- Una etapa de aprobación nueva si no existe una regla institucional que la solicite.

## Relación con SIGA

La propuesta fortalece el seguimiento y el control antes de completar una acción importante del proceso.

## Resultado esperado

El coordinador puede publicar teniendo claro el estado del horario.

---

# 11. H07 - Coordinador: disponibilidad dentro del proceso de asignación

## Usuario y actividad

**Rol:** Coordinador.  
**Proceso:** Asignación de instructor o ambiente.

El coordinador selecciona una sesión y define los recursos que necesita para ella.

## Situación encontrada

Cuando la información de disponibilidad se encuentra separada, el usuario puede tener que cambiar de pantalla y perder la referencia de la sesión que está configurando.

## Evidencia

El flujo revisado contempla la selección de sesiones y posteriormente la asignación de recursos.

## Propuesta

Mostrar la disponibilidad mientras se realiza la asignación.

## Funcionalidad mínima

El usuario debería poder consultar la disponibilidad del recurso seleccionado sin abandonar la sesión actual.

## UX/UI

La información debe aparecer como apoyo a la decisión, manteniendo visible la referencia de la sesión.

## No se propone

No se plantea:

- Crear un sistema independiente de disponibilidad.
- Automatizar todas las asignaciones.

## Relación con SIGA

La mejora puede contribuir a la eficiencia del proceso y disminuir consultas repetitivas.

## Resultado esperado

El coordinador puede tomar decisiones de asignación con la disponibilidad relacionada a la vista.

---

# 12. H08 - Coordinador: diferenciar crear y editar una sesión

## Usuario y actividad

**Rol:** Coordinador.  
**Proceso:** Gestión de sesiones.

El coordinador puede necesitar registrar una sesión nueva o modificar una que ya existe.

## Situación encontrada

Cuando las acciones de creación y edición utilizan recorridos demasiado similares, el usuario puede no tener claridad sobre qué operación está realizando.

## Propuesta

Diferenciar claramente desde el inicio si se está:

- Creando una sesión.
- Editando una sesión existente.

La información que ya existe debe conservarse cuando se trate de una edición.

## Funcionalidad mínima

El sistema debe reconocer la operación que se está realizando y mostrar los datos correspondientes.

## UX/UI

El título, los botones y los mensajes deben indicar claramente si se trata de crear o modificar.

## No se propone

No es necesario crear dos sistemas separados para realizar estas operaciones.

## Relación con SIGA

La diferenciación puede reducir errores y reprocesos durante la gestión del horario.

## Resultado esperado

El coordinador sabe qué operación está realizando antes de guardar.

---

# 13. H09 - Administrador: mostrar el impacto de cambios importantes

## Usuario y actividad

**Rol:** Administrador.  
**Proceso:** Modificación de configuraciones o elementos sensibles.

El administrador realiza cambios que pueden tener efectos sobre otros elementos del sistema.

## Situación encontrada

Antes de confirmar un cambio importante, el usuario debería poder conocer de forma resumida qué elementos podrían verse afectados.

## Propuesta

Mostrar una confirmación que explique el impacto principal antes de guardar.

## Funcionalidad mínima

La confirmación debe indicar el cambio que se realizará y, cuando corresponda, los elementos relacionados.

## UX/UI

El mensaje debe ser claro y evitar información innecesaria.

## No se propone

No se plantea crear un proceso complejo de aprobación si no existe una necesidad institucional que lo justifique.

## Relación con SIGA

La mejora favorece el control y permite tomar decisiones con mayor información.

## Resultado esperado

El administrador puede confirmar cambios importantes entendiendo previamente sus posibles efectos.

---

# 14. H10 - Back-office: mostrar la vigencia de las plantillas

## Usuario y actividad

**Rol:** Back-office.  
**Proceso:** Administración de plantillas.

El usuario gestiona plantillas que pueden utilizarse dentro del sistema.

## Situación encontrada

Si no se identifica claramente cuáles plantillas están vigentes, puede existir confusión al momento de seleccionar una para utilizarla.

## Propuesta

Mostrar de forma visible el estado o periodo de vigencia de cada plantilla.

## Funcionalidad mínima

Cada plantilla debe indicar si está vigente o si ya no corresponde al periodo actual.

## UX/UI

El estado debe aparecer directamente en el listado, evitando que el usuario tenga que abrir cada elemento para comprobarlo.

## No se propone

No se plantea crear un sistema adicional para administrar las vigencias.

## Relación con SIGA

La mejora favorece el control de la información utilizada dentro del proceso.

## Resultado esperado

El usuario puede identificar rápidamente cuál plantilla corresponde utilizar.

---

# 15. H11 - Parametrización: mostrar información antes de modificar

## Usuario y actividad

**Rol:** Usuario encargado de parametrización.  
**Proceso:** Cambio de configuración.

El usuario consulta una configuración y puede modificar sus valores.

## Situación encontrada

Modificar un parámetro sin tener suficiente contexto puede generar dudas sobre qué parte del sistema será afectada.

## Propuesta

Mostrar información contextual antes de confirmar la modificación.

## Funcionalidad mínima

La pantalla debe indicar qué configuración se está modificando y cuáles son sus valores actuales.

## UX/UI

La información debe aparecer antes de guardar y acompañar al usuario durante la modificación.

## No se propone

No se plantea agregar una pantalla adicional para cada configuración.

## Relación con SIGA

La propuesta fortalece el control y disminuye la posibilidad de realizar cambios sin suficiente información.

## Resultado esperado

El usuario puede modificar una configuración teniendo claro qué elemento está cambiando.

---

# 16. H12 - Transversal: mejorar mensajes y estados

## Usuarios

Todos los roles.

## Situación encontrada

Los mensajes del sistema cumplen una función importante para informar al usuario si una acción fue realizada, si existe un problema o si necesita realizar algún paso adicional.

Cuando estos mensajes no son suficientemente claros, el usuario puede no saber qué ocurrió.

## Propuesta

Unificar la forma de comunicar estados como:

- Éxito.
- Error.
- Advertencia.
- Información.
- Procesamiento.

## Funcionalidad mínima

Cada estado debe indicar claramente qué ocurrió y, cuando sea necesario, qué puede hacer el usuario después.

## UX/UI

Los mensajes deben mantener una estructura similar en todo el sistema.

## No se propone

No se necesita crear una sección nueva únicamente para mostrar mensajes.

## Relación con SIGA

Una comunicación más clara favorece la eficiencia y disminuye reprocesos.

## Resultado esperado

Los usuarios pueden interpretar con mayor facilidad el resultado de sus acciones.

---

# 17. H13 - Transversal: mantener el contexto entre pantallas

## Usuarios

Todos los roles.

## Situación encontrada

Al pasar de una pantalla a otra dentro de un mismo proceso, el usuario puede perder la referencia sobre el elemento que estaba gestionando.

## Propuesta

Mantener visible información básica del contexto mientras se avanza por el proceso.

Por ejemplo:

**Elemento seleccionado -> Acción actual -> Información relacionada**

## Funcionalidad mínima

Las pantallas relacionadas deben conservar la referencia del elemento seleccionado.

## UX/UI

Se pueden utilizar títulos, encabezados o rutas de navegación para indicar dónde se encuentra el usuario.

## No se propone

No es necesario repetir todos los datos en cada pantalla.

## Relación con SIGA

La mejora puede favorecer la trazabilidad y reducir errores durante el proceso.

## Resultado esperado

El usuario puede desplazarse entre pantallas sin perder la referencia de lo que está gestionando.

---

# 18. H14 - Transversal: mantener una interacción consistente

## Usuarios

Todos los roles.

## Situación encontrada

Cuando elementos similares funcionan de manera diferente en distintas pantallas, el usuario tiene que aprender nuevamente cómo utilizar cada sección.

## Propuesta

Mantener criterios similares para elementos que cumplen funciones equivalentes.

Por ejemplo:

- Botones con acciones semejantes.
- Formularios.
- Mensajes.
- Confirmaciones.
- Navegación.
- Estados.

## Funcionalidad mínima

Los componentes equivalentes deben mantener comportamientos similares.

## UX/UI

La consistencia debe aplicarse sin modificar innecesariamente los elementos que ya funcionan correctamente.

## No se propone

No se plantea rediseñar completamente todas las pantallas.

## Relación con SIGA

Una interacción consistente puede reducir errores y facilitar el uso del sistema.

## Resultado esperado

Los usuarios pueden reconocer más fácilmente cómo funcionan los elementos del sistema.

---

# 19. Conclusión

Después de revisar las diferentes oportunidades, se puede observar que varias mejoras pueden realizarse sobre los procesos que ya existen sin necesidad de crear módulos completamente nuevos.

Los principales puntos de mejora están relacionados con:

- Priorizar la información importante.
- Mantener el contexto durante los recorridos.
- Conectar funciones que hacen parte del mismo proceso.
- Hacer que las notificaciones permitan llegar directamente a la información afectada.
- Explicar mejor los conflictos.
- Mostrar el estado antes de realizar acciones importantes.
- Mantener mensajes y comportamientos consistentes.
- Evitar funciones adicionales que no sean necesarias.

La reingeniería propuesta debe enfocarse en resolver problemas concretos encontrados en el prototipo y no en agregar funcionalidades solamente por ampliar el sistema.

Como criterio final:

**Una mejora es pertinente cuando existe una situación identificable, puede justificarse mediante el análisis realizado, tiene una solución razonable y genera un resultado que posteriormente pueda comprobarse.**