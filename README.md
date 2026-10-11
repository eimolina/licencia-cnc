# Licencia Virtual CNC

Página que muestra y permite descargar la licencia del juego
*Aprendo las Señales de Tránsito - CNC* cuando se escanea el código QR.

No usa servidor ni base de datos: la licencia se dibuja en el navegador
con los datos que vienen en el enlace.

## Formato del enlace

```
https://USUARIO.github.io/licencia-cnc/?n=NOMBRE&i=NUMERO&d=AAAAMMDD
```

| Parámetro | Qué es | Ejemplo |
|-----------|--------|---------|
| `n` | Nombre del estudiante (máx. 40 caracteres) | `Mar%C3%ADa+Jos%C3%A9` |
| `i` | Número de licencia, corto o completo | `12` o `0501-000012-CNC` |
| `d` | Fecha de emisión | `20261011` |

Si falta algún dato, la página muestra un aviso en lugar de la licencia.

## Publicar en GitHub Pages

1. Crear un repositorio público llamado `licencia-cnc`.
2. Subir `index.html` y `escudo.png` a la raíz.
3. Ir a **Settings → Pages**, en *Source* elegir **Deploy from a branch**,
   rama `main`, carpeta `/ (root)`, y guardar.
4. En uno o dos minutos la página queda en
   `https://USUARIO.github.io/licencia-cnc/`.

## Cambio en el juego

En `drawLicense()`, reemplazar la línea del QR por:

```javascript
const BASE = 'https://USUARIO.github.io/licencia-cnc/';
const num  = String(parseInt(licNum.split('-')[1], 10));          // "0501-000012-CNC" -> "12"
const fec  = `${d.getFullYear()}${String(d.getMonth()+1).padStart(2,'0')}${String(d.getDate()).padStart(2,'0')}`;
const nom  = encodeURIComponent(typed).replace(/%20/g, '+');
const url  = `${BASE}?n=${nom}&i=${num}&d=${fec}`;
let qr = url;
try { makeQR(qr); } catch (e) { qr = `CNC|Licencia ${licNum}|Colegio Nicolas Copernico`; }
drawQR(c, W-190, H-34-18-150, 150, qr);
```

El generador de QR del juego admite unos 106 caracteres. Cada letra con
tilde o ñ ocupa 6 caracteres en el enlace, así que con nombres muy largos
el `try/catch` vuelve al QR de texto para que el juego no falle.
Probar con el nombre más largo de la lista antes de usarlo en clase.

## Privacidad

El nombre viaja solo dentro del enlace; no se guarda en ningún lado.
La página tiene `noindex` para que los buscadores no la muestren.
