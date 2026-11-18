# Teatro

Un motor para crear videojuegos de texto interactivo, inspirado en Twine, diseñado para ejecutarse en la consola.

- **Autor original:** [@lukeitorld](https://github.com/lukeitorld) — idea, arquitectura de escenas y motor original (v0.1–v0.5, escrito en Latino clásico).
- **v0.6:** migrado a **Latino.NET v1.0.81+** con el patrón *Factory* y estado encapsulado.

---

## ⚠️ Compatibilidad: Latino.NET v1.0.81+

A partir de la **v0.6**, Teatro es un módulo de **Latino.NET** (el sucesor moderno sobre .NET), no del Latino clásico de C. El lenguaje cambió la forma de cargar módulos, así que **la inicialización es distinta**:

```latino
incluir("teatro")          # incluir() devuelve nulo: inyecta la fabrica en el entorno global
teatro = CrearTeatro()     # la fabrica devuelve la instancia con su estado privado
```

> En Latino clásico se escribía `teatro = incluir("teatro")`. En v1.0.81 eso deja `teatro` en `nulo`, porque `incluir()` ya no retorna el valor del módulo; y un `retornar {...}` a nivel de archivo es un error global. Por eso el motor ahora se construye con una *fábrica*.

### Configuración

```latino
teatro = CrearTeatro()                 # ancho de consola por defecto: 100
teatro = CrearTeatro({"ancho": 60})    # ancho personalizado
```

### Interfaz pública

```latino
teatro.estado.configuracion.ancho      # configuración vigente
teatro.estado.historia                 # escenas visitadas (clones independientes)
teatro.cambiar_escena(escena)          # carga y renderiza una escena
teatro.procesar_entrada("1")           # valida y despacha la opción elegida
teatro.procesar_opcion(">comando()<")  # ejecuta el comando de una opción
teatro.armar_cuerpo(texto)             # ajusta el texto al ancho, por palabras
teatro.centrar_encabezado(título)      # centra el título entre bordes "="
```

---

## ¿Qué es Teatro?

**Teatro** es un módulo que funciona como motor de ficción interactiva o juegos conversacionales.  
Proporciona métodos y una estructura base para construir aventuras de texto en la consola, donde cada historia se desarrolla a través de **escenas** que incluyen un título, una descripción y un conjunto de opciones que el jugador puede elegir.

El nombre *Teatro* refleja su enfoque narrativo: las historias se representan como obras teatrales, en las que cada escena abre un nuevo acto y las decisiones del jugador marcan el rumbo de la obra.

## Estructura de una escena

Una escena es un **diccionario de 4 elementos**:

| Clave | Tipo | Uso |
|---|---|---|
| `nombre` | cadena | Título de la escena, centrado entre bordes `=` |
| `descripcion` | cadena | Cuerpo; se ajusta al ancho configurado |
| `id` | decimal | Identificador corto de la escena |
| `opciones` | lista de cadenas | Las decisiones del jugador |

Cada opción es `Texto visible >comando<`. El comando (entre `>` y `<`) se ejecuta al elegir la opción; `>vacio<` la oculta.

```latino
menu = {
    "nombre": "Menú Principal",
    "descripcion": "Bienvenido.",
    "id": 0,
    "opciones": ["Saber más >teatro.cambiar_escena(manual)<", "Oculta >vacio<", "Salir >teatro.cambiar_escena(salida)<"]
}
```

## Ejemplos

```bash
latino ejemplos/ejemploMinimo.lat
latino ejemplos/ejemploDosEscenas.lat
```

> **Nota de ruta:** `incluir()` resuelve las rutas relativas al **directorio del script de entrada** (el archivo que se pasa a `latino`), no al directorio de trabajo. Como los ejemplos viven en `ejemplos/`, incluyen el motor con `incluir("../teatro")`.

## Roadmap

- **V 0.50** — Versión final de Teatro para consola (Latino clásico). ✅
- **V 0.60** — Migración a Latino.NET v1.0.81: patrón *Factory*, `clonar()` en el historial, ajuste de texto por palabras y validación exacta de entradas. ✅
- **V 0.51~1.00** — Versión de Teatro con ventana gráfica usando GTK.

---

*Licencia MIT. Créditos del diseño original: @lukeitorld.*
