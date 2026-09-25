# Uso de las skills

## Qué instalas

Una skill es una carpeta de instrucciones, no un servicio ni un comando de shell. El agente lee `SKILL.md` y consulta sus referencias locales cuando corresponde. No hay un ejecutor central que encadene las skills, vigile GitHub o conceda permisos.

Las seis skills están redactadas en inglés. Sus resultados respetan el idioma del proyecto consumidor o, si no está definido, el del usuario. No hace falta instalar toda la colección: cada carpeta funciona por separado.

Consulta el [catálogo](../skills/README.md) para ver dependencias y el [README](../README.md) para copiar una skill o configurar su carga en Crush. Para equipos, sigue la [propuesta de distribución versionada](DISTRIBUTION.md), que distingue lo disponible de lo pendiente de construir.

## Primera utilización

1. Elige una revisión de la colección y revisa el contenido que vas a cargar.
2. Copia la carpeta completa de la skill, incluidas sus referencias, a la ruta admitida por el agente. Conserva los avisos de licencia aplicables al redistribuirla.
3. Comprueba que el agente la descubre. No asumas que todos los agentes admiten los mismos campos de frontmatter o rutas.
4. Trabaja desde el proyecto consumidor y proporciona la tarea, las rutas o las URLs reales relevantes. No ejecutes `SKILL.md` con un shell.
5. Empieza con una propuesta sin escrituras. Verifica que el agente distingue el destino, el alcance y las operaciones que necesitan aprobación.

En Crush, `user-invocable: true` permite invocación manual y las skills de proyecto aparecen como `project:nombre`; las globales como `user:nombre`. También puedes pedir la tarea en lenguaje natural y nombrar la skill. La selección por el modelo no es una garantía de activación ni un mecanismo de autorización.

## Elegir la skill

| Necesidad | Skill | Entrada mínima | Salida esperada |
| --- | --- | --- | --- |
| Registrar un cambio concreto | [git-conventional-commit](../skills/git-conventional-commit/SKILL.md) | Petición explícita de commit y alcance de archivos/cambios | Un commit local revisado o un bloqueo concreto |
| Concretar trabajo del backlog | [issue-refine-github](../skills/issue-refine-github/SKILL.md) | Issue existente y contexto del repositorio | Propuesta de conservar, ampliar o dividir; publicación solo aprobada |
| Diseñar una tarea | [plan-create](../skills/plan-create/SKILL.md) | Objetivo y proyecto, opcionalmente una issue | Plan por fases; archivo solo con contenido y destino aprobados |
| Implementar trabajo aprobado | [plan-execute](../skills/plan-execute/SKILL.md) | Fase, plan/issue, revisión y autorización | Implementación de una fase y evidencia de verificación |
| Preparar revisión de código | [delivery-review-github](../skills/delivery-review-github/SKILL.md) | Cambios, issue, fase y resultados reales | Comentario y PR propuestos; publicación solo aprobada |
| Preparar una versión | [release-prepare-github](../skills/release-prepare-github/SKILL.md) | Modo, componente, rango/SHA y política de versión | Changelog, preparación o release expresamente autorizada |

## Ejemplos por skill

### Commit

> Usa git-conventional-commit para crear un commit únicamente con los cambios de documentación que acabamos de revisar. No incluyas otros cambios ni hagas push.

La skill inspecciona el índice completo y los archivos sin seguimiento. Si hay cambios ajenos preparados o fragmentos mezclados que no puede seleccionar de forma segura, se detiene: no limpia el entorno para facilitar el commit. Una petición de mensaje de commit por sí sola no autoriza registrarlo.

### Refinamiento de una issue

> Usa issue-refine-github para revisar esta issue: [URL real]. Comprueba criterios, contratos, dependencias y PRs existentes. Propón cambios sin modificar GitHub.

La división no es obligatoria. Una issue pequeña puede necesitar solo una aclaración, mientras que una entrega amplia puede necesitar hijas. Mantiene los identificadores del importador en la issue original, distingue padre/hija de bloqueo y no mueve a Ready por haber terminado el análisis.

### Plan local

> Usa plan-create para proponer la implementación de [objetivo]. Explica los contratos afectados y las comprobaciones de cada fase. Muéstrame el plan antes de guardarlo.

El destino por defecto es `.agents/plans/YYYY-MM-DD-name/YYYY-MM-DD-name-plan.md` en el consumidor, salvo otra convención. El archivo se guarda después de aprobar contenido y destino. Si procede de GitHub, no se convierte en un segundo tablero de estados.

### Ejecución de una fase

