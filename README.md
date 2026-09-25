# common-skills

Colección de skills reutilizables para agentes de programación. Cada skill contiene instrucciones para una tarea concreta y puede incorporarse a otros proyectos sin depender de este repositorio completo.

La colección cubre refinamiento de issues, planificación local, ejecución de una fase, revisión de entregas para GitHub, commits convencionales y preparación de releases con changelog. Consulta el [catálogo](skills/README.md) para elegir la skill y sus dependencias.

## Documentación

- [Guía de uso](docs/USAGE.md): qué skill elegir, entradas/salidas, ejemplos y aprobaciones.
- [Distribución propuesta](docs/DISTRIBUTION.md): copias por proyecto fijadas a SHA, bundles verificables y plan del futuro instalador. Distingue funcionalidades actuales de propuestas.
- [Catálogo](skills/README.md): instrucciones y dependencias de las seis skills.

## Organización

```text
skills/
  README.md                 Catálogo de skills publicadas
  <nombre>/
    SKILL.md                Instrucciones y metadatos de una skill
    references/             Documentación adicional, opcional
    scripts/                Automatizaciones, opcional
    assets/                 Plantillas y otros recursos, opcional
templates/
  skill/
    SKILL.md                Plantilla para crear nuevas skills
docs/
  USAGE.md                  Uso, ejemplos y límites de autorización
  DISTRIBUTION.md           Diseño propuesto de distribución segura
AGENTS.md                   Reglas de mantenimiento del repositorio
.editorconfig               Formato básico de los archivos
.gitignore                  Estado local, secretos y archivos temporales
LICENSE                     Licencia MIT
```

Las carpetas `<nombre>/` y sus recursos son ilustrativos: se crean al añadir una skill. No hay aplicación, dependencias de ejecución comunes ni paso de compilación.

## Crear una skill

Desde la raíz del repositorio, sustituye `mi-skill` por un nombre en minúsculas con guiones:

```bash
test ! -e skills/mi-skill && cp -R templates/skill skills/mi-skill
```

1. Edita `skills/mi-skill/SKILL.md`: sustituye el nombre, la descripción y todos los textos orientativos.
2. Haz coincidir el campo `name` con el nombre de la carpeta. La `description` debe explicar cuándo activar la skill, no solo qué contiene.
3. Escribe un procedimiento concreto, requisitos comprobables y criterios de validación. Indica el directorio de trabajo de cada comando.
4. Añade recursos solo si son necesarios y enlázalos desde `SKILL.md`. Las rutas a estos recursos son relativas a la carpeta de la skill, no al proyecto consumidor.
5. Prueba la skill en un proyecto representativo y añádela al [catálogo](skills/README.md), incluyendo sus dependencias externas.

Mantén `SKILL.md` centrado en las decisiones y el flujo principal. Traslada los detalles extensos a `references/` e indica cuándo consultarlos para evitar cargar contexto innecesario.

## Reutilizar en otros proyectos

### Copiar una skill

Para copiar la skill de commits, ejecuta desde la raíz del proyecto consumidor, sustituyendo la ruta a esta colección:

```bash
mkdir -p .agents/skills
test ! -e .agents/skills/git-conventional-commit && cp -R /ruta/absoluta/common-skills/skills/git-conventional-commit .agents/skills/git-conventional-commit
```

Después, solicita al agente un commit de los cambios concretos que quieras registrar. Pedir solo una propuesta de mensaje no autoriza a modificar el índice ni el historial. La skill no hace push ni carga contenido remoto; sus convenciones y escenarios de validación están incluidos en la carpeta.

Copia la carpeta completa, no solo `SKILL.md`: puede depender de recursos incluidos. La comprobación evita sustituir una carpeta existente. Una copia no recibe actualizaciones automáticamente; revisa las diferencias antes de actualizarla y conserva las personalizaciones del proyecto consumidor.

Crush descubre `.agents/skills`, `.crush/skills`, `.claude/skills` y `.cursor/skills` por defecto. Para otros agentes, comprueba sus rutas y compatibilidad; el uso de `SKILL.md` no garantiza que todos interpreten las mismas extensiones.

### Cargar la colección directamente con Crush

En el `crushrc` del proyecto consumidor o en tu configuración global de Crush, añade una ruta absoluta a la carpeta `skills/` de tu copia local:

```bash
option skill-path /ruta/absoluta/common-skills/skills
```

