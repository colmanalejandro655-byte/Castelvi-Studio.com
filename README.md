# Castelvi Studio · Barbería interactiva

Página pública con reservas en vivo, galería, tienda y panel privado del barbero, con análisis financiero completo.

## Cómo verla

La forma recomendada es levantar un servidor local (así funciona la sincronización entre pestañas sin problemas):

```powershell
py -m http.server 8080
```

Después abrí:

- **Web pública:** http://127.0.0.1:8080/index.html
- **Panel del barbero:** http://127.0.0.1:8080/admin.html

> También podés abrir los `.html` con doble clic, pero algunos navegadores limitan `localStorage`/`BroadcastChannel` en `file://`.

**PIN del panel:** `2468` (cambiable en *Personalizar → Reservas y seguridad*).

---

## Qué se puede hacer desde el panel

| Sección | Qué hace |
|---|---|
| **Resumen** | Cupos libres, turnos en espera, atendidos hoy, ingresos del día, gráfico semanal, meta mensual y próximos turnos. |
| **Turnos** | Ordenados por llegada. Pasar a "En silla", marcar **✓ Ya se cortó**, cancelar, eliminar y avisar por WhatsApp. Al completar se suma el dinero y se descuentan los costos. |
| **Galería** | Subir fotos desde cámara/galería, **recortarlas** (zoom, arrastre y rotación), editar título/estilo/barbero, destacar y eliminar. |
| **Productos** | Agregar/editar productos con **precio** y **foto recortada**, activar/desactivar stock y eliminar. |
| **Finanzas** | Ingresos, gastos, utilidad neta, margen, ticket promedio, series diarias, ranking de servicios y productos, diagnóstico automático, costos por servicio, meta mensual, **arqueo de caja con billetes** y exportación CSV. |
| **Personalizar** | Nombre, textos, color, **imagen de portada recortable**, **horarios por día**, **precios de servicios**, contacto, redes, cupos por día, PIN y visibilidad de secciones. |
| **Datos** | Descargar/importar respaldo JSON, resetear y ver estado del sistema. |

Todo lo que cambiás se refleja **en la web al instante** si tenés ambas pestañas abiertas (sincronización en vivo).

---

## Datos ya cargados

- Barbería: **Castelvi Studio** · Exequiel Castelvi
- WhatsApp: `+595 992 271 978`
- Instagram: `https://www.instagram.com/castelvi.studio/`
- Google Maps: `https://maps.app.goo.gl/tAW9xdn6WXLUAufm9`
- Tarifas iniciales: Corte ₲ 120.000 · Barba ₲ 80.000 · Completo ₲ 180.000
- Cupos iniciales: 15 por día
- Tienda: SucVaetto Pomade ₲ 120.000 · Fox Seco Toque ₲ 90.000 · BEST+ Powder ₲ 85.000 · Oficina Hair ₲ 75.000 · Fox Pó Modelador ₲ 70.000

Las fotos que enviaste están en `assets/`: `hero-castelvi.webp` (portada), `corte-1/2/3.webp` (galería), `producto-*.jpg` (fotos reales de la tienda) y los billetes se usan en el **arqueo de caja**. Podés reemplazar cualquiera desde el panel, con recorte incluido.

---

## Estructura

```
.
├── index.html          # Vista pública
├── admin.html          # Panel privado (PIN)
├── css/
│   ├── styles.css      # Diseño oscuro/dorado, componentes públicos
│   └── admin.css       # Panel de administración
├── js/
│   ├── config.js       # Datos, utilidades, cuentas y sincronización
│   ├── app.js          # Reservas, carrusel, tienda, semáforo
│   └── admin.js        # Operación, personalización y finanzas
└── assets/             # Tus fotos, billetes y placeholders SVG
```

---

## Persistencia y migración a la nube

Ahora los datos se guardan en `localStorage` y se sincronizan entre pestañas del mismo navegador con `BroadcastChannel`. Es ideal para arrancar y probar.

Para que funcione entre **distintos dispositivos** (clientes reservando desde sus celulares), migrá a Supabase o Firebase:

1. Creá las colecciones `configuracion_dia`, `turnos`, `galeria_cortes`, `productos`, `gastos` y `arqueos`.
2. En `js/config.js`, dentro de `update()`, reemplazá `persist(draft)` por el `upsert` de tu backend.
3. En `subscribe()`, agregá el canal realtime (Supabase) o `onSnapshot()` (Firestore) y llamá `notify()` al recibir cambios.
4. Las fotos recortadas se guardan como WebP en `dataURL`; con Supabase Storage podés subir el Blob y guardar la URL pública en el campo `imagen`.

Todo el resto del código ya usa `Store.update()`, `Store.subscribe()` y `Store.getState()`, así que no hay que reescribir la lógica de las vistas.

---

## Publicación

1. Subí la carpeta a un repositorio de GitHub.
2. Conectá el repo a **Vercel** o **Netlify** (URL gratuita con HTTPS).
3. Si comprás un dominio, configuralo en Vercel y generá un **QR** con la dirección para imprimirlo en espejos y puerta.
4. Antes de publicar, **cambiá el PIN** y revisá que el link de Maps sea el de la ficha real del negocio.

> Recordatorio: al tratarse de almacenamiento local, cada navegador tiene su propia copia. Usá *Datos → Descargar respaldo JSON* periódicamente.
