# ArqDeSoftware

# Sistema Asistente Virtual UCB

Este sistema representa un **asistente virtual inteligente** diseñado para la Universidad Católica Boliviana. Sus principales funciones son:

- Recibe preguntas o comandos del usuario.
- Usa un modelo de IA para generar una respuesta.
- Ejecuta comandos concretos (como consultar horario o notas).
- Consulta una base de datos de estudiantes mediante una API.
- Usa observadores para reaccionar cuando se recibe una respuesta.

---

## Componentes y su Función

| Clase | Descripción |
|-------|-------------|
| **ControladorMain** | Punto de entrada y coordinación del asistente. Es un singleton. Gestiona el flujo de entrada y salida. |
| **GestorComandos** | Clase que gestiona una lista de comandos y puede ejecutarlos todos. Aplica el patrón Command. |
| **ComandoConsultarHorario** / **ComandoConsultarNotas** | Clases concretas de comando que encapsulan una acción específica (consultar horario o notas). |
| **ServicioDatosEstudiante** | Accede a los datos reales del estudiante (por API). Es usado por los comandos. |
| **EstrategiaIA** | Interfaz que define el método `generarRespuesta()`. |
| **EstrategiaOllama** / **EstrategiaGPT** | Implementaciones de IA que usan diferentes proveedores de modelo. |
| **ObservadorRespuestas** | Observa y reacciona cuando se recibe una nueva respuesta de la IA. Aplica el patrón Observer. |

---

## Patrones de Diseño Utilizados

### 1. Singleton – `ControladorMain`
- Solo puede haber una instancia global del controlador principal.
- Se usa para coordinar todas las demás clases.
- El método `obtenerInstancia()` garantiza una única instancia.

---

### 2. Command – `GestorComandos`, `ComandoConsultarHorario`, `ComandoConsultarNotas`
- Encapsula acciones en objetos.
- Permite agregar nuevos comandos fácilmente sin modificar el sistema central.
- Ideal para ejecutar comandos como "Consultar horario", "Consultar notas", etc.

---

### 3. Strategy – `EstrategiaIA`, `EstrategiaOllama`, `EstrategiaGPT`
- Permite cambiar dinámicamente el motor de IA usado para generar respuestas.
- `ControladorMain` o alguna parte del sistema podría decidir cuál usar según configuración.

---

### 4. Observer – `ObservadorRespuestas`
- Se activa cuando se genera una nueva respuesta desde la IA.
- Podría actualizar la UI, registrar logs o realizar otra acción automática.

---

## Flujo General del Asistente

1. El usuario escribe una pregunta o comando.
2. `ControladorMain` recibe la entrada.
3. Se elige y ejecuta una estrategia IA (`EstrategiaOllama` o `EstrategiaGPT`) para generar una respuesta.
4. La respuesta se pasa a los observadores.
5. Si la entrada corresponde a un comando (por ejemplo, "mostrar mi horario"):
   - `GestorComandos` ejecuta los comandos apropiados.
   - `ComandoConsultarHorario` llama a `ServicioDatosEstudiante` para obtener el horario.
   - Los datos se devuelven al controlador y se presentan al usuario.