`option` es una función de configuración de Crush, no un comando para ejecutar directamente en una terminal. No apuntes a la raíz del repositorio ni a `templates/`, para evitar cargar la plantilla como una skill real. Este método utiliza los archivos compartidos directamente: cualquier modificación de la colección afecta a los proyectos que la carguen.

No es necesario configurar proveedores, claves ni permisos para distribuir esta colección. Revisa las instrucciones y los scripts antes de permitir su ejecución en otro proyecto.

## Flujo de trabajo

| Paso | Skill | Resultado y límite |
| --- | --- | --- |
| Refinar | [issue-refine-github](skills/issue-refine-github/SKILL.md) | Propone conservar, ampliar o dividir una issue; no crea hijas por defecto. |
| Planificar | [plan-create](skills/plan-create/SKILL.md) | Propone fases y guarda un plan local solo con contenido y destino aprobados. |
| Ejecutar | [plan-execute](skills/plan-execute/SKILL.md) | Implementa una fase aprobada, verifica y se detiene para revisión. |
| Preparar entrega | [delivery-review-github](skills/delivery-review-github/SKILL.md) | Propone actualización de issue y PR con evidencia; publica solo lo autorizado. |
| Registrar cambios | [git-conventional-commit](skills/git-conventional-commit/SKILL.md) | Crea un commit local únicamente ante petición explícita. |
| Preparar release | [release-prepare-github](skills/release-prepare-github/SKILL.md) | Separa changelog, versión/evidencia y publicación de una release aprobada; no crea tags implícitos. |

No necesitas recorrer todos los pasos. Una issue pequeña puede ejecutarse sin dividirla; una tarea sin GitHub puede planificarse y ejecutarse localmente; una entrega existente puede revisarse sin un plan creado por estas skills. Instala cada carpeta necesaria mediante el mismo procedimiento de copia anterior, sustituyendo el nombre de la skill.

Las integraciones son opcionales y transmiten datos, no permisos: fuente y revisión, objetivo, alcance, fase, contratos, aceptación, dependencias, verificación y evidencia de aprobación. Ninguna skill carga archivos de carpetas hermanas ni instala otras. El refinamiento admite un borrador de `plan-create`, pero GitHub sigue siendo la fuente de verdad de estados y dependencias; no se mantiene otro tablero local.

Ejemplos de peticiones al agente:

- «Revisa si esta issue necesita refinamiento; no modifiques GitHub».
- «Propón un plan local para esta tarea; enséñamelo antes de guardarlo».
- «Implementa únicamente la fase P1 de este plan aprobado».
- «Prepara la actualización de la issue y la PR de esta entrega, sin publicarlas».
- «Genera un borrador de changelog entre este tag y este SHA, sin escribir ni publicar».
- «Prepara la próxima release con propuesta de versión, evidencias y checksums; no crees tags».

### Activación y aprobaciones

Las skills nuevas permiten selección por el modelo (`disable-model-invocation: false`) y uso manual (`user-invocable: true`). La selección depende del agente: no hay un observador de GitHub, hook ni garantía de ejecución automática. Invocar una skill nunca autoriza todas sus operaciones.

Si quieres reforzar el flujo, puedes incorporar esta regla a las instrucciones del proyecto consumidor después de revisarla:

> Antes de implementar una issue, evalúa alcance, contratos, dependencias y trabajo existente. Si necesita refinamiento, propone los cambios sin mutar GitHub; usa `issue-refine-github` si está disponible. Implementa una sola fase aprobada y presenta evidencia antes de continuar o publicar.

No se modifica automáticamente la configuración de otros proyectos. Aprobar un plan no aprueba commit, push, publicación de PR, cierre de issues ni cambios de Project. Una lista explícita de operaciones y textos aprobada conjuntamente puede autorizar una publicación sin repetir preguntas por cada comando.

### Adaptación al backlog y seguridad

Las skills descubren gobernanza, idioma, baseline, plantillas, repositorios responsables, relaciones y campos de Project del consumidor. No fijan organizaciones ni IDs y no confunden padre/hija con dependencia. Conservan marcadores únicos de importación en su issue original y no convierten toda tarea en un epic.

Las convenciones y plantillas de las skills son locales; las operaciones GitHub sí necesitan red y autenticación mediante `gh`. No se descargan skills ni se amplían scopes automáticamente. Se consulta la ayuda de la versión instalada antes de usar opciones como `--parent`; hay una alternativa REST para vincular hijas y se declara cualquier limitación de host o permisos.

