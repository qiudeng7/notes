# vscode tricks
## allow json comment
open settings.json of vscode:
```json
{
  // other settings...
  // jsonc means json with comment
  "files.associations": {
        "*.json": "jsonc"
    },
}
```