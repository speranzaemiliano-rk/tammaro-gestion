# Tammaro · Gestión

Sistema de gestión para una reventa en Mercado Libre: productos con precio sugerido, ventas con ganancia real, compras a proveedores y resumen mensual.

Un solo archivo (`index.html`), sin instalar nada. Datos en Firebase Realtime Database. Se publica en GitHub Pages.

## Puesta en marcha (una sola vez)

### 1. Crear el proyecto en Firebase
1. Entrá a https://console.firebase.google.com y creá un proyecto nuevo (por ejemplo `tammaro-gestion`). Google Analytics no hace falta.
2. **Authentication → Comenzar → Correo electrónico/contraseña → Habilitar.**
3. **Authentication → Settings → User actions:** desactivá «Enable create (sign-up)». Así nadie puede crearse una cuenta por su cuenta: solo entran las que vos creás.
4. **Realtime Database → Crear base de datos** (elegí la región más cercana, modo bloqueado).

### 2. Pegar la configuración en `index.html`
Firebase → ⚙️ Configuración del proyecto → «Tus apps» → agregar app **Web** (`</>`). Te muestra un bloque `firebaseConfig`.
Copiá sus valores dentro de `FIREBASE_CONFIG` al principio del `<script>` de `index.html`.
(Esta clave es pública por diseño: lo que protege los datos son las reglas del paso 3.)

### 3. Publicar las reglas de seguridad
Realtime Database → pestaña **Reglas** → pegá el contenido de `database.rules.json` → **Publicar**.
⚠️ Sin este paso la base queda cerrada para todos. No se aplica sola: hay que pegarlo a mano.

### 4. Crear tu usuario administrador
1. Authentication → Users → **Add user** (tu correo y una contraseña).
2. Copiá el **User UID** de ese usuario.
3. Realtime Database → Datos → pasá el mouse sobre la raíz → «+» → nombre `roles`, y adentro un hijo con **nombre = tu UID** y **valor = `admin`** (texto).
   Esto se hace desde la consola porque las reglas no dejan a nadie darse rol a sí mismo.

### 5. Publicar el sitio
1. Creá un repo en GitHub (por ejemplo `tammaro-gestion`) y subí `index.html`, `database.rules.json` y este README.
2. Settings → Pages → Branch `main`, carpeta `/ (root)` → Save.
3. En unos minutos queda en `https://TU-USUARIO.github.io/tammaro-gestion/`.
4. En Firebase → Authentication → Settings → **Authorized domains**, agregá `TU-USUARIO.github.io`.

## Identidad visual y presentaciones
- El sistema usa el logo (`logo.png`) y sus colores: naranja `#E84820` y gris `#A0A0A0`. Títulos en tipografía script (Yellowtail) y rótulos en Montserrat itálica, que acompañan al logo.
- **Presentación del mes:** en *Inicio*, botón «▶ Presentación del mes». Pantalla completa con flechas del teclado, deslizando en el celular, `F` para pantalla completa y `Esc` para salir. Se puede cambiar de mes desde la barra de abajo.
- **Al entrar al sistema:** aparece sola una bienvenida con el logo en 3D, sigue con el resumen del último mes con ventas y termina con el botón «Entrar al sistema». Se puede saltar con ✕. Si no la querés, tildá «No mostrar al iniciar» o desactivala en Configuración → «Al iniciar» (se guarda en cada dispositivo).
- **Logo en 3D:** `presentacion.html` (abrir `…/tammaro-gestion/presentacion.html`). Se mueve solo y se gira arrastrando con el mouse o el dedo.

## Sumar gente
1. Firebase → Authentication → Users → **Add user** (correo + contraseña, o que use «Olvidé mi contraseña»).
2. La persona entra una vez; verá «Cuenta pendiente».
3. Vos, en el sistema → **Usuarios**, le elegís el rol:
   - **Lector:** solo mira.
   - **Editor:** carga y modifica productos, ventas y compras.
   - **Admin:** además borra, cambia la configuración y gestiona usuarios.

## Excel: subir y exportar
- **📥 Subir Excel** (arriba, en todas las pantallas; solo editores y admin). Lee las hojas *Productos*, *Ventas* y *Proveedores* del Tablero. Por defecto **suma solo lo nuevo**: si subís dos veces el mismo archivo no se duplica nada. Opcionalmente actualiza los costos y precios de productos que ya existen. «Reemplazar todo» borra lo cargado y deja lo del Excel.
- **📤 Exportar Excel** (todos los roles). Baja un `.xlsx` con *Productos*, *Ventas*, *Proveedores* y *Resumen mensual*, en el mismo formato que acepta «Subir», así que sirve también de copia de seguridad.

## Calendario e informe en PDF
- **Calendario:** vista mensual con ventas por día (facturación, cantidad y ganancia), compras y el **inicio de mes**: cómo cerró el mes anterior y cuánta ganancia falta para cubrir los costos fijos. Tocá un día para ver su detalle.
- **📄 Informe PDF:** en *Inicio* y en *Calendario*. Genera un PDF con el logo: resumen, productos que más ganancia dejaron, detalle de ventas y de compras. Elegís un mes o «Todos los meses».

## Cómo se calcula
- **Facturación de una venta** = precio − comisión ML − impuestos + bonificaciones − envío.
- **Ganancia de una venta** = facturación − (costo del producto + packaging) × cantidad.
  (El costo se guarda en la venta, así que si después cambiás el costo del producto el historial no se mueve.)
- **Precio sugerido** = (costo total + costo fijo ML) ÷ (1 − comisión − impuestos − publicidad − margen objetivo).
- **Ganancia neta mensual** = ganancia bruta − contadora − monotributo.

## Datos en Firebase
`tammaro/config`, `tammaro/productos`, `tammaro/ventas`, `tammaro/compras`, más `roles/<uid>` y `usuarios/<uid>`.
