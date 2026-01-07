# Contributing to the ACP Registry

## Adding a New Agent or Extension

1. **Fork this repository**

2. **Create a directory for your entry**

   ```
   mkdir <id>/
   ```

   The directory name must match your entry's `id` field.

3. **Create `agent.json` or `extension.json`**

   Use `agent.json` for agents, `extension.json` for extensions. Both use the same schema.

   ```json
   {
     "id": "your-agent-id",
     "name": "Your Agent Name",
     "version": "1.0.0",
     "description": "Brief description of your agent",
     "repository": "https://github.com/your-org/your-repo",
     "authors": ["Your Name <email@example.com>"],
     "license": "MIT",
     "distribution": {
       // At least one distribution method required
     }
   }
   ```

4. **Add an icon** (optional but recommended)

   Place an SVG icon at `<agent-id>/icon.svg`.

   **Icon requirements:**
   - **16x16 SVG** - larger icons may be scaled down and lose quality
   - **Monochrome using `currentColor`** - enables theme support (light/dark mode)

   Example:
   ```svg
   <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16">
     <path fill="currentColor" d="M..."/>
   </svg>
   ```

   > **Warning**: Icons with hardcoded colors (`fill="#FF0000"`, `fill="red"`, etc.) will fail validation. Use `fill="currentColor"` or `fill="none"` only.

5. **Submit a Pull Request**

   The CI will validate your `agent.json` against the schema.

## Distribution Options

### Binary Distribution

For standalone executables:

```json
{
  "distribution": {
    "binary": {
      "darwin-aarch64": {
        "archive": "https://github.com/.../release.zip",
        "cmd": "./your-binary",
        "args": ["acp"],
        "env": {
          "OPTIONAL_VAR": "value"
        }
      }
    }
  }
}
```

Supported platforms: `darwin-aarch64`, `darwin-x86_64`, `linux-aarch64`, `linux-x86_64`, `windows-aarch64`, `windows-x86_64`

### npm Package (npx)

```json
{
  "distribution": {
    "npx": {
      "package": "@your-scope/your-package@1.0.0",
      "args": ["--acp"]
    }
  }
}
```

### PyPI Package (uvx)

```json
{
  "distribution": {
    "uvx": {
      "package": "your-package",
      "args": ["serve", "--acp"]
    }
  }
}
```

## Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier (lowercase, hyphens allowed) |
| `name` | string | Display name |
| `version` | string | Semantic version |
| `description` | string | Brief description |
| `distribution` | object | At least one distribution method |

## Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `repository` | string | Source code URL |
| `authors` | array | List of author names/emails |
| `license` | string | SPDX license identifier |

## Updating an Existing Entry

To update your agent or extension's version or distribution URLs:

1. Fork and update the `agent.json` or `extension.json` file
2. Submit a Pull Request
3. CI will validate and merge will trigger a new registry release

## Validation

All submissions are validated against the [JSON Schema](agent.schema.json). Additionally, **all distribution URLs must be accessible** - the CI validates that:

- Binary archive URLs return HTTP 200
- npm packages exist on registry.npmjs.org
- PyPI packages exist on pypi.org

Run validation locally:

```bash
uv run --with jsonschema .github/workflows/build_registry.py
```
