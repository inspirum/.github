# inspirum/.github

Shared GitHub configuration for [inspirum](https://github.com/inspirum) repositories.

## Community health files

Defaults for every repository that does not have its own copy:

- [Code of conduct](docs/CODE_OF_CONDUCT.md)
- [Contributing guide](docs/CONTRIBUTING.md)
- [Issue template](.github/ISSUE_TEMPLATE.md)
- [Pull request template](.github/PULL_REQUEST_TEMPLATE.md)

## Reusable workflows

### PHP

[`php.yml`](.github/workflows/php.yml) runs on every PHP library:

- **Lint** (once, on `lint-php`): `phpcs` and `composer-dependency-analyser`
- **Test** (per PHP version in `php`): `phpstan` and `phpunit`, plus a coverage upload to Scrutinizer on `coverage-php`

Paths are read from each repository's `phpcs.xml`, `phpstan.neon` and `phpunit.xml.dist`.

```yaml
name: Test

on:
  - push
  - pull_request
  - workflow_dispatch

jobs:
  php:
    uses: inspirum/.github/.github/workflows/php.yml@main
    with:
      php: '["8.2", "8.3", "8.4", "8.5"]'
      coverage-php: '8.2'
```

| Input          | Default                                | Description                                          |
|----------------|----------------------------------------|------------------------------------------------------|
| `php`          | `["8.1", "8.2", "8.3", "8.4", "8.5"]`  | JSON array of PHP versions to test on                |
| `lint-php`     | `8.5`                                  | PHP version for coding standard and dependency check |
| `coverage-php` | *(empty, no coverage)*                 | PHP version to collect and upload coverage on        |

## Versioning

Callers use `@main`, so every push to `main` takes effect in all repositories on their next run. Keep changes to the inputs backward compatible.
