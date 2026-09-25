# Distribución de la colección

## Estado actual

Disponible: seis carpetas autocontenidas en `skills/`, plantilla de autoría, catálogo y [guía de uso](USAGE.md). La distribución actual consiste en copiar carpetas revisadas o cargar una copia local desde Crush, como explica el [README](../README.md).

Lo siguiente es una propuesta de arquitectura, no un instalador ni un pipeline ya implementado. No hay gestor de actualizaciones, lockfile de instalación, bundles de release o CI de empaquetado en esta colección. Crear esta documentación no publica una versión ni crea tags.

## Recomendación

Usar **GitHub como origen versionado y copias locales por proyecto fijadas a un commit**. Después, añadir bundles de GitHub Releases y un instalador pequeño que verifique integridad, muestre el diff y requiera aprobación para actualizar.

La unidad de instalación sigue siendo cada carpeta de skill completa. Para empezar, una versión de la colección es más sencilla que seis ciclos independientes: una release puede distribuir un bundle completo y permitir seleccionar qué skills instalar. Si en el futuro hay consumidores y cadencias realmente diferentes, se puede evaluar versionado individual sin cambiar el formato de las carpetas.

El proyecto consumidor debe poder conservar las skills instaladas bajo control de versiones para que un cambio de instrucciones sea revisable como cualquier cambio de código. El destino predeterminado propuesto es `.agents/skills/` para agentes que lo soporten; otros destinos se seleccionan explícitamente. No se debe asumir soporte universal por usar `SKILL.md`.

## Comparación de opciones

| Opción | Ventajas | Límites y recomendación |
| --- | --- | --- |
| Copia por proyecto fijada a SHA | Revisión local, reproducibilidad y cambios visibles en PR | Requiere gestionar actualizaciones; recomendada para equipos |
| Copia global de una revisión fija | Menos duplicación y útil para uso personal | Afecta varios proyectos; debe actualizarse explícitamente |
| Ruta global a un checkout mutable | Configuración sencilla en Crush | Cualquier cambio local altera varios consumidores; no usar como distribución estable |
| Submódulo Git | Revisión fijada por Git | Añade pasos de checkout y gestión, y expone más estructura de la necesaria; no lo elegiría como valor por defecto |
| Gestor externo o marketplace | Puede facilitar descubrimiento e instalación | Exige auditar resolución de versiones, scripts, actualizaciones y permisos; adaptador opcional futuro, no raíz de confianza |

No necesitamos npm, un registry ni ejecutar código descargado para repartir archivos Markdown. Un comando cómodo de instalación puede añadirse sin convertir una dependencia remota mutable en autoridad sobre las instrucciones.

## Contrato propuesto del bundle

Publicar desde un SHA aprobado, con una lista explícita de contenido:

- Carpetas completas de las skills incluidas y sus referencias/recursos.
- Licencia y avisos de atribución aplicables. La distribución debe conservar el texto de MIT aunque solo instale una skill; no basta un enlace al origen.
- Manifiesto con versión de colección, SHA fuente, lista de skills y hashes de cada archivo distribuido.
- Checksum del archivo descargable y notas de cambios, incluyendo cambios de permisos, activación y comportamiento.

Excluir `.git/`, `.crush/`, credenciales, configuración personal, caches y plantillas de autoría de las rutas de carga. No incorporar instaladores o hooks del repositorio consumidor al bundle. Rechazar symlinks, rutas absolutas y entradas que escapen mediante `..` al extraer.

Una etiqueta legible ayuda a elegir una versión, pero puede moverse si no se protege: registrar también SHA y hashes. Un checksum publicado junto al bundle detecta corrupción, pero no autentica por sí solo a su autor frente a una cuenta comprometida. Evaluar firmas/atestaciones y releases inmutables como una capa adicional, con verificación y claves de confianza definidas antes de afirmar esa garantía.

## Contrato propuesto del instalador

### Inspección antes de escritura

