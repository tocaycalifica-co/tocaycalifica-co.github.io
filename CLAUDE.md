# Toca y Califica: redirecciones de las placas

Este repositorio es la página `https://tocaycalifica-co.github.io`. Cada placa (QR/NFC) apunta a `https://tocaycalifica-co.github.io/<CODIGO>` y `404.html` redirige según `codigos.js`.

- Placas: A001–A010 (madera, NFC+QR) y A011–A020 (económica, solo QR). `DEMO` → WhatsApp de ventas.
- Si un código está vacío (`""`), sale "placa sin activar" con botón de WhatsApp.

## Activar o cambiar una placa (lo que el usuario pedirá desde el celular)
El usuario manda un código (ej. A005) y un enlace de Google Maps del negocio. Hay que:

1. Sacar el Place ID del enlace. En el enlace hay `!1s0xAAAA:0xBBBB`. Si es un enlace corto (`maps.app.goo.gl`), seguir la redirección para obtener el enlace largo.
   ```python
   import struct, base64
   a, b = 0xAAAA, 0xBBBB
   inner = b'\x09' + struct.pack('<Q', a) + b'\x11' + struct.pack('<Q', b)
   print(base64.urlsafe_b64encode(b'\x0a' + bytes([len(inner)]) + inner).decode().rstrip('='))  # ChIJ...
   ```
2. El enlace de reseña directa (abre las 5 estrellas) es `https://search.google.com/local/writereview?placeid=<ChIJ...>`.
   Si el usuario manda un `g.page/r/.../review`, usarlo tal cual.
3. En `codigos.js` poner `"A005": "<enlace>", // Nombre del negocio`. Para desactivar, dejar `""`.
4. Hacer commit y push directo a `main` (GitHub Pages publica en 1 o 2 minutos). No crear ramas ni PR.
5. Responder en español, corto: qué placa quedó con qué negocio, y el enlace para programar el NFC: `https://tocaycalifica-co.github.io/<CODIGO>`.
   Si el usuario no dice el código de la placa, preguntárselo antes de cambiar nada.

Hay que mantener el formato de `codigos.js`, porque el usuario también lo edita a mano desde github.com.
