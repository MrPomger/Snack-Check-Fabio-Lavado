# Changelog

Todas las modificaciones notables de este proyecto estan documentadas bajo el estándar de SemVer.

## [1.0.1] - 2026-10-19
### Corregido
- **Estandarización del parámetro de entrada:** Corrección en el nodo de consulta a Open Food Facts para utilizar consistentemente `body.barcode` en lugar de `body.code`.
- **Extracción de datos nutricionales (`Set - Separar`):** Se corrigió la sintaxis JSON (coma faltante) y se reemplazó `added-sugars_100g` por `sugars_100g`, reflejando con precisión el contenido real de azúcar (ej. 56.3g en Nutella).
- **Lógica de cascada en Veredicto Final:** Implementación de la jerarquía correcta de evaluación: "no saludable" si cualquier indicador lo es ➔ "moderado" si alguno es moderado ➔ "saludable" solo si 2 o más lo aprueban.
- **Tratamiento de Nutri-Score ausente (`unknown`):** Si el valor falta o cae fuera del rango A–E, el veredicto de Nutri-Score se asigna como `unknown` y la decisión final depende únicamente del semáforo (evitando que cuente falsamente como "saludable").
- **Coerción de tipos en `worry score`:** Conversión explícita de string a number para la comparación estricta (`=== 1`), permitiendo la correcta activación del estado "moderado".

## [1.0.0] - 2026-10-19
### Añadido
- Implementación de la arquitectura completa y modular de n8n para el motor de *SnackCheck* (según diseño visual validado).
- Configuración de Webhook POST inicial con validación de presencia de códigos de barras y rama de error específica (`Codigo no existe o esta vacio`).
- Integración HTTP con la API de Open Food Facts y bifurcación de control para productos no encontrados (`Producto no encontrado`).
- Implementación de procesamiento modular secuencial mediante múltiples nodos `Set` (*Separar umbrales*, *Calcular umbrales*, *Veredicto Semáforo y Nutri-Score* y *Veredicto final*).
- Integración de validación previa de datos nutricionales con manejo de errores para registros faltantes (`Datos faltantes`).
- Incorporación del nodo de Inteligencia Artificial (`Message a model`) con enrutamiento de tono adaptativo.
- Documentación técnica completa (`README.md`), control de cambios (`CHANGELOG.md`) y registro estructurado de pruebas.