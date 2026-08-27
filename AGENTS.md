# AGENTS.md

## Quick reference

- `npm run dev` — starts server on http://localhost:3000 (port via `PORTA` env var)
- `npm test` — runs tests using Node built-in test runner
- No `npm install` needed — zero dependencies
- Node >= 20 required

## Architecture

Layered MVC-style API, zero-dependency, using raw `node:http` (no Express/Koa/etc.).

```
src/servidor.js          — HTTP server factory, routing, body parsing
src/rotas/               — route definitions (method + regex pattern → handler)
src/controladores/       — request handlers (validate → call service → respond)
src/servicos/            — business logic
src/repositorios/        — in-memory data store
src/comum/               — shared: ErroDominio, response helpers, ID generation
verificacoes/            — tests (portuguese for "verifications")
```

All source files and variable names are in **Portuguese**. Match this convention.

## Data is in-memory only

No database. `reiniciar()` from `livros-repositorio.js` resets state. Tests call it in `beforeEach`. Server restarts wipe all data.

## Conventions

- **IDs**: format `liv_` + 8 hex chars (e.g. `liv_a1b2c3d4`). See `src/comum/identificador.js`.
- **Error codes**: `DADOS_INVALIDOS` (422), `NAO_ENCONTRADO` (404), `CONFLITO` (409), `ROTA_NAO_ENCONTRADA` (404), `JSON_INVALIDO` (400), `ERRO_INTERNO` (500). Errors are wrapped in `ErroDominio`.
- **Response envelope**: errors return `{ erro: { codigo, mensagem } }`. Success returns the resource directly. 204 has no body.
- **ESM**: `"type": "module"` in package.json. All imports use `.js` extensions.
- **No body on GET/DELETE**: body parsing only happens for POST/PUT/PATCH.

## Tests

- Framework: `node:test` + `node:assert/strict` (no Jest, no Mocha)
- Command: `npm test` (runs `verificacoes/**/*.spec.js`)
- Tests spin up a real server on a random port via `criarServidor()` + `.listen(0)`
- To add a test file: place it under `verificacoes/` with the `.spec.js` suffix

## Discovered from source (do not invent — copied from actual code)

### 1. HTTP status used by validation (`src/comum/erros.js`)

`dadosInvalidos()` returns `ErroDominio('DADOS_INVALIDOS', mensagem, 422)` → validation errors always use **HTTP 422**.

```js
export function dadosInvalidos(mensagem) {
  return new ErroDominio('DADOS_INVALIDOS', mensagem, 422);
}
```

### 2. Error response body format (`src/comum/respostas.js`)

`enviarErro()` always wraps errors in the envelope below. The field is `erro`, with sub-fields `codigo` and `mensagem` (not `message`).

```json
{ "erro": { "codigo": "DADOS_INVALIDOS", "mensagem": "..." } }
```

Unhandled (non-`ErroDominio`) errors return HTTP 500 with `codigo: "ERRO_INTERNO"`.  
HTTP 204 responses have **no body** (`res.end()` with no content).

### 3. ID format (`src/comum/identificador.js`)

`novoIdentificador(prefixo)` generates `${prefixo}_${randomBytes(4).toString('hex')}`.  
For books the prefix is `liv`, producing IDs like `liv_a1b2c3d4` (prefix + underscore + 8 lowercase hex chars).

```js
export function novoIdentificador(prefixo) {
  return `${prefixo}_${randomBytes(4).toString('hex')}`;
}
```

Regex used in tests to validate the format: `/^liv_[0-9a-f]{8}$/`

### 4. Test location and file suffix (`verificacoes/`)

- Test files live under `verificacoes/` and must end with `.spec.js`.
- Current file: `verificacoes/livros.spec.js`
- Tests use `before`/`after`/`beforeEach`/`describe`/`it` from `node:test` and `assert` from `node:assert/strict`.
- Each suite calls `reiniciar()` in `beforeEach` to reset in-memory state.
