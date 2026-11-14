# Manual de Usuario: Emulador de Videoterminal CID 702 (CM 7203)

## 1. Introducción

Bienvenido al emulador interactivo de la **CID 702**, la primera videoterminal alfanumérica desarrollada y producida en serie en Cuba (1978), conocida internacionalmente dentro del sistema CAME como **SM-7203 / CM 7203**.

Este emulador reproduce fielmente las capacidades del hardware original: pantalla de **72 caracteres x 20 líneas** (1440 celdas), teclado con sub-etiquetas cirílicas según el estándar **KOI-7Н**, y comunicación con una computadora host a través de tres interfaces históricas simuladas.

---

Especificaciones Técnicas del Hardware Original

- **Dimensiones de pantalla:** 22 × 16,5 cm; 20 líneas × 72 caracteres de 5×7 píxeles.
- **Generador de caracteres:** 64 ASCII + 160 latinos/cirílicos + 64 gráficos.
- **Atributos:** parpadeo, inversión de video, 7 colores (en esta emulación: fósforo verde P1).
- **Memoria:** buffer interno de 1440 caracteres.
- **Codificación:** ASCII / KOI-7Н.
- **Interfaces:** IRPR (paralelo), S2 (serie V.24), IRPS (lazo de corriente).
- **Alimentación original:** 110/220 V, 60/50 Hz, 200 W (no aplicable al emulador).
- **Peso:** 22 kg sin teclado / 25 kg con teclado.

---

## 2. Guía de Inicio Rápido

1. **Encendido:** pulsa el botón circular en el extremo derecho del panel superior. Se iluminará en **rojo** y aparecerá el **banner de bienvenida** con datos técnicos y recomendaciones.
2. **Conexión con el Host:** la terminal intenta conectarse automáticamente al encenderse. El LED rojo de `ENLACE` confirma la conexión.
3. **Selección de Interfaz:** en el panel host (lateral derecho), usa el desplegable `INTERFAZ` para elegir `IRPR`, `S2` o `IRPS`.
4. **Escritura:** haz clic en las teclas virtuales o usa tu teclado físico real.
5. **Ayuda:** pulsa el botón con icono de **libro** en la barra de estado inferior para consultar este manual dentro del emulador.

---

## 3. Conociendo el Hardware

### 3.1. El Monitor CRT

- **Pantalla:** 20 líneas × 72 caracteres = 1440 celdas.
- **Cursor:** bloque verde parpadeante que indica la posición de escritura.
- **Señales sonoras:** la terminal emite un "beep" corto al llegar al borde de pantalla (fila 20) o al final de línea (columna 72), tal como el hardware original.

### 3.2. El Teclado

**Teclas alfanuméricas (oscuras):** letra latina principal en blanco, letra cirílica correspondiente en ámbar. La correspondencia es **fonética**: `A→А`, `B→Б`, `C→Ц`, `F→Ф`, `J→Й`, etc.

**Teclas de función (claras):** barra superior de funciones y barra inferior de edición/control.

**Comportamiento unificado:** todas las teclas virtuales reaccionan visualmente al pulsar la tecla equivalente física, incluida la **barra espaciadora**.

### 3.3. Panel de Control Superior

| Control | Comportamiento |
|---|---|
| **ENLACE** (LED) | Rojo si hay conexión establecida y la terminal está encendida |
| **SOLICITUD INF? / SUP** | Interruptores del protocolo handshake (ver §6) |
| **MODO** | Palanca **arriba = LOCAL**, **abajo = LÍNEA** |
| **ROLL** | Arriba = scroll automático; abajo = la pantalla se sobrescribe |
| **CENT CURS** | Centra el cursor (estado visual) |
| **MOV CURS (CONT/PASO)** | Arriba = las flechas repiten al mantener; abajo = solo primer press |
| **CM 7203** | Rótulo del modelo |
| **POWER** | Botón circular de encendido, rojo cuando está encendido |

### 3.4. Panel Host (Lateral Derecho)

- **CONECTAR / DESCONECTAR:** establece el enlace. Sin conexión no hay comunicación.
- **Selector de interfaz:** cambia entre `IRPR`, `S2` e `IRPS`.
- **Log de comunicación:**
  - `→ TX [interfaz]:` datos enviados **desde el host hacia la terminal**.
  - `← RX:` datos recibidos **desde la terminal hacia el host**.
- **Campo de envío:** envía el texto más CR+LF al terminal.
- **Botones rápidos:**
  - `CLEAR`: limpia pantalla y log (disponible incluso con la terminal apagada).
  - `HOME`, `EOL`, `LF`, `CR+LF`, `TAB`, `BELL`, `DEMO`: requieren terminal encendida.

---

## 4. Guía de Funciones del Teclado

### 4.1. Modificadores

| Tecla | Clic del mouse | Tecla física |
|---|---|---|
| **SHIFT** | Toggle persistente (se queda hundida) | Momentáneo (se consume al usarse) |
| **SHIFT LOCK** | Toggle persistente | Se sincroniza con `CapsLock` físico |
| **CTRL** | Toggle persistente | Momentáneo (se consume al aplicarse a una letra) |
| **KOI?** | Alterna entre latín y cirílico | — |

**Notas importantes sobre modificadores:**

