# Encode to URL

Use urllib to encode text to URL

```python
import urllib
from urllib import parse

itemName = '타락한 영혼'

itemUrl = parse.quote(itemName)
print(itemUrl)

# result: %ED%83%80%EB%9D%BD%ED%95%9C%20%EC%98%81%ED%98%BC
```