1. Resolver solo el origen y la versión autorizados; no usar `latest` silenciosamente ni sustituir una versión no encontrada.
2. Descargar datos en un directorio temporal privado, sin ejecutar scripts ni cargar las skills aún.
3. Verificar revisión, manifiesto, integridad, nombres y confinamiento de rutas antes de extraer o copiar.
4. Mostrar skills, destino, archivos nuevos/modificados/eliminados y dependencias de herramientas. Un modo de previsualización no escribe en el proyecto ni ejecuta instrucciones.
5. Solicitar aprobación de la instalación o actualización concreta. No modificar configuración global, proveedores, MCPs, hooks ni permisos.

### Instalación y registro

Instalar únicamente carpetas seleccionadas, conservando licencia y recursos. Mantener fuera de las rutas de descubrimiento un registro de instalación con origen, versión, SHA, hashes instalados y destino por skill. El nombre y formato de ese lockfile se decidirán al implementar; no existe todavía.

No instalar un segundo archivo `SKILL.md` como respaldo dentro de una ruta que el agente escanee. Las copias de recuperación deben estar fuera del descubrimiento de skills y protegidas frente a uso accidental.

### Actualización y recuperación

Comparar los hashes instalados con el registro anterior. Si hay personalizaciones o archivos desconocidos, detenerse y mostrar el conflicto; no sobrescribir, mezclar automáticamente ni borrar contenido ajeno. La actualización debe ser explícita, sin scheduler ni comprobaciones automáticas que cambien instrucciones durante una sesión.

Preparar y verificar los nuevos archivos antes de activarlos. Diseñar un reemplazo seguro por plataforma y conservar un registro recuperable si falla a mitad; no prometer atomicidad de múltiples carpetas donde no exista. Repetir la instalación de la misma revisión debe ser un no-op verificable.

Para desinstalar o volver a una revisión anterior, aplicar los mismos controles: eliminar únicamente archivos reconocidos y sin modificaciones, conservar lo ajeno y pedir autorización para el cambio de instrucciones. No usar borrados recursivos indiscriminados.

## Plan para montar la distribución

### 1. Validación y empaquetado local

Implementar un validador reproducible de metadatos, nombres, enlaces confinados, permisos, contenido permitido y preservación de licencias. Añadir pruebas de archivos maliciosos, symlinks, colisiones y carpetas independientes. Generar bundle/manifiesto/checksums deterministas desde una revisión limpia y aprobada, sin publicar.

### 2. Instalación local revisable

Implementar selección de skills, destino explícito, previsualización, registro de instalación, conflictos y recuperación. Probar instalación repetida, actualización con personalizaciones, interrupción y desinstalación en directorios temporales; ningún test debe tocar configuraciones personales.

### 3. Publicación controlada

Añadir CI con permisos mínimos que valide y prepare artefactos del SHA exacto. Separar build de publicación y requerir autorización para tag/release. Fijar revisiones de acciones/herramientas; revisar lo que ejecuta CI antes de entregarle credenciales. Preparar una primera release solo después de verificar los artefactos y el flujo de instalación.

### 4. Adaptadores y descubrimiento

Tras validar el flujo básico, documentar compatibilidad real por agente y ofrecer adaptadores de destino o integración con catálogos. El origen versionado y el registro local seguirán siendo la base; un marketplace no debe añadir actualización automática ni instalación de dependencias por sorpresa.

## Criterios de aceptación de la futura distribución

- La misma revisión produce los mismos archivos instalados y un inventario verificable.
- El consumidor puede instalar una sola skill sin dependencias entre carpetas.
- Ninguna descarga ejecuta código ni autoriza herramientas del agente.
- Una actualización muestra diferencias y no pierde cambios locales.
- No se extraen archivos fuera del destino ni se cargan respaldos o plantillas como skills reales.
- Licencia, revisión y hashes acompañan el contenido instalado.
- Un fallo parcial es visible y recuperable, sin declarar éxito ni borrar cambios ajenos.
- Las pruebas distinguen integridad de archivos, autenticidad del origen y conducta del agente; ninguna sustituye a las otras.
