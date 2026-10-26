# Borradores de issues

GitHub Issues está desactivado para este repositorio. Estos borradores corresponden al plan en [ROADMAP.md](ROADMAP.md) y están listos para publicar cuando se habilite.

## MVP 1

### Auditar seguridad de Firebase y preparar un deploy seguro

**Objetivo:** verificar y cerrar los accesos actuales antes de incorporar usuarios o datos reales.

**Alcance:** revisar reglas activas de Firestore, configuración del cliente, historial público y posibles datos o credenciales sensibles; definir permisos mínimos, respaldo y despliegue HTTPS. Considerar App Check como protección adicional.

**Aceptación:**
- La auditoría registra el estado inicial y riesgos concretos.
- Las reglas deniegan por defecto y conceden solo operaciones necesarias.
- Se prueban accesos permitidos y denegados antes de publicar.
- El procedimiento de despliegue y recuperación queda documentado.

**Bloqueo:** se necesita acceso al proyecto Firebase para verificar la configuración de consola.

### Diseñar y migrar gradualmente el modelo de Firestore

**Objetivo:** mantener Firestore NoSQL y dejar de sincronizar todo el negocio como un único documento `marjavpro/principal`.

**Alcance:** proponer documentos y colecciones para productos, clientes, compras, ventas, consignaciones y gastos, con límites claros por negocio y usuario. Mantener PDFs y remitos como evidencia vinculada a registros normalizados.

**Aceptación:** esquema documentado con reglas y consultas previstas; migración idempotente y respaldable; conteos e importes verificados; historial preservado y mecanismo de recuperación definido. No migrar a SQL sin límites medidos que justifiquen el cambio.

### Agregar OCR al flujo de carga de listas y documentos de compra

**Objetivo:** reconocer texto de PDFs escaneados cuando PDF.js no encuentre texto seleccionable.

**Alcance:** mantener extracción actual para PDFs con texto; aplicar OCR como alternativa; asociar documento de origen; requerir revisión humana antes de guardar.

**Aceptación:** detección de PDFs sin texto útil y progreso por página; corrección de productos, cantidades y precios antes de confirmar; ninguna modificación de costos antes de la confirmación explícita; verificación con PDFs digitales, escaneados y casos de error.

### Implementar conciliación mensual de gastos

**Objetivo:** revisar gastos del período y conciliarlos con comprobantes.

**Alcance:** registrar fecha, categoría, monto, medio de pago, proveedor y comprobante; marcar pendientes o conciliados y cerrar períodos. Separar conciliación de caja de la distribución de costos a productos.

**Aceptación:** consultar un mes, registrar y editar gastos; ver totales por categoría y medio y diferencias pendientes; corregir períodos cerrados sin borrar historial; documentar la política de cierre.

### Agregar bandas de precio y alertas de margen

**Objetivo:** guiar decisiones de precios sin modificarlos automáticamente.

**Alcance:** configurar piso aceptable (incluida pérdida planificada), objetivo y techo opcional por producto o lista; comparar precio vigente y escenarios ante cambios de costo.

**Aceptación:** mostrar margen estimado y estado de banda; importaciones de costos presentan impacto para revisión; precios solo cambian con confirmación manual; documentar costos incluidos y excluidos, como gastos operativos.

### Crear estadísticas de compras por producto y cliente

**Objetivo:** consultar cantidades e importes comprados por período, producto y cliente.

**Alcance:** unificar ventas y consignaciones con semántica explícita para venta, entrega en consignación, venta reportada y pago de deuda.

**Aceptación:** filtros por período, cliente, producto y modalidad; totales con acceso al documento fuente; los pagos de deuda no se cuentan como una segunda compra; documentar métricas y límites del histórico.

## MVP 2

### Implementar autenticación y roles admin, vendedor y cliente

**Objetivo:** agregar acceso multiusuario sobre Firebase Authentication.

**Alcance:** asociar cada cuenta a negocio y rol; aplicar permisos en Firestore Rules, sin depender de ocultar controles en la interfaz.

**Aceptación:** flujos de acceso y cierre definidos; permisos de cada rol documentados y aplicados; pruebas de aislamiento entre negocios y clientes; transición segura de los datos actuales.

### Implementar carrito para clientes y administración

**Objetivo:** crear pedidos desde el catálogo para clientes autenticados y administración.

**Alcance:** asociar carrito y pedido a usuario y negocio; guardar precio y condiciones al confirmar; gestionar estados del pedido.

**Aceptación:** agregar, editar y quitar productos; administración puede asistir según permisos; pedido conserva precio, cantidades y fecha; cambios posteriores de listas no alteran pedidos existentes; reglas impiden acceso cruzado.

### Registrar logs de auditoría para acciones de negocio

**Objetivo:** saber quién cambió datos importantes y cuándo.

**Alcance:** registrar actor, acción, entidad, fecha y cambios relevantes para precios, compras, gastos, pedidos y permisos; separar auditoría de errores técnicos.

**Aceptación:** eventos no editables desde el cliente; sin contraseñas, tokens ni datos innecesarios; búsqueda autorizada por período, actor y entidad; correcciones y anulaciones conservan historial.

### Agregar observabilidad y captura de errores tipo Sentry

**Objetivo:** detectar errores del navegador y problemas de rendimiento en producción.

**Alcance:** capturar excepciones, versión desplegada, flujo y contexto mínimo; definir alertas y retención.

**Aceptación:** errores de prueba visibles en el panel; datos personales, tokens y documentos se filtran; configuración de privacidad documentada; guía de respuesta a incidentes disponible.
