# renovate-config

Base Renovate preset for personal repos.

Frontend projects (see [MartinCa/frontend-kit](https://github.com/MartinCa/frontend-kit))
extend this **alongside** the frontend-specific preset, not instead of it —
add both to the project's `extends` array:

```json
{
  "extends": [
    "github>MartinCa/renovate-config",
    "github>MartinCa/frontend-kit:renovate-frontend"
  ]
}
