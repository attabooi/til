# Encode CSV Files

<br>

Korean should encoded in UTF-8 form to read in python. 

And there is a way to encode and read csv files.

<br>


```python
import pandas as pd

df = pd.read_csv('item_list.csv', encoding = 'cp929')
```

