# mini-armonic

Laboratorio sonoro-visual de proporciones armónicas. Convierte un controlador MIDI en un generador de campos armónicos sostenidos, donde en lugar de tocar notas aisladas manipulás relaciones de frecuencia simples (`2:3`, `3:4`, `φ`, etc.) y visualizás esas proporciones en tiempo real.

Basado en ideas de **Harmonic Information Theory** por Mariano Fernández Méndez / AlterMundi.

---

## Demo

Abrir en el navegador, conectar el controlador y tocar un pad:

<video src="./mini-armonic-demo.mp4" controls width="100%" poster="./screenshot-v3.png"></video>

También podés ver la captura estática:

![mini-armonic screenshot](./screenshot-v3.png)

---

## Qué hace

- Genera un campo armónico sostenido a partir de una frecuencia fundamental `f0`.
- Superpone armónicos con un spread configurable.
- Dibuja una figura de **Lissajous** con el ratio activo.
- Muestra un **espectro** con etiquetas `1:n` de los armónicos.
- Visualiza un **círculo armónico** con las proporciones distribuidas logarítmicamente.
- Permite programar **16 pasos** de ratios y recorrerlos con un arpegiador.
- Loguea todos los mensajes MIDI entrantes en un **MIDI inspector** integrado.

---

## Requisitos

- Navegador moderno con soporte para **Web MIDI** y **Web Audio**: Chrome, Edge, Brave, Opera.
- Un controlador MIDI (Minilab 3, teclado con faders, etc.).
- Python 3 para servir el archivo localmente (Web MIDI requiere `localhost` o HTTPS).

---

## Cómo usarlo

```bash
cd mini-armonic
python3 -m http.server 8081
```

Abrí en el navegador:

```text
http://localhost:8081/mini-armonic-v3.html
```

Hacé clic en **INICIAR**, conectá tu controlador y empezá a explorar.

> No funciona abriendo el archivo `.html` directamente porque Web MIDI está restringido a `localhost` o HTTPS.

---

## Mapeo MIDI

### Knobs del Minilab 3 (factory preset)

| Knob | CC  | Parámetro     | Rango       |
|------|-----|---------------|-------------|
| 1    | 86  | f0            | 50 - 400 Hz |
| 2    | 87  | armónicos     | 1 - 16      |
| 3    | 89  | spread        | 0.2 - 2.0   |
| 4    | 90  | ratio X       | 1 - 8       |
| 5    | 110 | ratio Y       | 1 - 8       |
| 6    | 111 | fase          | 0 - 2π      |
| 7    | 116 | volumen       | 0 - 1       |
| 8    | 117 | perturbación  | 0 - 1       |

También soporta el preset alternativo Analog Lab/User MIDI con CC 21-28.

### Faders físicos (modo learn)

La interfaz tiene 4 slots para faders físicos:

1. Hacé clic en **LEARN** del slot que querés asignar.
2. Mové el fader físico de tu controlador.
3. El slot captura el CC y queda mapeado.

Por defecto los slots controlan:

| Slot | Parámetro |
|------|-----------|
| 1    | f0        |
| 2    | volumen   |
| 3    | ratio X   |
| 4    | ratio Y   |

Si movés un control no asignado, el **MIDI inspector** muestra el CC exacto.

### Pads del Minilab 3 (notas 36-43)

| Pad | Nota | Preset      | Ratio    |
|-----|------|-------------|----------|
| 1   | 36   | drone       | 1:1      |
| 2   | 37   | 1:2         | 1:2      |
| 3   | 38   | 2:3         | 2:3      |
| 4   | 39   | 3:4         | 3:4      |
| 5   | 40   | 3:5         | 3:5      |
| 6   | 41   | 4:5         | 4:5      |
| 7   | 42   | φ           | 1:φ      |
| 8   | 43   | φ²          | 1:φ²     |

### Shift + Pads

Con **SHIFT** activo (botón de la UI, tecla `Space`, o pad 37) los pads pasan a funciones secundarias:

| Pad | Función secundaria           |
|-----|------------------------------|
| 1   | guardar ratio en paso actual |
| 2   | toggle SHIFT                 |
| 3   | arpegiador on/off            |
| 4   | secuenciador on/off          |
| 5   | grabar (reservado)           |
| 6   | reproducir (reservado)       |
| 7   | stop (reservado)             |
| 8   | limpiar secuencia            |

### Teclado del controlador

Las notas del teclado (C3-C6 aprox.) cambian la frecuencia fundamental `f0` según la nota tocada.

### Atajos del teclado de la PC

- `Space` — toggle SHIFT.
- `A` — toggle arpegiador.

---

## Interfaz

| Sección             | Descripción                                          |
|---------------------|------------------------------------------------------|
| Faders / Knobs      | Controles virtuales de los 8 parámetros principales. |
| Faders físicos      | 4 slots con modo learn para faders reales.           |
| Funciones           | SHIFT, ARP, SEQ, SAVE, CLEAR.                        |
| Canvas central      | Lissajous, espectro y círculo armónico.              |
| Campo activo        | Métricas en tiempo real.                             |
| MIDI inspector      | Log de mensajes MIDI entrantes.                      |
| Presets armónicos   | 8 pads de proporciones.                              |
| Programador 16 pasos| Secuenciador de ratios.                              |

---

## Visualización

- **Lissajous**: figura XY que dibuja la proporción activa `ratioX : ratioY`.
- **Espectro**: barras frecuenciales con etiquetas `1:n` de armónicos detectados.
- **Círculo armónico**: armónicos `1:n` distribuidos logarítmicamente alrededor de `f0`.

---

## Programador de 16 pasos

Cada paso guarda un ratio `X:Y`. Cuando **ARP** y **SEQ** están activos, el campo avanza automáticamente por los 16 pasos. Hacé clic en un paso para guardar el ratio actual ahí, o usá **SHIFT + Pad 1**.

---

## Archivos

| Archivo                | Descripción                                      |
|------------------------|--------------------------------------------------|
| `mini-armonic-v3.html` | Versión actual.                                  |
| `mini-armonic-v2.html` | Rediseño futurista previo.                       |
| `mini-armonic-v1.html` | Primera versión funcional.                       |
| `screenshot-v3.png`    | Captura de la interfaz v3.                       |
| `README.md`            | Este archivo.                                    |

---

## Stack técnico

- HTML5 + CSS3 (grid, flexbox, glassmorphism).
- Vanilla JavaScript.
- Web Audio API para síntesis.
- Web MIDI API para controladores.
- Canvas 2D para visualización.

Sin dependencias externas. Servidor local solo para cumplir con las restricciones de Web MIDI.

---

## Créditos

- Concepto: [Mariano Fernández Méndez](https://hit.altermundi.net/) / AlterMundi.
- Implementación: JereC4str0 + Hermes Agent.

---

## Roadmap

- [ ] Loop/grabación de movimientos de knobs.
- [ ] Exportar campo armónico a WAV.
- [ ] Presets guardables en `localStorage`.
- [ ] Integración con DAW vía MIDI o WebSocket.
- [ ] Modo osciloscopio XY a pantalla completa.
