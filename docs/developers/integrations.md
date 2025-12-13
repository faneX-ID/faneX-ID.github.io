# Building Integrations

## Step-by-Step

1.  **Create Directory**: Make a new folder in your custom repository.
2.  **Create Manifest**: Define metadata in `manifest.json`.
3.  **Implement Logic**:
    - **Python**: Create `integration.py` inheriting from `Integration` base class.
    - **PowerShell**: Create `integration.ps1` script.

## Example Python Integration

```python
from services.integration_base import Integration
from services.service_registry import service_registry

class MyIntegration(Integration):
    async def async_setup(self):
        service_registry.register(self.domain, "hello", self.hello)
        return True

    async def hello(self, data):
        return {"response": "World"}
```
