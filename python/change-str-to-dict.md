# Change str to dict

When we load json file from api server. We might need change str to dict. 

Here is the way I did for making data set. If you know a better way, it will be appreciate if you let me know.

<br>


```python
import ast

def str_to_dict(x):
    if type(x) == str and (x[0] == '{' or x[0] '['):
        return as.literal_eval(x) # use literal_eval() for changing text to json
    else:
        return x

itemId = ['{"rows":[{"itemId":"59b756d84bf9478eaca58bed8f74762f","itemName":"데저트 컨실멘트 숄더","itemRarity":"에픽","itemType":"방어구","itemTypeDetail":"천 머리어깨","itemAvailableLevel":105}]}']

# to extract itemName

item_Name = [] 
for i in itemId:      # extract the value in list one by one (ie. itemId[0])
    j = str_to_dict(i)  # let the value change to dict
    for k in j['rows']:   # extract the values in 'rows' from j
        item_Name.append(k['itemName']) # extract only itemName 
print(item_Name)

```