# Alcance MVP — FlowSync

> Pedido de partida: «Quiero que FlowSync sea una herramienta para que los equipos remotos sepan en qué está trabajando cada uno sin tener que hacer reuniones de sincronización. Algo tipo tareas compartidas pero más en tiempo real y menos rollo que Jira.»

## 1. El terreno que ya existe

FlowSync hoy solo resuelve **identidad**: una persona se registra (nombre opcional, email y contraseña), inicia sesión, ve su perfil y cierra sesión. No existe nada del dominio de trabajo: ni tareas, ni equipos, ni espacios. El modelo de datos actual contiene únicamente usuarios y sus sesiones. El MVP reutiliza esa identidad tal cual y construye encima; no se vuelve a especificar el registro ni el login.

## 2. El interrogatorio, con las respuestas decididas

1. **¿Qué tiene que dejar de pasar?** La ronda de "¿en qué estás?" de la daily (la mitad de sus 15 min) y las preguntas por Slack. La parte de bloqueos de la daily sigue: este MVP no la resuelve.
2. **¿La unidad es la tarea o la persona?** La tarea. Frescura, no presencia: nada de "quién está conectado" (es vigilancia, rechazada a propósito).
3. **¿Qué es "tiempo real"?** Ver los cambios de estado de las tareas sin refrescar ni preguntar. La señal es un resumen que espera, no un aviso que interrumpe: sin notificaciones.
4. **¿Cómo es el equipo?** 3–10 personas remotas, roles planos. Caso de estudio: 6 personas, SaaS, 3 husos horarios. Un único espacio compartido.
5. **¿Qué mínimo lo hace usable y qué queda fuera?** Una tarea tiene título, responsable, estado y vencimiento; la lista se filtra por estado. Fuera: sprints, estimaciones, épicas, backlog priorizado, informes e integraciones.

**Supuestos declarados** (lo que la ficha no cubría):

- S1. Tres estados fijos, no configurables: *Pendiente → En curso → Hecha*.
- S2. Solo el título es obligatorio; estado arranca en *Pendiente*; responsable y vencimiento opcionales.
- S3. Tarea sin responsable = libre; cualquiera la toma asignándosela.
- S4. Roles planos: cualquiera crea, edita, reasigna y archiva.
- S5. Si dos personas tocan la misma tarea, gana el último cambio y los demás lo ven al momento.
- S6. "Qué se movió" = lo cambiado desde mi última visita, resaltado en la misma lista.
- S7. Vista por defecto: todo lo que no está *Hecha* ni archivado; el filtro es opcional.
- S8. "Vencida" = tiene fecha, ya pasó y no está *Hecha*. Se marca, no avisa.
- S9. Archivar oculta la tarea de la lista sin perderla: se ven con el filtro "Archivadas" y se pueden desarchivar. Archivar ≠ *Hecha*: es para lo que ya no aplica o se creó por error.
- S10. Todo usuario registrado entra al único espacio (sin invitaciones). Válido para el caso de estudio; no para un producto real.
- S11. Varios equipos, o personas en más de uno, quedan fuera: supuesto de un único espacio.
- S12. Web usable desde el móvil; sin app nativa.

## 3. Alcance en cinco bloques

### Problema

En un equipo remoto nadie ve en qué está cada uno sin interrumpir a alguien. Cuesta la mitad de la daily y genera trabajo duplicado: dos personas tocaron el mismo módulo la misma semana sin saberlo y se perdieron dos días.

### Usuarios

Pares de un equipo remoto de 3–10 personas, sin jerarquía: todos actualizan su trabajo y consultan el de los demás. Caso de estudio: 6 personas, producto SaaS, 3 husos horarios, hoy con un gestor pesado y daily por videollamada. **No** es usuario: un lead que necesita reportes ni un equipo que necesita sprints o estimaciones.

### Propuesta de valor

Una lista compartida que es a la vez **tu cola de trabajo** y **el estado del equipo**. Tomar una tarea o cambiarle el estado son dos clics; los demás lo ven sin preguntar ni refrescar, y al volver ves qué se movió. Quien actualiza cobra en el momento: organiza su trabajo y deja de recibir preguntas.

**Hipótesis:** si actualizar cuesta dos clics, el estado se mantiene fresco solo; y si se mantiene fresco, en una semana el equipo cancela la ronda de "¿en qué estás?" y nadie pide que vuelva. **Riesgo #1:** que el estado se quede viejo.

### Alcance

