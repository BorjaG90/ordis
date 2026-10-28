# Constitución — Cefalón Ordis

Principios innegociables. Toda especificación, plan, tarea o implementación debe cumplirlos.

## Propósito
Cefalón Ordis es un dashboard autoalojable, personalizable y multipestaña para navegadores web. Debe ser fácil de desplegar, mantener y extender.

## Principios
1. **Simplicidad primero**: Priorizar soluciones simples y directas sobre abstracciones prematuras.
2. **La spec manda**: Nada se implementa si no está en la especificación activa. Si falta una decisión, se detiene y se pregunta.
3. **Separación de responsabilidades**: Frontend, backend y base de datos deben estar claramente separados. La lógica de negocio no pertenece a los componentes de UI.
4. **Tipado estricto**: Tanto en TypeScript (frontend) como en Go (backend), se debe usar tipado estricto y evitar `any` cuando sea posible.
5. **Datos del usuario son sagrados**: Los datos persistidos deben ser tratados con cuidado. Las migraciones deben ser seguras, reversibles cuando sea posible y nunca provocar pérdida de datos.
6. **Seguridad por defecto**: Validación en servidor, manejo correcto de CORS, secretos fuera del código y principio de menor privilegio.
7. **Idioma**: Código, nombres de variables/funciones y commits en inglés. Interfaz de usuario, documentación y mensajes al usuario en español.

## Stack Tecnológico
- **Frontend**: [React](https://react.dev/) + [Vite](https://vitejs.dev/) + [TypeScript](https://www.typescriptlang.org/)
- **Backend**: [Go](https://go.dev/) (API REST)
- **Base de datos**: [SQLite](https://www.sqlite.org/) con migraciones versionadas
- **Despliegue**: Autoalojable (Docker/compose opcional, pero compatible)

## Estructura del Proyecto
Se adopta un enfoque monorepo con separación clara:

```
ordis/
├── web/          # Frontend React + Vite + TypeScript
├── server/       # Backend Go (API REST)
├── migrations/   # Migraciones SQL versionadas para SQLite
├── docs/         # Documentación (constitución, specs, etc.)
└── assets/       # Recursos estáticos
```

*Nota*: Si la estructura ya existe con nombres distintos, se debe adaptar respetando esta separación conceptual.

## Convenciones

### Frontend (web/)
- **Componentes**: Funcionales con hooks. Un componente por archivo cuando sea lógico.
- **Tipado**: TypeScript estricto. Props tipadas, evitar `any`.
- **Estado**: Preferir estado local; estado global solo si es necesario y justificado.
- **Llamadas API**: Centralizadas (cliente HTTP tipado). Nunca hacer `fetch` directamente disperso sin abstracción.
- **Estilos**: Consistentes con el proyecto existente. Evitar CSS global excesivo.
- **Imports**: Ordenados y usando paths coherentes con la configuración de Vite/TS.

### Backend (server/)
- **Arquitectura**: `handlers/` -> `services/` -> `repository/` (o equivalente). Separación clara de responsabilidades.
- **HTTP**: Usar `net/http` estándar o un router ligero justificado. Mantener handlers finos, lógica en services.
- **Errores**: Errores explícitos, con contexto. No tragar errores silenciosamente.
- **Validación**: Validar todas las entradas del cliente en el servidor.
- **Configuración**: Mediante variables de entorno. Proveer `.env.example`.
- **Logs**: Estructurados y útiles para debugging, sin información sensible.

### Base de Datos (SQLite)
- **Migraciones**: Versionadas, incrementales e idempotentes cuando sea posible. Ubicadas en `migrations/`.
- **Acceso**: Mediante `database/sql`. Usar queries preparadas.
- **Transacciones**: Usar transacciones para operaciones multi-tabla.
- **Esquema**: Tipos SQLite apropiados, claves primarias, índices cuando necesario.
- **Migraciones hacia atrás/adelante**: Pensar en compatibilidad con datos existentes.

### API
- **Formato**: JSON para requests/responses.
- **REST**: Convenciones RESTful coherentes (recursos, verbos HTTP correctos).
- **Códigos HTTP**: Usar códigos semánticamente correctos (200, 201, 400, 401, 403, 404, 409, 422, 500).
- **Errores**: Respuestas de error consistentes y predecibles.
- **CORS**: Configurado explícitamente para el origen del frontend en desarrollo/producción.

## Calidad y Desarrollo
- **Linting/Formatting**: Usar linters y formateadores del ecosistema (ESLint/Prettier para frontend, `gofmt`/`golangci-lint` para Go).
- **Typecheck**: `tsc --noEmit` debe pasar en frontend.
- **Tests**: Añadir tests cuando se implemente lógica compleja o crítica. No bloquear por tests inexistentes, pero sí corregir tests en rojo.
- **Build**: `vite build` para frontend y build nativo de Go deben funcionar.
- **Desarrollo local**: Debe ser posible levantar frontend y backend juntos de forma sencilla (documentado en README).
- **Variables de entorno**: Nunca commitear secretos. Usar `.env` local y `.env.example` versionado.

## Reglas para Agentes
- **Leer antes de editar**: Entender el contexto del archivo y archivos relacionados antes de modificar.
- **Seguir convenciones**: Mimicar el estilo, patrones y arquitectura existentes.
- **Cambios mínimos**: Hacer solo los cambios necesarios para resolver la tarea.
- **Verificar**: Ejecutar lint/typecheck/build cuando sea razonable tras cambios significativos.
- **Sin commits automáticos**: Solo commitear si el usuario lo pide explícitamente.
