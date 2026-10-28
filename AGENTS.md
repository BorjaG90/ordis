# AGENTS.md — Cefalón Ordis

Cefalón Ordis es un dashboard autoalojable y personalizable para múltiples navegadores web. Está pensado para uso personal (self-hosting), con el objetivo de ofrecer un punto de partida unificado para enlaces, widgets y configuraciones propias.

## Stack y estructura
- **Frontend:** React + Vite + TypeScript
- **Backend:** Go (API REST)
- **Base de datos:** SQLite
- **Otros:** Migraciones SQL versionadas, assets estáticos

**Estructura:**
- `web/` — Frontend React + Vite + TypeScript
- `server/` — Backend Go (API REST)
- `migrations/` — Migraciones SQL versionadas para SQLite
- `docs/` — Documentación (incluye `constitution.md`)
- `assets/` — Recursos estáticos

## Comandos
- **Frontend dev:** `cd web && npm run dev`
- **Frontend build:** `cd web && npm run build`
- **Frontend lint/typecheck:** `cd web && npm run lint && npx tsc --noEmit`
- **Backend dev/run:** `cd server && go run ./...`
- **Backend build:** `cd server && go build ./...`
- **Backend test:** `cd server && go test ./...`

## Convenciones
- **Idioma:** Código, variables, funciones y commits en inglés. Interfaz, documentación y mensajes al usuario en español.
- **Estilo:** Seguir el estilo y patrones ya existentes en el archivo/módulo que se edite. Usar `gofmt` en Go. Usar el formateador/linter configurado en el frontend.
- **Tipado:** TypeScript estricto. Evitar `any`. En Go usar tipado explícito.
- **Patrones:** Componentes funcionales + hooks en React. En Go: handlers finos, lógica en `services/`, acceso a datos en `repository/` cuando aplique.
- **Archivo de referencia:** [`docs/constitution.md`](docs/constitution.md)

## Reglas de dominio / trampas conocidas
- **Migraciones SQLite:** No hacer `DROP TABLE` destructivo sin una justificación clara y un plan de migración. Pensar siempre en compatibilidad con datos existentes.
- **Datos del usuario:** Son sagrados. Cualquier cambio en persistencia debe minimizar el riesgo de pérdida de datos.
- **Secretos:** Nunca commitear `.env`, claves, tokens o credenciales. Usar `.env.example` para documentar variables necesarias.
- **API/Contrato:** Mantener tipado coherente entre frontend y backend. Los errores nunca deben exponer stack traces al cliente.
- **CORS:** Debe estar configurado explícitamente entre frontend y backend.

## Forma de trabajar
- **Planificar antes de tocar:** Si el cambio es complejo (más de 1 archivo, afecta API, esquema o configuración), leer contexto, buscar patrones similares y planificar los pasos antes de editar.
- **Cambios mínimos:** Hacer solo lo necesario para cumplir la tarea. Evitar refactors no solicitados.
- **Explicar al terminar:** Resumir qué se ha cambiado, qué archivos se han tocado y qué verificación se ha realizado. Sin commits automáticos.

## Límites
- ✅ **Siempre:** Leer archivo + imports/contexto antes de editar, seguir `docs/constitution.md`, validar entradas en servidor, usar tipado estricto, mantener separación de responsabilidades.
- ⚠ **Pregunta antes:** Añadir dependencias nuevas, crear archivos o carpetas nuevas, modificar esquema de base de datos o migraciones, cambiar el contrato de la API, modificar configuración de build, CORS, autenticación o variables de entorno.
- 🚫 **Nunca:** Hacer commits o push automáticos, exponer stack traces al cliente, commitear secretos, eliminar o renombrar migraciones existentes sin motivo justificado y plan de migración, romper compatibilidad con datos existentes sin alternativa.

## Verificación
- **Frontend:** `cd web && npm run lint && npx tsc --noEmit && npm run build`
- **Backend:** `cd server && gofmt ./... && go build ./... && go test ./...` (ejecutar si existen tests o tras cambios relevantes)
- **Integración:** Comprobar comunicación entre frontend y API, y que las migraciones aplican correctamente en un entorno local.
- **Final:** Revisar diff, asegurar ausencia de secretos y confirmar que el cambio cumple con las convenciones.