> Usa plan-execute para implementar solo P1 del plan aprobado en [ruta real]. Conserva los cambios previos y presenta las verificaciones antes de avanzar.

También acepta una issue/sub-issue con alcance aprobado y verificable. Revisa vigencia y prerrequisitos antes de editar. Termina con resultados por criterio, separando verificado, fallido, no ejecutado y bloqueado. No avanza a P2 aunque parezca sencillo.

### Entrega a revisión

> Usa delivery-review-github para preparar la actualización de [issue real] y la PR de esta fase. Incluye alcance, motivación, decisiones, pruebas, riesgos y pendientes. No publiques todavía.

Consulta una PR existente antes de crear otra. La propuesta identifica base/head, revisión probada y semántica de cierre. Una fase parcial referencia la issue sin cerrarla; completar una hija no cierra su padre. El formato de la herramienta puede exigir una PR breve: la evidencia detallada puede quedar enlazada desde la issue.

### Release y changelog

> Usa release-prepare-github en modo solo changelog para el rango entre [tag base real] y [SHA candidato real]. Presenta las notas sin escribir archivos ni crear tags.

> Prepara la release de [componente]: propón versión, notas, manifiesto de evidencia y artefactos. No hagas commit, tag, push ni publicación.

> Publica el draft aprobado [release real] únicamente si conserva el SHA, notas, canal y checksums del manifiesto que hemos aprobado.

La skill no elige simplemente el tag de versión más alta. Verifica línea de release y ascendencia, distingue primera versión e historial incompleto, y comprueba el SHA remoto del tag. Si falta el tag, necesita autorización específica para crearlo y subirlo. git-cliff es opcional; no se instala automáticamente. Publicar en GitHub no implica publicar paquetes en un registry.

## Flujo combinado sin permisos implícitos

Un recorrido posible es:

```text
issue existente -> propuesta de refinamiento
                -> borrador local opcional
                -> aprobación y actualización del backlog
                -> ejecución de una fase aprobada
                -> evidencia y propuesta de PR
                -> publicación/revisión/merge autorizados
                -> preparación de release
                -> aprobación específica de tag y publicación
```

También puedes empezar directamente en cualquier paso. El traspaso incluye fuente y revisión, alcance, fase, criterios, dependencias y evidencia; no transmite autoridad. El siguiente agente revalida el estado. No hay dependencia de archivos entre carpetas de skills.

## Qué autoriza cada petición

| Petición | No autoriza por sí sola |
| --- | --- |
| Analizar o proponer | Escribir archivos o mutar GitHub |
| Guardar un plan aprobado | Implementarlo o publicarlo remotamente |
| Implementar una fase | Commit, push, PR o siguiente fase |
| Crear un commit | Push o tag |
| Abrir una PR | Merge, cerrar una issue parcial o publicar una release |
| Fusionar una PR de release | Tag o publicación, salvo una autorización explícita aplicable al flujo |
| Crear un draft de release | Publicación final, Latest o registry |

Una aprobación conjunta puede cubrir una lista exacta de operaciones, destinos y contenido sin repetir preguntas. Si cambian el alcance, SHA, texto, artefactos o canal de forma material, hay que reconciliar y renovar la aprobación correspondiente. Las reglas superiores y permisos de la herramienta siguen vigentes.

## Problemas frecuentes

- **No aparece la skill:** comprueba ubicación, carpeta completa, frontmatter y soporte del agente. En Crush, una ruta personalizada debe apuntar a `skills/`, no a la raíz de esta colección o a `templates/`.
- **Falta acceso a GitHub:** el agente puede preparar una propuesta con limitaciones, pero no debe declarar verificadas relaciones o permisos que no pudo leer. No compartas tokens en la conversación.
- **Hay cambios locales ajenos:** no se resuelven con reset, stash o staging indiscriminado. Delimita la tarea sin destruir trabajo.
- **Una operación remota devuelve timeout:** el recurso puede existir. Consulta primero y reanuda lo pendiente; no repitas creaciones ni borres para simular rollback.
- **El código pasó pruebas pero falta evidencia de release:** una PR o una simulación no prueba distribución, checksums ni aceptación del producto final.
- **Quieres actualizar las skills:** revisa diferencias, conserva personalizaciones y fija la nueva revisión. No actualices automáticamente una carpeta compartida mientras agentes la utilizan.

## Límites de validación

Esta colección contiene instrucciones y escenarios de mantenimiento, no una garantía de cumplimiento del modelo. Las verificaciones de enlaces, sintaxis y mocks de comandos no prueban permisos reales, resistencia a inyecciones ni integridad de artefactos remotos. Las pruebas reales requieren un entorno y destino expresamente autorizados.
