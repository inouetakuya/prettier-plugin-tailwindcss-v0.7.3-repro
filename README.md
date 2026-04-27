# prettier-plugin-tailwindcss v0.7.3 regression repro

Minimal reproduction for [tailwindlabs/prettier-plugin-tailwindcss#456](https://github.com/tailwindlabs/prettier-plugin-tailwindcss/issues/456).

After upgrading `prettier-plugin-tailwindcss` from `0.7.2` to `0.7.3`, running `prettier --check .` throws `TypeError` for nearly every file.

## Environment

- prettier `3.6.2`
- prettier-plugin-tailwindcss `0.7.3`
- tailwindcss `4.1.18`
- Node.js v24
- pnpm `10.33.0`

## Steps to reproduce

```sh
pnpm install
pnpm format:check
```

## Expected output (errors observed)

Three distinct `TypeError`s are thrown depending on which Prettier parser handles the file:

| File                  | Parser     | Error                                       |
| --------------------- | ---------- | ------------------------------------------- |
| `next.config.ts`      | typescript | `TypeError: e.charAt is not a function`     |
| `eslint.config.mjs`   | babel      | `TypeError: a.startsWith is not a function` |
| `postcss.config.mjs`  | babel      | `TypeError: a.startsWith is not a function` |
| `src/app/globals.css` | postcss    | `TypeError: t.slice is not a function`      |

All four traces pass through `prettier-plugin-tailwindcss/dist/index.mjs:303` (the wrapped `parser.parse`).

## Verifying the regression

Pinning to `0.7.2` makes every error disappear (only formatting warnings remain):

```sh
pnpm add -D prettier-plugin-tailwindcss@0.7.2
pnpm format:check
```
