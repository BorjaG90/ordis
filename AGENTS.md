# AGENTS.md — Guía para agentes en Cefalón Ordis

Este archivo proporciona directrices para agentes que trabajen en este repositorio.

## Reglas
- Lee `docs/constitution.md` y la spec activa (`specs/NNN-*/`) antes de tocar código.

## Principios rápidos
- Seguir estrictamente la constitución. Si hay conflicto, la constitución prevalece.
- Código en inglés, UI/documentación en español.
- Cambios mínimos, lectura previa obligatoria.
- Sin commits automáticos.

## Estructura esperada
- `web/` — React + Vite + TypeScript (frontend)
- `server/` — Go API REST (backend)
- `migrations/` — SQL migrations for SQLite
- `docs/` — documentación
- `assets/` — recursos

## Comandos comunes (ajustar si difieren)
- **Frontend dev**: `cd web && npm run dev` (o `pnpm/yarn`)
- **Frontend build**: `cd web && npm run build`
- **Frontend lint/typecheck**: `cd web && npm run lint && npx tsc --noEmit`
- **Backend dev/run**: `cd server && go run ./...`
- **Backend build**: `cd server && go build ./...`
- **Backend test**: `cd server && go test ./...`
- **Migrations**: según herramienta elegida (golang-migrate, goose, etc.)

## Convenciones de trabajo
- **Antes de editar**: leer archivo + imports/contexto. Buscar patrones similares.
- **Nuevos archivos**: seguir estructura y naming existentes.
- **API**: mantener contrato tipado entre frontend/backend (considerar tipos compartidos si existen).
- **Errores**: nunca exponer stack traces sensibles al cliente.
- **Config**: usar env vars, documentar en `.env.example`.
- **SQLite**: migraciones seguras, sin `DROP TABLE` destructivo sin justificación y plan de migración.

## Checklist al terminar
- [ ] Cambios revisados y mínimos
- [ ] Lint/typecheck/build OK si aplicable
- [ ] Sin secretos commiteados
- [ ] Documentación actualizada si cambia API/config
- [ ] Sigue estructura y convenciones
