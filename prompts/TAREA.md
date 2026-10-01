# Tarea: Mi prompt avanzado

Laboratorio 07: Tecnicas Avanzadas de Prompting.
Herramienta de IA usada: ChatGPT (plan gratuito)

## Tarea elegida

Generar **casos de prueba para un formulario de registro de usuarios** de una aplicacion web. El formulario tiene nombre, correo, contrasena y confirmacion de contrasena. La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero, y el correo no puede estar repetido.

Es una tarea util en desarrollo de software porque unos buenos casos de prueba detectan errores antes de que lleguen al usuario, y porque la calidad del resultado depende mucho de como se escribe el prompt.

## Version 1: prompt basico

```text
Dame casos de prueba para un registro de usuarios.
```

**Tecnica agregada:** ninguna (prompt basico, solo una instruccion directa).

**Por que:** es el punto de partida para comparar. Sirve para ver que entrega la IA cuando no recibe contexto ni formato.

**Que observe en la respuesta:** la IA entrego una lista general de casos (registro correcto, correo invalido, campos vacios, contrasena corta). Los casos son genericos: no consideran las reglas de la contrasena ni el correo repetido, y el formato no es fijo, por lo que no se puede copiar directamente a una hoja de calculo.

## Version 2

```text
<rol>Actua como analista de pruebas de software senior de aplicaciones web.</rol>
<contexto>Formulario de registro con nombre, correo, contrasena y confirmacion de contrasena. La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero. El correo no puede repetirse.</contexto>
<tarea>Escribe 8 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato>
```

**Tecnicas agregadas:** role prompting (analista de pruebas senior de aplicaciones web) y prompt estructurado (etiquetas rol, contexto, tarea y formato).

**Por que:** la v1 era generica porque la IA no conocia las reglas del formulario ni el formato que yo necesitaba. El rol especifico orienta el enfoque hacia pruebas de software, y las etiquetas separan claramente cada parte del pedido.

**Que mejoro:** los casos ya usan las reglas del contexto (contrasena sin mayuscula, sin numero, de menos de 8 caracteres, correo repetido, confirmacion que no coincide). La respuesta llego en una tabla con las 4 columnas pedidas y exactamente 8 casos. Lo que todavia falta: no se ve el razonamiento, no hay garantia de que cubra casos limite, y el estilo de cada fila puede variar.

## Version 3: prompt final

```text
<rol>Actua como analista de pruebas de software senior de aplicaciones web.</rol>
<contexto>Formulario de registro con nombre, correo, contrasena y confirmacion de contrasena. La contrasena debe tener minimo 8 caracteres, una mayuscula y un numero. El correo no puede repetirse.</contexto>
<tarea>
1. Piensa paso a paso que puede fallar en cada campo.
2. Escribe 8 casos de prueba que cubran casos validos, invalidos y limite.
3. Revisa tu tabla, indica si falta algun caso limite y agregalo marcandolo como "(agregado)".
</tarea>
<ejemplo>
CP-01 | Registro correcto | Ana, ana@mail.com, Clave123, Clave123 | Cuenta creada y mensaje de exito
</ejemplo>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado. Responde en espanol.</formato>
```

**Tecnicas agregadas:** chain of thought (pensar paso a paso que puede fallar), few-shot / ejemplo (una fila modelo para fijar el formato), descomposicion de la tarea en tres pasos numerados y autocritica (revisar la tabla y agregar lo que falte).

**Por que:** en la v2 faltaba ver el razonamiento y asegurar que se cubrieran los casos limite. Chain of thought obliga a analizar cada campo antes de escribir los casos. El ejemplo fija como deben escribirse los datos de entrada y los resultados. La autocritica agrega los casos que la primera version pudo olvidar.

**Que mejoro:** la IA primero explica que puede fallar en cada campo, luego entrega la tabla con el mismo formato del ejemplo, y al final marca los casos agregados con "(agregado)". Los casos cubren validos, invalidos y limite (por ejemplo, contrasena de exactamente 8 caracteres), y la respuesta es mas facil de revisar.

## Tecnicas usadas en el prompt final

| Parte del prompt final | Tecnica |
|------------------------|---------|
| `<rol>Actua como analista de pruebas de software senior de aplicaciones web.</rol>` | Role prompting |
| Uso de etiquetas `<rol>`, `<contexto>`, `<tarea>`, `<ejemplo>` y `<formato>` | Prompt estructurado |
| "Piensa paso a paso que puede fallar en cada campo." | Chain of thought |
| Lista numerada de tareas (1. pensar, 2. escribir, 3. revisar) | Descomposicion |
| `<ejemplo>` con la fila CP-01 | Few-shot (un ejemplo del formato) |
| "Revisa tu tabla, indica si falta algun caso limite y agregalo..." | Autocritica |

## Evaluacion del resultado

| Criterio | Cumple (Si / No) |
|----------|------------------|
| La tabla tiene las 4 columnas pedidas (ID, escenario, datos de entrada, resultado esperado) | Si |
| Tiene los 8 casos solicitados | Si |
| Incluye casos que usan las reglas de la contrasena y el correo repetido | Si |
| Incluye casos limite (por ejemplo, contrasena de exactamente 8 caracteres) | Si |
| La IA muestra su razonamiento antes de la tabla | Si |
| Indica que casos agrego en la autocritica con "(agregado)" | Si |
| Hay casos repetidos o que no tienen sentido | No |
| Responde en espanol y con el formato del ejemplo | Si |

## Por que elegi estas tecnicas

Elegi role prompting y prompt estructurado porque el pedido tiene varias partes (rol, contexto con reglas, tarea y formato) y separarlas con etiquetas evita que la IA las mezcle. Agregue chain of thought porque en pruebas de software importa pensar primero que puede fallar en cada campo, y asi puedo revisar el razonamiento. Use un ejemplo (few-shot) porque necesito que todas las filas tengan el mismo formato para copiarlas a una hoja de calculo, y con un solo ejemplo bastaba para fijarlo. Incluyo la autocritica porque la primera respuesta suele olvidar casos limite. No use few-shot con muchos ejemplos porque la tarea no es una clasificacion repetitiva y un ejemplo era suficiente. Tampoco hice una descomposicion en varios chats porque la tarea es pequena y cabe en un solo pedido bien ordenado.