# Plan de producto

## Decisiones

- Mantener Firebase Authentication y Cloud Firestore (NoSQL); la autenticación no es motivo para migrar a SQL.
- Evolucionar el modelo actual antes de sumar roles: hoy el cliente sincroniza el estado del negocio en un único documento `marjavpro/principal`.
- Tratar PDFs de proveedores y remitos como documentos de origen. El OCR propone datos; una persona los revisa y confirma antes de registrarlos.
- No cambiar precios automáticamente. Mostrar bandas y escenarios para que la persona decida.

## MVP 1: operación segura y análisis

1. **Seguridad y despliegue:** auditar reglas Firestore activas, acceso actual, historial público y configuración; definir autenticación/permisos mínimos, respaldo y despliegue HTTPS antes de usar datos reales con nuevos usuarios.
2. **Modelo de datos Firestore:** separar progresivamente el documento global en documentos/colecciones con límites de acceso claros. Preservar importes, fechas e historial mediante migración verificable y mecanismo de recuperación.
3. **Compras documentadas y OCR:** detectar si un PDF ya contiene texto; si no, convertir páginas a imágenes y aplicar OCR. Revisar líneas, productos y precios antes de confirmar. Conservar procedencia (proveedor, fecha y documento) ligada a la compra registrada.
4. **Gastos mensuales:** categorizar gastos, registrar fecha/medio y estado de conciliación, comparar movimientos contra comprobantes y cerrar períodos revisados. Definir por separado cualquier distribución de gastos entre productos.
5. **Bandas de precio:** permitir por producto/lista un piso de margen o pérdida aceptable, objetivo y techo opcional. Mostrar el efecto de nuevos costos e identificar precios fuera de banda; no reescribir precios en forma automática.
6. **Estadísticas:** consultar cantidades e importes por producto y cliente, por período y modalidad (venta o consignación), evitando duplicar pagos de deuda como nuevas compras.

## MVP 2: acceso multiusuario y operación

1. Autenticación y autorización por roles: admin, vendedor y cliente; limitar consultas y escrituras a los datos permitidos para cada rol.
2. Carrito para clientes autenticados y para administración, con pedido, precios aplicados y estado trazables.
3. Registro de acciones relevantes (actor, acción, entidad y fecha) separado de los errores técnicos.
4. Observabilidad de fallos y rendimiento, con captura de excepciones tipo Sentry y contexto mínimo, sin secretos ni datos sensibles innecesarios.

## Secuencia y criterios

Empezar con auditoría de seguridad y diseño del modelo Firestore; luego completar las funciones del MVP 1. Iniciar el MVP 2 cuando reglas y modelo permitan aislar datos por usuario/negocio. Cada entrega debe definir casos de aceptación, migración/retrocompatibilidad y verificación manual en navegador. Reconsiderar SQL solo si consultas, relaciones o volumen demuestran límites concretos que la nueva estructura Firestore no resuelve.

## Issues

- Seguridad y deploy: revisar reglas, acceso, historial y publicación segura.
- Modelo Firestore: reemplazar gradualmente el documento global y planificar migración.
- OCR y compras documentadas: extraer, revisar y guardar procedencia.
- Conciliación mensual de gastos.
- Bandas de precio y alertas de margen.
- Estadísticas de compras por producto/cliente.
- Roles de usuario y permisos.
- Carrito compartido entre cliente y administración.
- Logs de auditoría.
- Observabilidad y captura de errores tipo Sentry.
