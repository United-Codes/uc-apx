# uc-apx

A CLI for reading, scaffolding, and validating Oracle APEX applications exported
in the **APEXlang** (`.apx`) format that ships with **Oracle APEX 26.1**. Built
for developers and AI agents alike.

`uc-apx` is a single static binary — no runtime required. Output
is minified JSON by default (directly parseable), with `--json-pretty` for humans
and `--toon` for token-efficient agent workflows.

## 📖 Documentation

**Full documentation lives at [united-codes.com/products/uc-apx/docs](https://united-codes.com/products/uc-apx/docs).**

- [Installation](https://united-codes.com/products/uc-apx/docs/getting-started/)
- [Quickstart](https://united-codes.com/products/uc-apx/docs/getting-started/quickstart/)
- [Command reference](https://united-codes.com/products/uc-apx/docs/commands/)
- [Changelog](https://united-codes.com/products/uc-apx/docs/other/changelog/)

## Install

Download the latest binary for your platform from the
[releases page](https://github.com/United-Codes/uc-apx/releases), then drop it on
your `PATH`. See the [installation docs](https://united-codes.com/products/uc-apx/docs/getting-started/)
for per-platform commands.

## What it does

- **Read** — inspect any construct in an exported app: `overview`, `pages`,
  `page`, `tree`, `search`, `lov`, `refs`, `deps`, `schema`, and more.
- **Scaffold** — create pages, regions, items, processes, validations, and other
  constructs from bundled templates.
- **Edit & delete** — surgically reshape an app without re-scaffolding whole pages.
- **Validate** — fast local structural checks, or `validate --official` to wrap
  SQLcl's `apex validate` for full-schema verification.

```bash
uc-apx --app-dir my-app overview
uc-apx --app-dir my-app create page --id 10 --name "Employees" --type interactive-report
uc-apx --app-dir my-app validate --official
```

See the [command reference](https://united-codes.com/products/uc-apx/docs/commands/)
for everything.

## Support

For enterprise support, training, or help, [contact us here](https://www.united-codes.com/?context=uc-apx#contact).

Otherwise, please use GitHub issues and discussions for community questions. Please note that there is no guarantee of a timely response for community support.

## License

© [United Codes](https://united-codes.com).