1. Crear una tarea escribiendo solo el título.
2. Lista única del espacio con título, responsable, estado y vencimiento; por defecto, lo no terminado.
3. Cambiar el estado desde la lista en un clic.
4. Tomar una tarea libre (asignármela) o reasignarla en un clic.
5. La lista se actualiza sola: el cambio de otro aparece en segundos, sin refrescar.
6. Marca de "vencida".
7. Filtrar por estado.
8. Resaltar lo que cambió desde tu última visita.
9. Editar título y vencimiento, y **archivar** (y desarchivar) una tarea.

### NO-alcance

| Fuera | Qué hipótesis no ayuda a validar |
|---|---|
| Borrado definitivo de tareas | Un borrado por error hace desaparecer un estado del que otros dependían y no se puede deshacer; archivar cubre la misma necesidad (sacar ruido de la lista) sin perder nada. |
| Presencia / "conectado ahora" / actividad de la persona | El estado es de la tarea. Es vigilancia, rechazada a propósito, y no cambia ninguna decisión. |
| Notificaciones push, email o Slack | La hipótesis es resumen que espera. Un aviso sustituye una interrupción por otra. |
| Resumen diario por email | Duplica el punto 8 fuera de donde se trabaja; si el valor llega por email, la lista sobra. |
| Integraciones (Git/PRs, CI, calendario, Slack) | Derivar el estado es otro producto y ocultaría justo lo que hay que medir: si la gente lo teclea sola. |
| Importar tareas de otro gestor | Se valida con tareas nuevas; importar invita a convivir con el otro gestor, que es como muere la categoría. |
| Estado o marca de "bloqueada" | Los bloqueos quedan fuera; mezclarlos ensucia la métrica (qué ronda se canceló). |
| Comentarios, descripción larga, adjuntos, menciones | Llevan a conversar dentro de la tarea: es chat. Para saber quién está en qué basta título y estado. |
| Varios equipos/espacios, invitaciones | Con un único equipo de 6 se valida lo mismo; queda como supuesto (S10, S11). |
| Roles y permisos | Son pares; un permiso convierte "dos clics" en "pídeselo a quien puede". |
| Estados configurables o más de tres | Configurar es el "rollo de Jira"; tres distinguen libre, tomada y terminada. |
| Prioridades, etiquetas, subtareas, dependencias | Sirven para ordenar trabajo; la hipótesis es ver quién está en qué. |
| Sprints, estimaciones, épicas, backlog ordenable | Renuncia explícita: quien lo necesita no es nuestro usuario. |
| Informes, analítica, métricas en la app | No hay lead que los consuma; el éxito se mide preguntando al equipo. |
| Historial de cambios por tarea | Es reporting con otro nombre; el punto 8 ya responde "qué se movió". |
| Aviso de "tarea en curso sin movimiento" | Ataca el riesgo #1 presionando a la gente; la ficha elige abaratar la actualización. Si el estado se queda viejo, hay que verlo como resultado, no taparlo. |
| Filtro por persona / "mis tareas" | Con 3–10 personas la lista es corta, y el valor está en ver al equipo, no solo lo mío. |
| Búsqueda y ordenación configurable | Con decenas de tareas no cambia ninguna decisión. |
| Bloqueo de edición / edición simultánea | La colaboración simultánea está excluida; el choque se evita viendo al momento que la tarea está tomada. |
| App móvil nativa u offline | Una web usable en el móvil cubre los 3 husos horarios. |
| Recuperar contraseña, editar perfil, avatares | No cambian si el estado se mantiene fresco; con 6 personas se resuelve a mano. |

## Las tres líneas

1. **Los dos números:** la IA propuso 9 cosas dentro; después de mi recorte quedaron 9 (cambié "borrar tareas" por "archivarlas").
2. **Lo que dejé fuera, y por qué:** mi único recorte propio fue el **borrado definitivo de tareas**, reemplazado por archivar. No ayuda a validar que el estado se mantenga fresco con dos clics, y un borrado por error hace desaparecer un estado del que otros dependían sin forma de deshacerlo; archivar saca el ruido de la lista sin perder nada. El resto del NO-alcance lo acepté tal como lo propuso la IA: no recorté más, acepté su recorte.
3. **La exclusión de la que menos seguro estoy: el bloqueo de edición simultánea.** Se contradicen el dolor que origina el producto (dos personas trabajando en lo mismo sin saberlo) y la promesa de "dos clics, sin flujos": sin bloqueo, si dos personas toman la misma tarea en la misma ventana de segundos, gana el último cambio (S5) y una puede pisar a la otra sin enterarse, que es justo el episodio que queremos evitar; con bloqueo, tomar una tarea deja de ser un gesto instantáneo. Entraría si en la semana de uso real ocurre al menos una vez que dos personas toman la misma tarea casi a la vez y ninguna se da cuenta, es decir, si ver el cambio en segundos no alcanza.
