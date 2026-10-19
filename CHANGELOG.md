# Changelog

Todas las modificaciones notables de este proyecto estan documentadas bajo el estándar de SemVer.

## [1.0.0] - 2026-10-19
### Añadido
- Implementación de la arquitectura completa y modular de n8n para el motor de *SnackCheck* (según diseño visual validado).
- Configuración de Webhook POST inicial con validación de presencia de códigos de barras y rama de error específica (`Codigo no existe o esta vacio`).
- Integración HTTP con la API de Open Food Facts y bifurcación de control para productos no encontrados (`Producto no encontrado`).
- Implementación de procesamiento modular secuencial mediante múltiples nodos `Set` (*Separar umbrales*, *Calcular umbrales*, *Veredicto Semáforo y Nutri-Score* y *Veredicto final*).
- Integración de validación previa de datos nutricionales con manejo de errores para registros faltantes (`Datos faltantes`).
- Incorporación del nodo de Inteligencia Artificial (`Message a model`) con enrutamiento de tono adaptativo.
- Documentación técnica completa (`README.md`), control de cambios (`CHANGELOG.md`) y registro estructurado de pruebas.