El modo propuesta no ejecuta comandos de escritura ni `gh pr create --dry-run`, que puede hacer push. La publicación revalida revisiones, detecta recursos existentes y conserva resultados parciales para evitar duplicados. Estas instrucciones no son un sandbox ni una garantía de resistencia a prompt injection.

### Releases y changelog

`release-prepare-github` tiene tres modos: solo changelog, preparación de release y publicación expresamente aprobada. Fija componente, línea de release y SHA candidato; no toma simplemente el tag de versión más alta. Contempla primera release, historial incompleto, rangos vacíos, prereleases y workspaces, y conserva entradas históricas y notas manuales.

La generación usa Git y, opcionalmente, git-cliff ya instalado con configuración local revisada y `--offline --no-exec`. No instala herramientas ni descarga configuración. La falta de git-cliff no impide redactar el changelog desde el historial.

La preparación incluye un manifiesto de revisiones, criterios, pruebas, artefactos, checksums y limitaciones. Una PR de versión/changelog puede pasar por `delivery-review-github`, pero su merge no autoriza la release. La publicación verifica el SHA real del tag remoto, trabaja primero con un draft y comprueba los assets antes de publicar; `--verify-tag` impide crear tags implícitos, pero no sustituye comparar el SHA. Tag, push, publicación, Latest, registries y workflows requieren su autorización correspondiente.

## Convenciones

- Skills, referencias, ejemplos y plantilla de autoría en inglés. La documentación de mantenimiento permanece en español.
- El idioma de las skills no impone el de sus resultados: planes, issues y mensajes siguen las reglas del proyecto consumidor o, si no existen, el idioma del usuario.
- Una responsabilidad concreta por skill; evita instrucciones genéricas duplicadas.
- Skills autocontenidas: sin rutas personales, dependencias de carpetas hermanas ni referencias a archivos internos de este repositorio.
- Declara herramientas y versiones requeridas en cada skill; no supongas que el consumidor comparte tu entorno.
- El contexto y las instrucciones del proyecto consumidor deben respetarse. Las skills no deben intentar saltarse permisos ni imponer cambios ajenos a su tarea.
- No incluyas secretos ni datos reales. Usa ejemplos ficticios y variables de entorno cuando corresponda.
- Mantén los archivos de licencia y avisos de atribución aplicables al redistribuir contenido.

## Comprobaciones

Actualmente no hay suite de tests, linter ni CI configurados. Para cambios de documentación, ejecuta desde la raíz:

```bash
git diff --check
git status --short
```

`git diff --check` no revisa archivos nuevos sin seguimiento: inspecciónalos también antes de incorporarlos. Revisa que el frontmatter contenga `name` y `description`, que el nombre coincida con la carpeta, que los enlaces relativos existan y que no queden textos de la plantilla.

Si una skill incorpora scripts, documenta y ejecuta sus comprobaciones específicas; no existe un comando de pruebas común para toda la colección. Verifica también que la carpeta siga funcionando al copiarse fuera del repositorio.

Para la skill de commits, utiliza sus [escenarios de validación](skills/git-conventional-commit/references/validation.md) en repositorios temporales. Las demás incluyen escenarios locales en su `SKILL.md` o referencias: [refinamiento](skills/issue-refine-github/references/refinement.md), [ejecución](skills/plan-execute/references/execution.md) y [entrega](skills/delivery-review-github/references/delivery-template.md). La skill de release incluye su [matriz de validación](skills/release-prepare-github/references/validation.md) para rangos, evidencias, tags, assets y fallos parciales.

Verifica enlaces después de copiar cada carpeta por separado y comprueba el modo propuesta, aprobaciones, fuentes obsoletas y recuperación parcial con respuestas simuladas. No hagas mutaciones de prueba contra un backlog real. Distingue sintaxis y mecánica de comandos de pruebas reales de GitHub y de la evaluación del agente ante contenido no confiable.

## Archivos locales

`.gitignore` excluye `.crush/` completo, archivos `.env`, temporales del editor y entornos/cachés de Python. Permite `.env.example` y `.env.*.example`, que nunca deben contener credenciales reales. Las skills publicadas deben vivir en `skills/`, no en `.crush/skills/`.

## Licencia

[MIT](LICENSE).