- **`Ctrl + letra`** genera el carácter de control correspondiente (Ctrl+C → ETX, Ctrl+A → SOH, Ctrl+D → EOT, etc.).
- Con **texto seleccionado** (en el log o en la ayuda) y `Ctrl+C`, se respeta el atajo del navegador en vez de enviar control a la terminal.
- El `Shift` **virtual** también afecta al tecleo desde el teclado físico (`e.shiftKey || T.shift`).

### 4.2. Teclas de Función

| Tecla | Función |
|---|---|
| **KOI?** | Alterna entre entrada latina y cirílica. La letra cirílica corresponde fonéticamente a la latina. |
| **INS REM / SUP REM** | Inserción / supresión de líneas (reservado para futuras versiones). |
| **BREAK?** | Envía ETX (ASCII 0x03). Fuerza **flush inmediato** del buffer de TX. |
| **INIC CAR / SUP CAR** | Inicio / supresión de carácter (reservado). |
| **RESET** | Limpia toda la pantalla. |
| **FEED / BACK FEED** | Avanza / retrocede una línea (reservado para teletipo). |
| **START / STOP** | Registra en el log del host; marca inicio/fin de operación. |
| **STEP CLEAR** | Borra la línea actual. |
| **NEW LINE** | Envía CR+LF al host (en modo LÍNEA) y salta de línea en pantalla. **Flush inmediato.** |
| **BACK?** | Retroceso: borra el carácter anterior y envía 0x08 al host en modo LÍNEA. |
| **DEL** | Borra bajo el cursor (desplazando el resto). Envía 0x7F en modo LÍNEA. |
| **ESC** | Envía escape (0x1B). |
| **???** (tecla blanca) | Reservada, no emite nada. |

---

## 5. Interfaces de Comunicación

La terminal emula tres interfaces históricas con velocidades simuladas distintas:

| Interfaz | Uso histórico | Velocidad simulada |
|---|---|---|
| **IRPR** | Paralela, conexión directa de corta distancia | Instantánea (~100.000 car/s) |
| **S2** | Serie V.24 / RS-232, líneas de datos | ~9.600 baudios (10 ms por carácter) |
| **IRPS** | Lazo de corriente, entornos industriales ruidosos | ~9.600 baudios (15 ms por carácter) |

**Comportamiento:**

- Al cambiar la interfaz con la terminal conectada, el enlace se reinicia brevemente para aplicar la nueva velocidad.
- Los datos enviados desde el host **no generan eco** en el buffer de transmisión (evita el bucle `→ TX` → `← RX` que confundía el log).
- Los caracteres aparecen en pantalla con retardo visible en `S2` e `IRPS`; instantáneo en `IRPR`.

---

## 6. Handshake y SOLICITUD

Los interruptores **SUP** e **INF?** representan el protocolo de handshake entre terminal y computadora:

- **SUP (Request To Send):** terminal lista para transmitir al host.
- **INF? (Flow Control / XOFF):** solicita pausa del flujo desde el host (buffer próximo a llenarse o terminal ocupada).

En el hardware original la computadora respondía con **CTS** o **XON**. En esta simulación los interruptores mantienen estado visual; el handshake real está simplificado para no bloquear la interfaz.

---

## 7. Modo LOCAL y Modo LÍNEA

### LOCAL (palanca arriba)

Terminal autónoma. Los caracteres se muestran en pantalla pero **no** se transmiten al host.

### LÍNEA (palanca abajo)

Modo semidúplex. Cada carácter tecleado se muestra en pantalla y se acumula en el buffer de transmisión. El buffer se envía al host en tres casos:

1. Al pulsar **NEW LINE** → envía `CR+LF` y fuerza **flush inmediato**.
2. Al pulsar **BREAK?** → envía `ETX` y fuerza **flush inmediato**.
3. Por **debounce de 400 ms** de inactividad → agrupa caracteres sueltos para evitar spam en el log.

Al volver de LÍNEA a LOCAL, cualquier buffer pendiente se envía antes de cambiar de modo.

---

## 8. Barra de Estado

Muestra el estado actual en cinco campos:

| Campo | Valores |
|---|---|
| **ESTADO** | APAGADO / ENCENDIDO |
| **ENLACE** | DESCONECTADO / CONECTADO |
| **MODO** | LOCAL / LINE |
| **INTERFAZ** | IRPR / S2 / IRPS |
| **CURSOR** | fila,columna (00,00 a 19,71) |

En el extremo derecho, dos botones con icono:

- **Monitor:** muestra u oculta el Panel Host.
- **Libro:** muestra u oculta este Panel de Ayuda.

---

## 9. Atajos y Comportamientos Especiales

- **Shift físico:** modificador momentáneo. Ambas teclas SHIFT virtuales se hunden visualmente al pulsarlo.
- **Control físico:** modificador momentáneo (como Shift). Combinado con una letra genera el carácter de control correspondiente.
- **Ctrl + letra con texto seleccionado:** se respeta el atajo del navegador (copiar, buscar, etc.).
- **Backspace / Delete:** funcionan con el teclado físico y virtual.
- **Flechas, Home, End:** mueven el cursor. Las flechas respetan el modo CONT / PASO del switch MOV CURS.
- **Sonidos:**
  - Click agudo para teclas (1400 Hz).
  - **Clack grave diferenciado** para interruptores (sweep 2200→900 Hz).
- **Barra espaciadora:** reacciona visualmente al pulsar Space en el teclado físico, igual que el resto de teclas.
- **Transiciones visuales:** los interruptores tienen transición de 0.04 s (sin lag perceptible).

