# Architecture

faneX-ID Core manages the lifecycle of all extensions.

## Plugin Manager

The Plugin Manager scans the `plugins/` directory (and configured Repositories) for valid components.

### Manifest Structure

Every addon must have a `manifest.json`:

```json
{
  "schema_version": "1.0.0",
  "domain": "my_addon",
  "version": "1.0.0",
  "logo": "https://...",
  "implementations": {
      "python": "integration.py"
  }
}
```
