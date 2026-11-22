# fix(core): detect Copilot Auto via model auto flag

Closes #65464

## Resumen
Tras GitHub eliminar `billing.auto_discount` de `/models`, Copilot Free no mostraba modelos. `RemoteModel` no modelaba el flag `auto: true` y `autoModel()` filtraba solo por `billing.auto_discount`. Se añade `auto: Schema.optional(Schema.Boolean)` y se incluye `model.auto === true` en el pool, conservando `auto_discount` por retrocompatibilidad.

## Plan de prueba
Una prueba enfocada en `packages/core/test/github-copilot/models.test.ts` con payload sintético servido por `Bun.serve({port:0})` y control negativo. Rojo: `expect(auto).toBeDefined()` → `undefined`. Verde: 3/3; `--rerun-each 10` 30/30; 20 repeticiones 60/60 sin servidor colgado (`try/finally server.stop(true)`).

## Cambios
- `packages/core/src/github-copilot/models.ts`
- `packages/core/test/github-copilot/models.test.ts`

## Cierre de issues
Closes #65464. Condición no bloqueante: el issue es `status/repro-simulated`; el mantenedor debe confirmar el payload real de `/models`. Sin bandera (bugfix), sin dependencias nuevas.
