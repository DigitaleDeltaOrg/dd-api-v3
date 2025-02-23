```python
import requests
from typing import Optional
from datetime import datetime
from urllib.parse import urljoin

class ApiClient:
def __init__(self, base_url: str, api_key: str):
self.base_url = base_url
self.session = requests.Session()
self.session.headers.update({
'Authorization': f'Bearer {api_key}',
'Accept': 'application/json'
})

    def get_locations(self, parameter_type: Optional[str] = None) -> dict:
        try:
            url = urljoin(self.base_url, 'references/locations')
            params = {'$select': 'id,name,coordinates'}
            
            if parameter_type:
                params['$filter'] = f"type eq '{parameter_type}'"

            response = self.session.get(url, params=params)
            response.raise_for_status()
            
            return response.json()
            
        except requests.exceptions.RequestException as e:
            print(f"Error fetching locations: {e}")
            raise

# Gebruik:
client = ApiClient('https://api.dd.nld/v3/', 'jouw-api-key')
try:
locations = client.get_locations(parameter_type='waterHeight')
for location in locations['value']:
print(f"Location: {location['name']}")
except Exception as e:
print(f"Failed to fetch locations: {e}")
```
