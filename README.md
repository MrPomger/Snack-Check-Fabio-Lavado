# SnackCheck Automation Engine - Capstone Project

## Propósito
Backend automatizado sin servidor diseñado para evaluar perfiles nutricionales en tiempo real a partir del código de barras de un producto empaquetado. Conecta la base de datos abierta de Open Food Facts con la síntesis de lenguaje natural mediante Inteligencia Artificial (Groq LLM) para ofrecer veredictos amigables, claros y empáticos a los usuarios de la aplicación móvil de SnackCheck.

## Función
El flujo está estructurado de manera modular y secuencial, garantizando robustez y facilidad de depuración:
1. **Entrada y Validación:** El nodo `Webhook - Recibir Datos del Código de Barras` capta las solicitudes POST. El nodo `IF - Validar Presencia del Código de Barras valido` evalúa si el código existe; si está vacío, deriva al nodo de error `Webhook - Error: Codigo no existe o esta vacio`.
2. **Capa de Datos:** El nodo `API Call - Open Food Facts` realiza la consulta externa.Para evitar que el flujo colapse de forma abrupta si la API externa devuelve un error 404 (producto inexistente), se configuró este nodo con la opción **"Never Error"**. Esto permite que la operación nunca detenga el workflow ante fallos de búsqueda, permitiendo redirigir el flujo de manera controlada hacia el nodo de error `Webhook - Error: Producto no encontrado`. El nodo `IF - validar presencia del producto` verifica entonces si la API encontró el artículo exitosamente
3. **Procesamiento y Lógica Modular:** 
   - `Set - Separar umbrales nutricionales`: Aisla y estructura las variables crudas de la API.
   - `IF - Verificar datos`: Comprueba que existan registros nutricionales utilizables (si faltan, deriva a `Webhook - Error: Datos faltantes`).
   - `Set - Calcular Umbrales Nutricionales`: Evalúa los semáforos de azúcar, sal y grasa por cada 100g y calcula la puntuación de preocupación.
   - `Set - Veredicto Semaforo y Nutri_Score`: Traduce las lecturas independientes.
   - `Set - Veredicto final`: Aplica la jerarquía para seleccionar el peor escenario entre el semáforo y el Nutri-Score.
4. **Capa de Inteligencia (IA):** El nodo `Message a model` procesa los datos estructurados y redacta un párrafo con tono dinámico (alentador o cauteloso con delicadeza).
5. **Capa de Salida:** Los nodos `Set - Mensaje final` y `Webhook - Mensaje final que devuelve` formatean el resultado multisección con emojis y marcadores visuales.

## Configuración
1. Importa el archivo JSON del flujo en tu espacio de trabajo de n8n.
3. Envía peticiones POST al endpoint del webhook en Hoppscotch utilizando un cuerpo JSON:
  
   {
     "code": "3017620422003"
   }

## Manejo de Errores y Excepciones
- **Código ausente o vacío:** Atajado por la primera bifurcación del nodo `IF` inicial, derivando la solicitud hacia un webhook que responde con una estructura autodocumentada.
- **Producto no encontrado (Manejo de API 404):** Para evitar que el flujo colapse cuando la API externa devuelve un error 404 (producto inexistente), se configuró el nodo `API Call - Open Food Facts` con la opción **"Never Error". Esto permite que la operación no detenga el workflow de forma abrupta, permitiendo que el flujo capture el estado y lo dirija con gracia hacia el webhook de error `Producto no encontrado`.
- **Datos nutricionales faltantes:** Si el artículo existe en la base de datos pero carece de registros de nutrientes utilizables, el nodo `IF - Verificar datos` interrumpe la clasificación y emite una advertencia limpia de datos insuficientes.