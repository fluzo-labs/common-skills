# Catálogo de skills de Fluzo

| Skill | Cuándo usarla | Dependencias |
| --- | --- | --- |
| [git-conventional-commit](git-conventional-commit/SKILL.md) | Crear un único commit local solicitado por el usuario, con alcance revisado y mensaje Conventional Commits. | Git y permisos sobre el repositorio consumidor; sin red ni instalaciones adicionales. |
| [issue-refine-github](issue-refine-github/SKILL.md) | Evaluar una issue existente y proponer conservarla, ampliarla o descomponerla sin duplicar el backlog. | GitHub CLI y acceso de lectura; escritura y Project solo con permisos y aprobación específicos. |
| [plan-create](plan-create/SKILL.md) | Proponer y guardar un plan local aprobado con contratos, fases y criterios verificables. | Acceso al proyecto; Git opcional y GitHub CLI solo para fuentes remotas. |
| [plan-execute](plan-execute/SKILL.md) | Implementar una fase aprobada, actualizar progreso local y ofrecer tres mensajes de commit con evidencia. | Herramientas del consumidor; GitHub CLI solo para fuentes de GitHub. |
| [delivery-review-github](delivery-review-github/SKILL.md) | Proponer push, comentario y PR revisados; tras un merge verificado, recomendar la siguiente issue elegible sin implementarla. | Git, GitHub CLI y contexto de la entrega o PR; permisos adicionales solo para operaciones aprobadas. |
| [release-prepare-github](release-prepare-github/SKILL.md) | Generar changelog, preparar versión y evidencia, o publicar una release aprobada desde un tag y SHA verificados. | Git; GitHub CLI para operaciones remotas; git-cliff opcional y herramientas de verificación del consumidor. |
| [convention-document](convention-document/SKILL.md) | Convertir acuerdos confirmados en convenciones con ejemplos, excepciones e índice actualizado. | Acceso a documentación/código; skill de commits instalada solo para el handoff de commit. |

El recorrido guiado de Fluzo es refinar → planificar → ejecutar una fase → actualizar progreso → elegir commit → preparar push y PR → revisar/mergear bajo las reglas del consumidor → verificar merge → recomendar siguiente issue/fase o preparar release. Cada skill cierra con un paso recomendado, motivo y respuesta breve ligados al estado real, salvo restricciones superiores de respuesta. Los bloqueos se resuelven antes de avanzar. La recomendación post-merge identifica una issue existente con título y enlace, prioriza el plan actual y no cambia asignaciones ni estados; no hay observador automático. La PR de la última fase incluye cierre de hija y padre si las fases previas y los criterios globales están verificados. Aprobar una entrega o fusionar su PR no autoriza tags ni publicación. No se encadenan mutaciones automáticamente. El paso de commit requiere resolver `git-conventional-commit` instalada por nombre; sin ella, se detiene ese paso sin descargarla. Los permisos y las aprobaciones se comprueban en cada paso; GitHub conserva la autoridad del estado remoto.

La plantilla vive en `../templates/skill/`, fuera de esta carpeta para que los agentes no la carguen como una skill real.

Cada skill publicada ocupa una carpeta `skills/<nombre>/` con un archivo `SKILL.md`. Usa nombres en minúsculas con guiones y el mismo valor en el campo `name` del frontmatter.

Al publicar una skill, añade aquí un enlace a su `SKILL.md`, una descripción breve de cuándo usarla y sus dependencias externas. No registres plantillas ni borradores como skills disponibles.

Cada carpeta debe poder copiarse de forma independiente a otro proyecto. Guarda sus recursos opcionales en `references/`, `scripts/` y `assets/` dentro de la propia carpeta; créalos solo cuando tengan contenido.
