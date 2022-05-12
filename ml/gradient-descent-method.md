# GD(Gradient Descent Method)

## **What is GD?**

<br>

![image](https://user-images.githubusercontent.com/99746319/168099836-b8662909-d050-4160-bf96-5f7dfeba75da.png)

<br><br>

GD is an optimisation algorithm used to find a minimum point of loss function. 

<br><br><br><br>

![oLMCrxb](https://user-images.githubusercontent.com/99746319/168101444-6dd4150c-9640-4627-bead-5b2d63e6fbda.gif)

<br><br>
It starts from a randomly decided point, and moves step by step:`Learning Rate`.

The process continues until it reaches at the minimum.

<br><br><br><br>

---

## **What is SGD?**

![image](https://user-images.githubusercontent.com/99746319/168099871-cc9ac1f6-1fc5-4171-b018-719f6f82a3cb.png)

<br><br>

GD uses the full data for each step, so it costs lots of time.
 For solving this problem, SGD uses separated data to reduce the time. 
 However it has lower accuracy than GD

<br><br><br><br>

![image](https://user-images.githubusercontent.com/99746319/168099888-b1499606-2a32-4730-9d36-7c8435e76ec0.png)


<br><br>



### GD

- Include full-data for calculation
- Find a optimisation step
- Slow training

### SGD

- Not include full-data for calculation
- Low accuracy but fast training

<br><br>

![image](https://user-images.githubusercontent.com/99746319/168099909-052ca3ff-d6a9-4e74-9ee5-f4f8b3a05eb4.png)

<br><br>

The graph shows the importance of proper Learning Rate of SGD.

<br><br><br><br><br>

---

## **Linear Regression with *Keras***
<br>

```python
import os
os.environ['KAGGLE_USERNAME'] = 'username' # username
os.environ['KAGGLE_KEY'] = 'key' # key
!kaggle datasets download -d rsadiq/salary # download datasets from kaggle
!unzip salary.zip # unzip


# library
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.optimizers import Adam, SGD
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns
from sklearn.model_selection import train_test_split

df = pd.read_csv('Salary.csv')

df.tail(5)


# linear regrssion
x_data = np.array(df['YearsExperience'], dtype=np.float32)
y_data = np.array(df['Salary'], dtype=np.float32)

x_data = x_data.reshape((-1, 1))
y_data = y_data.reshape((-1, 1))

print(x_data.shape)
print(y_data.shape)

x_train, x_val, y_train, y_val = train_test_split(x_data, y_data, test_size=0.2, random_state=2021) 
# 

print(x_train.shape, x_val.shape)
print(y_train.shape, y_val.shape)

model = Sequential([
  Dense(1)
])

model.compile(loss='mean_squared_error', optimizer=SGD(lr=0.01))

model.fit(
    x_train,
    y_train,
    validation_data=(x_val, y_val), 
    epochs=100 

```