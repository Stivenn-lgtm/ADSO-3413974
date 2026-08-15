## Analisis Panel Aprendiz MockUp (Sistema de Gestion de Horarios)

## Indice de contenido

- [Rol: Auth y Shell](#rol-auth-y-shell)
- [Rol: Aprendiz](#rol-aprendiz)
- [Rol: Instructor](#rol-instructor)
- [Rol: Coordinador](#rol-coordinador)
- [Administrador](#administrador)
- [Rol: Back-office](#rol-back-office)
- [Rol: Parametrizacion](#rol-parametrizacion)
- [Cierre del analisis general del sistema](#cierre-del-analisis-general-del-sistema)

El sistema busca complementar la gestion de horarios del SENA, centralizando la programacion academica y facilitando su consulta, creacion, modificacion y actualizacion.

Su objetivo es mejorar la organizacion y comunicacion entre los diferentes roles, permitiendo acceder a informacion actualizada de las formaciones de manera rapida y sencilla, sin reemplazar los sistemas institucionales existentes.

--------------------------------------------------------------------------------------

## Rol: Auth y Shell

Este apartado contiene las funciones generales que sirven como punto de entrada al sistema y que son compartidas por los diferentes usuarios.

### Alcance

El apartado esta compuesto por 6 pantallas:

1. Login
2. Recuperar contraseña
3. Nueva contraseña
4. App Shell por rol
5. Panel de notificaciones
6. Estados globales

Estas pantallas permiten realizar el ingreso, recuperar el acceso y utilizar los elementos generales de navegacion y comunicacion.

**Las pantallas son responsive**

### Flujograma Principal

El funcionamiento general puede seguir estos pasos:

1. El usuario ingresa sus datos de acceso.
2. Si no recuerda su contraseña, selecciona la opcion para recuperarla.
3. Establece una contraseña nueva.
4. Ingresa al sistema con las opciones correspondientes a su rol.
5. Consulta las notificaciones disponibles.
6. Revisa los estados que pueda mostrar el sistema.

De esta forma, Auth y Shell funciona como una base comun para los demas apartados.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se identifica facilmente el inicio de sesion, la recuperacion de contraseña y la entrada al sistema.
2. **Que no queda claro:** Algunos estados generales no explican de manera suficiente que significan o que efecto tienen.
3. **Que botones o textos sobran:** Las opciones principales son necesarias y no se observa una cantidad excesiva.
4. **Que informacion falta:** Seria bueno incluir una pequeña explicacion para los estados que puedan afectar una operacion.
5. **Que error podria cometer:** El usuario puede interpretar mal un estado o no prestar atencion a una notificacion.
6. **Que consecuencia tiene ese problema:** Podria tomar una decision equivocada o no enterarse de un cambio importante.

### Comparacion con SIGA

La propuesta mantiene el acceso y la navegacion como funciones generales, pero busca presentar la informacion de manera mas centralizada. Las notificaciones y los estados pueden organizarse dependiendo del usuario que ingrese.

Frente a SIGA, se puede mejorar la forma en que se comunican los avisos y los estados para que el usuario identifique rapidamente aquello que requiere su atencion.

### Reingenieria

Se propone:

* Mantener un inicio de sesion sencillo.
* Explicar los estados generales mediante informacion de apoyo.
* Conservar una recuperacion de contraseña directa.
* Organizar las notificaciones de acuerdo con el rol.

Con esto se busca que el ingreso al sistema sea sencillo y que la informacion general no genere confusiones.

--------------------------------------------------------------------------------------

## Rol: Aprendiz

El aprendiz es uno de los usuarios principales del sistema porque necesita consultar de forma constante la programacion de sus formaciones y conocer cualquier cambio que se realice.

### Alcance

El rol cuenta con 4 pantallas:

25. Mi horario - semana
26. Notificaciones
27. Detalle de clase
28. Detalle de notificacion

El objetivo principal es que el aprendiz tenga en un solo lugar su horario y las novedades relacionadas con este.

**Todas las pantallas son responsive**

### Flujograma Principal

El proceso para el aprendiz seria:

1. Ingresar con sus credenciales.
2. Entrar a la seccion de horario.
3. Revisar las formaciones programadas.
4. Abrir una sesion para conocer datos como competencia, instructor, ambiente, ubicacion, fecha y hora.
5. Revisar las notificaciones cuando exista alguna modificacion.
6. Entrar al detalle de una notificacion cuando necesite mas informacion.

El recorrido es corto y permite llegar rapidamente a la informacion que normalmente necesita un aprendiz.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se identifica donde esta el horario, las notificaciones y el detalle de cada formacion.
2. **Que no queda claro:** No siempre se distingue de inmediato cual es la siguiente formacion ni cuales avisos estan pendientes de revisar.
3. **Que botones o textos sobran:** En general la informacion presentada es necesaria y no se encuentra demasiado cargada.
4. **Que informacion falta:** Seria util mostrar el dia actual y datos del lider de la ficha.
5. **Que error podria cometer:** Podria confundir una notificacion ya revisada con una nueva.
6. **Que consecuencia tiene ese problema para el aprendiz:** Podria no darse cuenta de un cambio de horario, una cancelacion o una reprogramacion.

### Comparacion con SIGA

Al igual que SIGA, el MockUp permite consultar informacion academica y especialmente la programacion. La diferencia es que plantea una consulta mas directa y visual, agrupando horario y novedades en un mismo espacio.

Se podria mejorar la prioridad de la siguiente formacion y diferenciar mejor las notificaciones pendientes.

### Reingenieria

Se propone:

* Resaltar la proxima formacion.
* Diferenciar notificaciones leidas y pendientes.
* Mostrar el dia y la fecha actual.
* Agregar informacion del lider de ficha.
* Destacar cancelaciones y reprogramaciones.
* Incluir una opcion para consultar el cronograma fijo.
* Mantener una vista de horario similar a la del instructor.
* Permitir revisar el avance del proyecto formativo.
* Utilizar los terminos conocidos por los aprendices del SENA.

Estas mejoras ayudarian a que el aprendiz encuentre rapidamente lo que necesita y reduzca el riesgo de pasar por alto un cambio.

--------------------------------------------------------------------------------------

## Rol: Instructor

El instructor tiene funciones relacionadas con la consulta de su horario, el manejo de su disponibilidad y el seguimiento de las fichas que tiene a cargo.

### Alcance

El rol cuenta con 6 pantallas:

19. Mi horario - semana
20. Detalle de sesion
21. Mi disponibilidad
22. Modal crear excepcion
23. Seguimiento de ficha
24. Registrar seguimiento

Estas pantallas cubren tanto la consulta de la programacion como algunas acciones de gestion y seguimiento.

**Las pantallas son responsive**

### Flujograma Principal

1. Ingresar al sistema.
2. Revisar las sesiones programadas.
3. Abrir el detalle de una sesion.
4. Consultar la disponibilidad.
5. Registrar una excepcion cuando sea necesario.
6. Consultar el seguimiento de una ficha.
7. Registrar el seguimiento correspondiente.

El instructor no solo recibe informacion, sino que tambien participa en la actualizacion de datos del proceso formativo.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se encuentran facilmente el horario, la disponibilidad y el seguimiento.
2. **Que no queda claro:** La relacion entre una excepcion de disponibilidad y las sesiones que ya estan programadas.
3. **Que botones o textos sobran:** Las acciones principales se encuentran agrupadas de manera sencilla.
4. **Que informacion falta:** Seria conveniente mostrar mejor el estado de las sesiones, permitir adjuntar soportes a las excepciones y presentar un resumen del avance de la ficha.
5. **Que error podria cometer:** Podria registrar una excepcion sin identificar correctamente la ficha o realizar un seguimiento sobre una sesion equivocada.
6. **Que consecuencia tiene ese problema:** La programacion o el seguimiento podrian quedar con informacion incorrecta.

### Comparacion con SIGA

El MockUp organiza en un mismo espacio funciones que estan relacionadas con la programacion y el seguimiento. Esto facilita que el instructor consulte y gestione sus actividades sin tener que buscar la informacion en diferentes lugares.

La oportunidad de mejora se encuentra principalmente en conectar de forma mas clara disponibilidad, excepciones y horario.

### Reingenieria

Se propone:

* Relacionar la disponibilidad directamente con el horario.
* Mostrar las excepciones dentro del horario.
* Identificar ficha y sesion al registrar seguimiento.
* Presentar un resumen del avance de la ficha.
* Agregar una vista del horario fijo.
* Permitir adjuntar documentos validos en las excepciones.

De esta forma se tendria un mayor control sobre los cambios realizados por el instructor.

--------------------------------------------------------------------------------------

## Rol: Coordinador

El coordinador tiene una participacion importante porque se encarga de organizar los horarios, revisar conflictos y controlar elementos como ambientes y fichas.

### Alcance

El rol cuenta con 12 pantallas:

7. Dashboard / Inicio
8. Horarios - lista
9. Detalle de horario
10. Crear / editar horario
11. Modal agregar / editar sesion
12. Modal confirmar publicacion
13. Panel de conflictos
14. Modal resolver conflicto
15. Disponibilidad
16. Detalle de ambiente
17. Fichas - lista
18. Detalle de ficha

Estas funciones permiten trabajar desde la planeacion hasta la publicacion de una programacion.

**Las pantallas son responsive**

### Flujograma Principal

1. Ingresar mediante las credenciales.
2. Revisar el dashboard.
3. Consultar un horario existente o comenzar uno nuevo.
4. Agregar y modificar las sesiones.
5. Revisar los conflictos.
6. Solucionar los conflictos encontrados.
7. Comprobar la disponibilidad de ambientes.
8. Revisar la informacion de la ficha.
9. Validar la programacion.
10. Publicar el horario.

El coordinador es quien conecta gran parte de la planeacion academica.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se entiende donde crear horarios, agregar sesiones, revisar conflictos, consultar ambientes y consultar fichas.
2. **Que no queda claro:** Se podria explicar mejor que conflicto debe solucionarse primero y como una modificacion afecta el resto del horario.
3. **Que botones o textos sobran:** Las acciones se encuentran relacionadas con las tareas principales del coordinador.
4. **Que informacion falta:** Seria util contar con una vista general del horario por dia y semana.
5. **Que error podria cometer:** Puede intentar publicar sin haber revisado todos los conflictos o sin comprobar la disponibilidad.
6. **Que consecuencia tiene ese problema:** Podrian aparecer problemas de asignacion de ambientes, instructores o sesiones.

### Comparacion con SIGA

El MockUp busca complementar las funciones de programacion que ya maneja el SENA, organizando en un mismo recorrido la creacion, revision y publicacion de horarios.

Una mejora importante seria hacer mas visible la relacion entre conflictos, disponibilidad y publicacion para que el coordinador tenga mas seguridad antes de confirmar.

### Reingenieria

Se propone:

* Mostrar claramente el estado actual del horario.
* Incluir una vista de horario fijo.
* Permitir consultar los documentos que respaldan excepciones.
* Dar prioridad a los conflictos sin resolver.
* Relacionar ambientes y disponibilidad con las sesiones.
* Mostrar advertencias antes de publicar.
* Facilitar el recorrido horario -> sesion -> conflicto -> ambiente -> ficha.

Esto permitiria validar mejor la programacion antes de hacerla visible para los demas usuarios.

--------------------------------------------------------------------------------------

## Administrador

El administrador esta orientado al control general de la plataforma, principalmente en usuarios, roles, indicadores y datos de referencia.

### Alcance

El rol cuenta con 8 pantallas:

29. Panel de indicadores
30. Drill-down de KPI
31. Usuarios - lista
32. Crear / editar usuario
33. Detalle de usuario
34. Modal asignar / revocar rol
35. Datos de referencia
36. Editar catalogo / valor / parametro

Su funcion principal es supervisar y mantener configuraciones que afectan a otros apartados.

**Las pantallas son responsive**

### Flujograma Principal

1. Ingresar al sistema.
2. Consultar los indicadores.
3. Abrir el detalle de un KPI cuando se necesite mas informacion.
4. Consultar la lista de usuarios.
5. Crear, editar o revisar un usuario.
6. Asignar o retirar roles.
7. Administrar datos de referencia.
8. Actualizar valores y parametros.

El administrador trabaja principalmente sobre la configuracion y supervision del sistema.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se encuentran los indicadores, usuarios y datos de referencia.
2. **Que no queda claro:** Falta mostrar con mayor claridad que consecuencias puede tener una modificacion.
3. **Que botones o textos sobran:** Las opciones principales son directas y estan relacionadas con la funcion administrativa.
4. **Que informacion falta:** Podria mostrarse informacion adicional al revisar los graficos y el efecto de un cambio.
5. **Que error podria cometer:** Podria asignar un rol equivocado o cambiar un parametro que no correspondia.
6. **Que consecuencia tiene ese problema:** Un usuario podria obtener permisos incorrectos o un modulo podria funcionar de manera diferente.

### Comparacion con SIGA

El MockUp concentra las tareas de administracion necesarias para controlar usuarios, roles y configuraciones. Esto ayuda a separar las funciones administrativas de las tareas propias de la programacion academica.

### Reingenieria

Se propone:

* Mostrar el impacto de los cambios antes de guardarlos.
* Solicitar confirmacion para acciones importantes.
* Diferenciar roles y permisos de forma clara.
* Mostrar el estado de cada usuario.
* Ampliar la informacion de los graficos.
* Mantener un historial de modificaciones.

Con esto se puede mejorar el control de los cambios administrativos.

--------------------------------------------------------------------------------------

## Rol: Back-office

El Back-office esta pensado como un espacio de apoyo para administrar documentos, plantillas, auditoria y parametros utilizados por el sistema.

### Alcance

El rol cuenta con 9 pantallas:

37. Documentos - lista
38. Plantillas de documento
39. Auditoria
40. Parametrizacion / catalogos
41. Detalle de documento + versiones
42. Modal generar documento
43. Editor / preview de plantilla
44. Modal detalle de auditoria
45. CRUD catalogo / valor / parametro

Estas funciones permiten controlar informacion documental y operaciones de soporte.

**Las pantallas son responsive**

### Flujograma Principal

1. Ingresar al sistema.
2. Consultar documentos.
3. Revisar o crear plantillas.
4. Generar documentos.
5. Consultar versiones.
6. Revisar los registros de auditoria.
7. Administrar catalogos, valores y parametros.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Se identifican las secciones de documentos, plantillas, auditoria y catalogos.
2. **Que no queda claro:** Se podria explicar mejor como se relaciona una plantilla con los documentos que se generan.
3. **Que botones o textos sobran:** Las funciones estan separadas de acuerdo con cada actividad.
4. **Que informacion falta:** Seria bueno mostrar version, estado, responsable y fecha de modificacion.
5. **Que error podria cometer:** Podria modificar una plantilla o parametro que este siendo utilizado.
6. **Que consecuencia tiene ese problema:** Los documentos generados o algun proceso dependiente podria quedar incorrecto.

### Comparacion con SIGA

El MockUp agrega funciones de soporte relacionadas con documentos, auditoria y configuraciones. La centralizacion de estas actividades permite llevar un mayor control sobre la informacion que utiliza el sistema.

### Reingenieria

Se propone:

* Mostrar estado y version de los documentos.
* Guardar historial de cambios.
* Relacionar plantillas con documentos generados.
* Pedir confirmacion antes de modificar parametros importantes.
* Mostrar quien hizo un cambio y cuando.
* Facilitar el ingreso de datos para usuarios que no manejan formatos tecnicos.

--------------------------------------------------------------------------------------

## Rol: Parametrizacion

La parametrizacion es un apartado del sistema y no un rol independiente. Algunos usuarios autorizados pueden ingresar para configurar informacion que sirve de base para los demas modulos.

### Alcance

Cuenta con 8 pantallas:

46. Hub de parametrizacion
47. Curriculo academico
48. Jornadas / franjas horarias
49. Tipos de ambiente e inventario
50. Catalogos de monitoreo
51. Estados de actores
52. Geografia institucional
53. RBAC - roles y permisos

Aqui se definen datos y reglas que posteriormente son utilizados por los otros procesos.

**Las pantallas son responsive**

### Flujograma Principal

1. Ingresar al sistema.
2. Abrir el hub de parametrizacion.
3. Seleccionar la categoria que se desea modificar.
4. Consultar o actualizar los datos.
5. Guardar la configuracion.
6. Verificar que los cambios correspondan a las necesidades del sistema.

### Entendimiento de UI / UX

1. **Que entiende rapido:** Las diferentes categorias estan separadas y permiten encontrar la configuracion correspondiente.
2. **Que no queda claro:** No siempre se muestra que otros modulos dependen de un parametro.
3. **Que botones o textos sobran:** La organizacion por categorias facilita la navegacion.
4. **Que informacion falta:** Seria util indicar las dependencias de cada configuracion.
5. **Que error podria cometer:** Puede cambiar un dato que este siendo utilizado en otros procesos.
6. **Que consecuencia tiene ese problema:** Podrian aparecer inconsistencias en horarios, ambientes, permisos o informacion academica.

### Comparacion con SIGA

La propuesta organiza en un mismo apartado configuraciones academicas, administrativas y de seguridad. Esto permite que otros procesos utilicen una misma base de datos y reglas.

### Reingenieria

Se propone:

* Informar que procesos dependen de cada parametro.
* Pedir confirmacion antes de cambios importantes.
* Mantener un registro de modificaciones.
* Relacionar los parametros con los modulos que los utilizan.
* Diferenciar configuraciones activas e inactivas.

--------------------------------------------------------------------------------------

## Cierre del analisis general del sistema

### Hallazgos generales

Al revisar las 53 pantallas se puede observar que el sistema busca reunir diferentes procesos relacionados con la gestion de horarios y la formacion.

Los puntos que mas oportunidades presentan son:

- Informacion repartida entre varios apartados.
- Necesidad de destacar la informacion mas importante.
- Relacion poco visible entre horario, disponibilidad, ambientes y excepciones.
- Falta de mayor seguimiento sobre cambios realizados.
- Necesidad de validar informacion antes de publicar.
- Comunicacion que puede mejorar entre los diferentes roles.

### Matriz general de oportunidades

| Area | Oportunidad de mejora | Impacto |
| --- | --- | --- |
| Horarios | Centralizar y priorizar la informacion de las sesiones (Horario Fijo - Horario con Excepciones) | Facilita la consulta y reduce confusiones |
| Notificaciones | Diferenciar cambios y notificaciones pendientes | Evita ignorar informacion importante |
| Disponibilidad | Relacionarla directamente con la programacion | Reduce conflictos de horario |
| Conflictos | Priorizar y validar conflictos antes de publicar | Reduce errores en la programacion |
| Seguimiento | Mejorar la relacion entre ficha, sesion y seguimiento | Facilita el control del proceso formativo |
| Documentos | Incorporar versiones e historial de cambios | Mejora la trazabilidad |
| Administracion | Mostrar el impacto de modificaciones | Reduce errores de configuracion |
| Parametrizacion | Relacionar parametros con los modulos que utilizan | Facilita el control del sistema |
| Acceso y comunicacion | Centralizar autenticacion, notificaciones y estados | Mejora la experiencia general |

### Reingenieria general del sistema

La reingenieria busca conservar las funciones principales del MockUp, pero mejorar la relacion entre los procesos que actualmente se encuentran separados.

Se plantea:

1. Centralizar la gestion de horarios como eje principal del sistema (Horario Fijo - Horario con Excepciones).
2. Relacionar directamente horarios, sesiones, disponibilidad, ambientes y conflictos.
3. Incorporar validaciones antes de publicar o modificar una programacion.
4. Priorizar informacion importante mediante alertas, estados y notificaciones.
5. Mantener trazabilidad de cambios realizados en documentos, usuarios y parametros.
6. Mostrar el impacto de cambios administrativos o de parametrizacion.
7. Facilitar la navegacion entre los procesos relacionados.
8. Mantener una interfaz sencilla y adaptada a cada rol.
9. Utilizar los terminos propios del SENA para evitar confusiones.
10. Complementar los sistemas institucionales existentes sin reemplazarlos.

### Propuesta de flujo general reingenierizado

El flujo general podria organizarse de la siguiente manera:

**Parametrizacion -> Planeacion -> Validacion -> Publicacion -> Comunicacion -> Ejecucion -> Seguimiento -> Monitoreo**

### Parametrizacion

Se configuran los datos necesarios para que el resto del sistema pueda funcionar, como programas, jornadas, ambientes, estados, indicadores, ubicacion y permisos.

#### Planeacion

El coordinador organiza las sesiones y horarios teniendo en cuenta fichas, instructores, jornadas y ambientes.

#### Validacion

Antes de publicar se revisan conflictos, disponibilidad y posibles excepciones.

#### Publicacion

Cuando la programacion ya fue revisada, se publica el horario.

#### Comunicacion

Los aprendices, instructores y demas usuarios reciben la informacion que corresponda a su rol.

#### Ejecucion

El aprendiz consulta su horario y el instructor desarrolla sus actividades de acuerdo con la programacion.

#### Seguimiento

Se registra informacion sobre el avance de las fichas y las novedades del proceso formativo.

#### Monitoreo

Los usuarios administrativos consultan indicadores, auditoria, documentos y configuraciones para mantener el control.

### Relacion entre los roles

**Administrador / Parametrizacion**
-> Define usuarios, permisos, parametros y datos necesarios.

**Coordinador**
-> Organiza, revisa y publica los horarios.

**Instructor**
-> Consulta su programacion, gestiona disponibilidad y registra seguimiento.

**Aprendiz**
-> Consulta su horario y recibe las novedades.

**Back-Office**
-> Administra documentos, plantillas, auditoria y soporte.

La idea es que la informacion pueda pasar de una etapa a otra sin tener que repetirla.

### Resultado esperado

Con las mejoras planteadas se espera:

- Disminuir errores en la programacion.
- Reducir informacion repetida.
- Hacer mas facil la consulta de horarios.
- Mejorar la comunicacion entre los roles.
- Detectar conflictos antes de publicar.
- Tener mayor control sobre los cambios.
- Mejorar la trazabilidad.
- Mantener una interfaz sencilla para cada usuario.

### Conclusion

El analisis de las 53 pantallas permitio identificar que el MockUp presenta una propuesta integral para complementar la gestion de horarios y procesos relacionados del SENA. Cada rol participa en una parte diferente del proceso, pero existen oportunidades de mejora principalmente en la integracion, priorizacion, validacion y trazabilidad de la informacion.

La reingenieria propuesta no busca reemplazar los sistemas institucionales existentes, sino complementarlos mediante una herramienta que conecte los diferentes procesos y roles alrededor de la gestion de horarios, permitiendo un uso mas intuitivo y rapido por parte de los usuarios.

De esta manera, el sistema puede pasar de tener procesos separados a una solucion mas conectada, donde la informacion fluya desde la parametrizacion y planeacion hasta la publicacion, comunicacion, seguimiento y monitoreo de las formaciones.
