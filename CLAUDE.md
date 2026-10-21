# CLAUDE.md — Seguimiento de Partidas (seguimiento-partidas)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
Sigue las partidas de tela del reporte semanal de PCP. Cada registro es partida + tela + color + servicio, con prioridad (URGENTE / 1 este mes / 2 próximo mes / 3 después), grupo, kg, estilo, ruta y fecha hito, y tres seguimientos: **Aprobación** (la marca Calidad: Pendiente / Aprobación Interna / Aprobación Cliente), **Llegada** (la marca Producción Textil: Pendiente / Por confirmar / Confirmada) y **Comentario**. Cada fila está Activa o Cerrada.
- `seguimiento.html` — **pantalla principal de PCP** (con usuario del portal): KPIs (partidas, urgentes, kg, pendientes de aprobación y de llegada), filtros, comentarios y **carga del reporte semanal** (Excel de PCP; primero muestra una vista previa de nuevas, actualizadas, cerradas y omitidas; las que no vienen pasan a Cerrado y se conservan aprobación, llegada y comentario). Exporta Excel propio y una tarjeta PNG ("Resumen visual") para el cliente.
- `actualizar.html` — **marcar aprobación y llegada**, por link directo (fuera del portal): la persona escribe su nombre arriba y ese nombre queda como quién actualizó. Solo muestra partidas activas.
- `observadores.html` y `observadores-portal.html` — **vista de solo lectura** de las partidas con llegada confirmada (activas o cerradas). Son la misma pantalla: la primera sin usuario (es la que abre el botón "👁️ Vista observadores"), la segunda con usuario del portal.

## Base de datos (Supabase lives-datos)
- `seguimiento.html`: `sp_listar`, `sp_comentario`, `sp_ultima_carga`, `sp_cargar_reporte` (con `p_simular` para la vista previa). Tablas `sp_partidas`, `sp_cambios`, `sp_cargas`.
- `actualizar.html`: `sp_libre_listar`, `sp_libre_aprobacion`, `sp_libre_llegada` (sobre `sp_partidas`, historial en `sp_cambios`).
- Observadores: `sp_libre_observadores` (sin usuario) y `sp_observadores` (con usuario).
- No usa Storage. No usa la puerta `segp_api` (esa es de otro sistema).

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 3 personas (analistas de Planeamiento y supervisora de Top Sample), más Calidad y Producción Textil por el link directo de `actualizar.html`.

## Quién puede registrar
- Carga del reporte: lo decide la base (`sp_listar` devuelve `puede.seguimiento`; sin permiso no aparece el botón).
- Aprobación y llegada: por link directo, con el nombre que escribe la persona.
- A Sistemas: aquí va su control de roles (Calidad marca aprobación, Producción Textil marca llegada, PCP carga el reporte).

## Pendientes vistos al escribir esto (21/10/2026)
- Comentario heredado en `observadores-portal.html` que dice "pantalla LIBRE por link (sin portal)"; ya no corresponde.
