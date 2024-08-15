# Contributing to n8n-error-handling-pattern

Thank you for contributing. This repo maintains production-quality n8n error handling patterns — contributions must meet the same bar.

## Before You Submit

- [ ] Workflow JSON has `active: false`
- [ ] `meta.instanceId` is stripped from workflow JSON
- [ ] Root-level `id` is stripped from workflow JSON
- [ ] No API keys, tokens, or real credentials in any file
- [ ] All sample data uses fictional values (555-format phones, `matter_99999` IDs)
- [ ] Node names follow the `CATEGORY - Action (System)` convention
- [ ] README or inline sticky notes explain non-obvious decisions

## Workflow JSON Standards

- Use the `Code` node — not the deprecated `Function` node
- Pin `typeVersion` to the version you tested against
- Set `active: false` in the root of the JSON
- Strip instance-specific fields: `meta.instanceId`, root `id`

## Pull Request Process

1. Fork the repo and create a branch: `feat/your-pattern-name`
2. Add or update workflow JSON in `workflows/`
3. Add matching sample payloads in `payloads/` (success + failure paths)
4. Update `CHANGELOG.md` with your addition
5. Run the pre-submit checklist above
6. Open a PR — the CI will validate JSON syntax and check for credentials

## Reporting Issues

Use the issue templates provided in `.github/ISSUE_TEMPLATE/`.
