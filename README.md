# Círculos CQB

Juego de combate cuerpo a cuerpo en un mapa cerrado (CQB), visto desde arriba, con cono de visión.
Todo está en **un solo archivo** (`index.html`): no necesita instalación ni servidor propio.

- **Local:** 2 jugadores en el mismo dispositivo (pantalla dividida), con teclado o con controles táctiles.
- **En línea:** de 2 a 4 jugadores (lo decide quien crea la sala), cada uno en su propio dispositivo y a pantalla completa. Se entra con un **código de 5 caracteres** generado al azar. Todos contra todos: gana el último con vidas.

## Cómo subirlo a GitHub Pages

1. Crea un repositorio nuevo en GitHub (por ejemplo `circulos-cqb`).
2. Sube `index.html` y este `README.md` (**Add file → Upload files**).
3. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`. Guarda.
4. En un minuto aparecerá la dirección `https://TU_USUARIO.github.io/circulos-cqb/`. Esa es la página del juego.

> Usa siempre la dirección `https://` de GitHub Pages. El modo en línea y la pantalla completa no funcionan abriendo el archivo directamente desde el disco.

## Cómo jugar en línea

1. Abre el juego → **Jugar en línea**. Escribe tu nombre.
2. **Quien organiza:** elige cuántos jugadores como máximo (2, 3 o 4) y pulsa **Crear sala**. Aparece el **código**; pulsa *Compartir enlace* o pásalo a tus amigos.
3. **Los demás:** abren la misma página, **Jugar en línea → Unirse**, escriben el código (o abren el enlace compartido, que ya trae el código).
4. En la sala cada quien escoge su personaje. El anfitrión pulsa **Iniciar partida** cuando quiera (mínimo 2 jugadores).
5. Al terminar, el anfitrión puede dar **Jugar otra vez**.

Cada jugador ve su propia pantalla completa con su cono de visión, su vida y un marcador de todos arriba a la derecha. Quien es eliminado pasa a espectar.

## Cómo funciona la conexión

- La conexión es **directa entre dispositivos (WebRTC)** usando la librería [PeerJS](https://peerjs.com/) (se carga desde un CDN). El servidor gratuito de PeerJS solo sirve para que los dispositivos se encuentren con el código; no guarda nada de la partida.
- **El dispositivo del anfitrión ejecuta el juego** y manda el estado a los demás unas 20 veces por segundo. Los demás envían sus controles. Por eso:
  - El anfitrión debe **mantener la pestaña abierta y en primer plano**; si se cierra, la sala termina.
  - Conviene que el anfitrión tenga buena conexión y un dispositivo no muy lento.
- En redes muy restrictivas (algunos datos móviles, redes de empresa o escuela) la conexión directa puede fallar. En ese caso hace falta un servidor TURN: añádelo en el bloque `NET.PEER_OPTIONS.config.iceServers` al inicio del script de `index.html` (hay un ejemplo comentado). También puedes usar tu propio servidor PeerJS (ver el comentario en el mismo bloque).
- No hay protección contra trampas: cada cliente recibe el estado completo de la partida. Está pensado para jugar con amigos.

## Controles

**Local, Jugador 1:** mover `W A S D` · disparar `E` · recargar `Q` · granada `R` · cable `F` (punto A, camina, `F` otra vez = punto B) · torreta `G` · pistola/escopeta `T` · botiquín guardado `X` · mina `C`

**Local, Jugador 2:** mover flechas · disparar `-` · recargar `.` · granada `,` · cable `M` · torreta `N` · pistola/escopeta `B` · botiquín guardado `V` · mina `K`

**En línea (teclado):** mover `W A S D` o flechas · disparar `E`, `-` o `Espacio` · el resto de teclas como arriba (cualquiera de las dos columnas).

**Celular:** joystick flotante a la izquierda y botones a la derecha (en local, cada jugador usa su mitad de la pantalla). Botones ⛶ pantalla completa y ☰ menú.

Otras teclas en local: `0` reinicia, `Esc` vuelve a elegir personaje.

## Reglas rápidas



- 3 vidas de 100 de salud. Bala a 5 m/s (1 casilla = 1 m).
- Daño: disparos 20 · torreta 10 · granada 99 · cable 60 · mina 200 · sin balas, tocar a alguien le quita 1 de vida cada 0.1 s.
- Pistola y escopeta (5 perdigones en cono, medio ritmo, gasta 5 balas por disparo). 3 cargadores de 10 balas, recarga de 3 s.
- Granadas (2 por jugador) y objetos recogibles que solo ves dentro de tu cono: botiquín (+1 vida o se guarda y cura en 5 s), chaleco (−30 % de daño), cable, torreta y mina. Los globos morado (ametralladora) y verde (pistola de rebote) se ven siempre.

## Personalizar

Todo está en `index.html`: constantes de daño, tiempos y velocidades al inicio del script; la configuración de red en `NET`. Los sprites (Rem y Hisoka) están incrustados en base64; para cambiarlos reemplaza las cadenas `data:image/png;base64,...` en el arreglo `CHARS`.
