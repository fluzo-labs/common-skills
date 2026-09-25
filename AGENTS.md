# Guía para agentes

## Alcance y estructura

Este repositorio distribuye instrucciones reutilizables, no una aplicación. Contiene una plantilla y siete skills: `git-conventional-commit`, `issue-refine-github`, `plan-create`, `plan-execute`, `delivery-review-github`, `release-prepare-github` y `convention-document`. No inventes comandos de compilación, despliegue o pruebas de una aplicación inexistente.

- `skills/` es la única raíz de distribución. Cada skill publicada vive en `skills/<nombre>/SKILL.md`.
- `templates/skill/SKILL.md` es material de autoría, no una skill publicada. Mantén las plantillas fuera de las rutas de carga de los agentes.
- `skills/README.md` es el catálogo: actualízalo al añadir, eliminar o renombrar una skill.
- `README.md` explica creación, copia y configuración de Crush. Consúltalo antes de cambiar el flujo de distribución.
- `docs/USAGE.md` documenta entradas, salidas, ejemplos y aprobaciones de las siete skills. Actualízalo cuando cambien sus contratos.
- `docs/DISTRIBUTION.md` contiene la propuesta de distribución fijada a SHA, bundles e instalador. No presentes esas funciones como implementadas hasta que existan y estén verificadas.

## Cambios en skills

Usa la plantilla existente. El nombre de carpeta debe estar en minúsculas con guiones y coincidir con `name` en el frontmatter. `description` funciona como criterio de activación: explica las solicitudes para las que sirve la skill.

Mantén el procedimiento principal en `SKILL.md`; enlaza los detalles extensos desde `references/` y explica cuándo leerlos. Los recursos ejecutables y las plantillas pertenecen a `scripts/` y `assets/` dentro de la misma skill. No crees carpetas opcionales vacías.

La unidad de distribución es la carpeta completa. No introduzcas dependencias de archivos de otras skills, de la raíz de este repositorio ni de rutas absolutas del autor. El paso guiado de commit usa la skill instalada `git-conventional-commit` por nombre; si falta, se detiene ese paso, sin descargarla ni duplicar su ejecutor. Distingue las rutas relativas a la skill de las rutas del proyecto consumidor y explicita el directorio de trabajo de los comandos.

Antes de publicar, elimina los textos orientativos de la plantilla, verifica el procedimiento en un proyecto representativo y declara todas las dependencias externas. Las instrucciones deben respetar las reglas y permisos del proyecto consumidor.

## Comprobaciones

Desde la raíz del repositorio:

```bash
git diff --check
git status --short
```

No hay suite de tests, linter, manifiesto de dependencias ni CI configurados. No presentes estas comprobaciones de Git como pruebas funcionales de las skills. `git diff --check` omite los archivos nuevos sin seguimiento: revísalos expresamente.

Comprueba manualmente los campos del frontmatter, los enlaces locales, las coincidencias entre carpeta y nombre, y la ausencia de secretos y textos orientativos. Si cambias scripts de una skill, ejecuta las comprobaciones que esta documente y verifica su uso desde una copia fuera del repositorio.

Para `git-conventional-commit`, lee `skills/git-conventional-commit/references/validation.md`. Prueba los comandos en repositorios temporales sin remotos ni credenciales, nunca creando commits de prueba en esta colección. Las pruebas mecánicas de Git no certifican resistencia del agente a prompt injection.

Todas las skills están redactadas de forma independiente y son autocontenidas: no añadas referencias a proveedores externos de skills, procedencia de implementaciones externas, descargas ni actualizaciones automáticas. Usa Fluzo como marca de la colección/plataforma, no como identidad ficticia del runtime. El cierre `Prepared with Fluzo skills` es opcional según las reglas de respuesta y no se inserta en archivos del consumidor sin aprobación. La atribución de commits depende de las instrucciones del consumidor, no de una identidad inventada por la skill. Mantén la autorización explícita, la conservación del staging parcial y la prohibición de saltarse hooks o hacer push como invariantes de la skill de commits.

## Planificación y entrega

