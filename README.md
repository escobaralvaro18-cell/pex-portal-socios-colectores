# Portal de Socios y Colectores · Prototipo UX

Prototipo navegable del rediseño del portal de socios/colectores de PuntoXpress, junto con el requerimiento formal (RQ) y las capturas de cada escenario. **Este repositorio es la fuente de verdad visual y de comportamiento para el desarrollo.**

## Cómo ver el prototipo

Abrir `prototipo/prototipo-portal-socios.html` en cualquier navegador (doble clic; no requiere servidor ni internet).

- **Login demo:** cualquier correo y contraseña; el código de verificación acepta cualquier 6 dígitos.
- **Cuentas demo:** el bloque de usuario (abajo del sidebar) cambia entre *Farmacias Económicas* (socio afiliado) y *Telecomunicaciones El Salvador* (colector). En la cuenta colectora, el mismo menú cambia el perfil: Administrador / Supervisor / Cobrador.
- **⚠️ Solo para el prototipo:** ese selector de cuenta/perfil es un atajo de demostración. En producción no existe: cada usuario tiene una única cuenta asignada y un único rol, y ambos vienen de la asignación del usuario en el sistema (no son seleccionables por el usuario final). Ver la nota en el RQ, sección 1.

## Prioridades de desarrollo

1. **Prioridad 1 — Paylink 1:1** (módulo Links de pago, funcionalidad nueva) + base de autenticación y estructura.
2. **Prioridad 2 — Rediseño** de Transacciones y del Dashboard de inicio.

## Documentación

- `docs/RQ-Portal-Socios-Colectores.docx` — requerimiento formal (formato RQ) con los comportamientos detallados: matriz de permisos por perfil, comportamiento del selector de fechas opción por opción, validaciones, flujo del drawer de links de pago, puntos de integración marcados como **[API]**.
- `docs/capturas/` — capturas numeradas de cada escenario (las mismas figuras del RQ).

## Stack destino

El portal actual está construido en **Next.js (App Router) + Tailwind CSS + Lucide**; el prototipo usa tokens equivalentes a Tailwind para que la traducción sea 1:1. Todo el CSS/JS está inline en el HTML del prototipo, organizado por secciones comentadas.

### Mapa prototipo → componentes sugeridos

| Sección del HTML | Componente Next.js sugerido |
|---|---|
| Tokens CSS (`:root`) | `tailwind.config` (colores, radios, sombras) |
| `#view-login`, `#card-otp` | `app/auth/login` (+ paso OTP) |
| `.sidebar` (riel + hover overlay) | `components/Sidebar` |
| Menú de cuenta/perfil (`#acct-pop`) | `components/AccountSwitcher` |
| Selector de rango (`.range-wrap`) | `components/DateRangePicker` (regla de historial 3 meses) |
| KPIs (`kpiHTML()`) | `components/KpiCard` |
| Gráfica (`renderChart()`) | `components/RevenueChart` |
| Combobox proveedor (`.cbx`) | `components/ProviderCombobox` (dependiente de Producto) |
| Tabla + detalle (`renderTx`, `#dt-ov`) | `components/TransactionsTable` + `TransactionDetailModal` |
| Drawer de links (`#nl-ov`) | `components/PaymentLinkDrawer` (stepper 3 pasos) |
| Config de cuentas (`ACCOUNTS`) | Modelo de datos: servicios, productos, proveedores y permisos por cuenta |

### Reglas de negocio clave (detalle completo en el RQ)

1. **Permisos por cuenta y perfil** gobiernan el menú completo; secciones vacías ocultan su título; redirección automática si la página actual deja de estar permitida.
2. **Historial de 3 meses:** rangos y comparativas limitados; últimos 3 meses no muestra chip de variación.
3. **Transacciones es un query explícito:** filtros + Buscar (con skeleton); cambiar el rango resetea la búsqueda; Descargar solo existe con resultados.
4. **Paylink 1:1 solo genera** (link + QR): monto editable o bloqueado según parámetro por colector y por servicio (vía IT), expiración opcional, eliminación posible. Server-side: código del link y QR.

## Contenido del repositorio

- `prototipo/prototipo-portal-socios.html` — el prototipo (fuente de verdad de componentes y comportamiento).
- `docs/` — RQ formal + capturas de cada escenario.
- `assets/` — logos (color, blanco, redondo).
- **Prototipo en vivo:** https://lucent-meerkat-25110a.netlify.app
