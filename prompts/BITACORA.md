# Bitacora de tecnicas avanzadas

Laboratorio 07: Tecnicas Avanzadas de Prompting.
Herramienta de IA usada: (escribe aqui cual usaste)

## Ejercicio 2: Zero-shot, one-shot y few-shot
| Tipo | Aciertos (de 5) | Formato de la respuesta | Todas con el mismo formato (Si/No) |
|------|-----------------|-------------------------|------------------------------------|
| Zero-shot | 5 | Tabla Comentario / Clasificacion (formato elegido por la IA) | No |
| One-shot | 5 | Lista numerada "Etiqueta - comentario" (no copio el ejemplo) | No |
| Few-shot | 5 | "texto" -> etiqueta, igual que los ejemplos | Si |
## Ejercicio 3: Chain of Thought
| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) |
|--------|--------------------|---------------------------|------------------|
| Directo | S/ 318.60 | No | Si |
| Paso a paso | S/ 318.60 | Si | Si |

Ver el razonamiento es util aunque la respuesta directa haya sido correcta, porque permite comprobar cada calculo con la calculadora y no solo creerle a la IA. Si hubiera un error, se vería exactamente en que paso ocurrio, algo imposible de saber con un numero suelto.

## Ejercicio 4: Role prompting
| Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas |
|---------|-------------------------------|-----------------------|----------------------|
| A. Sin rol | Sencillo, general | Si, codigo corto en Python y analogia de la caja | A cualquier persona que quiere una idea rapida |
| B. Rol docente | Muy sencillo, con comparaciones de la vida diaria | Si, Python, diagrama de la caja y ejemplo de precio x cantidad | A estudiantes que nunca han programado |
| C. Rol senior | Tecnico: tipo de dato, valor, primitivas y referencia | Si, codigo Java con int, String, double y boolean | A un companero de trabajo con conocimientos previos |

## Ejercicio 5: Descomposicion
**Pedido de una sola vez:** la IA entrego un programa completo y muy largo (clases Producto y SistemaInventario con menu de 7 opciones). Tomo decisiones por su cuenta y no pude revisar nada antes de recibir todo el codigo.

**Pedido por pasos (mismo chat):**
- Paso 1: entrego 5 requisitos (registrar, consultar, actualizar, entradas/salidas de stock y reportes).
- Paso 2: diseño 4 clases (Producto, Inventario, MovimientoStock, ReporteInventario) con sus atributos y tipos.
- Paso 3: escribio la clase Producto con constructor y get/set, coherente con el diseño (incluye categoria).
- Paso 4: propuso 3 mejoras: validar precio y stock, validar datos obligatorios y metodos aumentarStock/reducirStock.

**Comparacion:** el pedido por pasos fue mas ordenado y coherente: cada respuesta se baso en la anterior y pude revisar los requisitos y el diseño antes de pedir el codigo. El pedido de una sola vez fue mas rapido, pero largo y con supuestos que yo no habia decidido.

## Ejercicio 6: Prompt estructurado y autocritica
**Prompt basico:** entrego 10 casos genericos, pero no incluyo el bloqueo despues de 3 intentos porque no conocia esa regla.

**Prompt estructurado:** entrego 6 casos de prueba en tabla con las 4 columnas pedidas, incluyendo el bloqueo (CP-04, CP-05, CP-06).

**Autocritica:** la IA reconocio que faltaban casos limite y agrego CP-07 a CP-12 (campos vacios, correo sin @, contrasena con espacios, solo espacios).

| Que revisar | Cumple (Si / No) |
|-------------|------------------|
| Tiene las 4 columnas pedidas? | Si |
| Incluye el bloqueo despues de 3 intentos? | Si |
| Incluye casos con campos vacios? | Si |
| Indica que casos agrego en la autocritica? | Si |
| Hay algun caso repetido o que no tenga sentido? | Si: CP-12 se solapa con CP-07 y CP-11 tiene un resultado esperado ambiguo |

### Prompt estructurado y mensaje de autocritica

```text
<rol>Actua como analista de pruebas de software.</rol>
<contexto>Login web con correo y contrasena. La cuenta se bloquea despues de 3 intentos fallidos.</contexto>
<tarea>Piensa paso a paso que puede fallar y escribe 6 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato>

Revisa tu tabla: faltan casos limite como campos vacios, correo sin @ o contrasena con espacios? Agrega los que falten e indica cuales agregaste.
```