- Las seis skills de planificación, ejecución, entrega, release y documentación permiten selección por el modelo, pero no hay hooks ni automatización externa. Seleccionar una skill no autoriza mutaciones; conserva las barreras de aprobación de cada paso.
- `issue-refine-github` refina trabajo existente; dividir es opcional. `plan-create` guarda un borrador aprobado; `plan-execute` implementa una sola fase, actualiza el progreso local autorizado y ofrece tres mensajes de commit; `delivery-review-github` propone o publica únicamente la entrega autorizada.
- Los traspasos son opcionales y transmiten fuente/revisión, alcance, fase, contratos, aceptación, dependencias y evidencia. No añadas imports ni enlaces a archivos de skills hermanas. Recomienda instalar la colección para el recorrido completo; para registrar un commit, resuelve y lee `git-conventional-commit` instalada por nombre. Una opción de commit elegida no aprueba push ni acepta la fase.
- Descubre convenciones en el consumidor. No fijes nombres de organización, repositorio, Project, campos ni estados. Las skills están en inglés, pero sus resultados siguen el idioma del consumidor o, si no está definido, el del usuario.
- Conserva IDs únicos del importador solo en la issue original; diferencia jerarquía, dependencias y campos de Project. Un plan local no debe convertirse en una segunda fuente de estados remotos.
- Comprueba comandos contra la ayuda de `gh` instalado. `gh pr create --dry-run` puede hacer push y no sirve como simulación segura. Usa mocks sin red para validar argumentos y declara que no prueban permisos ni conducta del modelo.
- Los escenarios de mantenimiento están dentro de cada skill. Verifica portabilidad por separado, cero escrituras sin aprobación, fase única, evidencia ligada a la revisión, reintentos sin duplicados y ausencia de cierres de padre en fases intermedias, y cierre de hija y padre mediante la PR final solo tras verificar fases previas y criterios globales.
- Crear estas skills no autoriza modificar el backlog del usuario, sus Projects, PRs ni otros repositorios. Las pruebas remotas requieren autorización específica.

## Progreso local y documentación

- El contrato de seguimiento está en `skills/plan-execute/references/progress.md` y debe coincidir con la plantilla de `plan-create`. Anuncia el archivo actualizado como parte de la ejecución; respeta restricciones de solo lectura y cambios concurrentes.
- Distingue implementación, verificación y revisión humana. `has_completed_all_phases` es booleano y solo es verdadero cuando también hay aceptación y criterios globales satisfechos. Una checklist antigua no prueba aceptación.
- Ofrece tres mensajes numerados basados en cambios reales; no fuerces tipos distintos. Sin cambios, no inventes un commit. Sin la skill de commits, las propuestas son provisionales y el handoff se detiene.
- `convention-document` registra acuerdos confirmados con motivación, ejemplos, excepciones e índice actualizado. Una corrección no autoriza por sí sola escritura o política permanente. No exportes conversaciones privadas.
- El cierre de padre ocurre al merge aplicable de la PR final, no al abrirla ni desde el refinamiento. Revalida todas las hijas, razón de cierre, evidencia, rama destino y soporte entre repositorios.

## Releases

- `release-prepare-github` separa solo changelog, preparación y publicación. Sus referencias locales contienen criterios de rango/versión, manifiesto, publicación y validación. No es un publisher automático ni instala git-cliff o CI.
- Preserva la aprobación ligada al repositorio, versión, SHA final, notas, assets/checksums y canal. Merge de una PR no equivale a autorización de tag o release. El tag remoto debe resolverse hasta el commit: `--verify-tag` solo verifica existencia.
- Mantén draft-first, uploads explícitos sin `--clobber`, reconciliación antes de reintentos y decisiones explícitas para prerelease/Latest. No compitas con un workflow de release existente ni publiques paquetes por implicación.
- git-cliff es opcional: revisa configuración local y overrides, exige `--offline --no-exec` cuando se use y no descargues config. Si no está instalado, usa Git; declara que no se probó el generador real.
- Valida publicación mediante mocks sin red y portabilidad de la carpeta. No crees releases o tags reales para probar estas instrucciones. Los mocks no prueban decisiones del agente, permisos remotos ni integridad real de artefactos.

## Convenciones y riesgos

- Escribe los archivos `SKILL.md`, sus referencias, ejemplos, la plantilla de autoría y `README.md` en inglés. Conserva `README.es.md` como versión española y mantén ambas versiones sincronizadas y enlazadas entre sí. El resto de documentación de mantenimiento permanece en español. Respeta `.editorconfig`: UTF-8, LF, salto de línea final y dos espacios de indentación. Markdown permite espacios finales para saltos de línea explícitos.
- `.crush/` está ignorado por completo porque contiene estado local. No guardes allí skills que deban distribuirse; usa `skills/`.
- Los ejemplos `.env.example` y `.env.*.example` son versionables, pero no deben contener credenciales reales.
- No amplíes automáticamente permisos ni añadas configuración de proveedores para distribuir skills.
- En Crush, `option skill-path` pertenece al `crushrc`, no a una sesión de shell normal. La ruta debe apuntar a `skills/`, nunca a la raíz ni a `templates/`.
- Una copia en otro proyecto no se actualiza sola. Una ruta compartida de Crush refleja inmediatamente los cambios locales de esta colección; considera ese impacto al modificar una skill existente.
- Conserva `LICENSE` y los avisos de atribución aplicables al incorporar recursos externos.
