# Logistic Regression


## 📌Binary Logistic Regression

<br>

`Run it in colab`

<br>

```python
import os
os.environ['KAGGLE_USERNAME'] = 'attabooi' # username
os.environ['KAGGLE_KEY'] = '' # key

!kaggle datasets download -d heptapod/titanic
!unzip titanic.zip

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.optimizers import Adam, SGD
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('train_and_test2.csv')
df = pd.read_csv('train_and_test2.csv', usecols=[
  'Age', 
  'Fare', 
  'Sex', 
  'sibsp', 
  'Parch', 
  'Pclass',
  'Embarked', 
  '2urvived' 
])

sns.countplot(x='Sex', hue='2urvived', data=df) # survivors according to sex

sns.countplot(x=df['2urvived']) 

print(df.isnull().sum())
df = df.dropna()

# preprocessing
x_data = df.drop(columns=['2urvived'], axis=1)
x_data = x_data.astype(np.float32)

y_data = df[['2urvived']]
y_data = y_data.astype(np.float32)

## Standardization
scaler = StandardScaler()
x_data_scaled = scaler.fit_transform(x_data)
print(x_data.values[0])
print(x_data_scaled[0])

## Split data to train and validation set
x_train, x_val, y_train, y_val = train_test_split(x_data, y_data, test_size=0.2, random_state=2021)
print(x_train.shape, x_val.shape)
print(y_train.shape, y_val.shape)

## Machine Learning
model = Sequential([
  Dense(1, activation='sigmoid')
])

model.compile(loss='binary_crossentropy', optimizer=Adam(lr=0.01), metrics=['acc'])

model.fit(
    x_train,
    y_train,
    validation_data=(x_val, y_val), 
    epochs=20 

## Predict data
y_pred = model.predict([[5]])
print(y_pred)
```

<br><br><br>

## 📌Multinomial Logistic Regression

<br>

`Run it in colab`

<br>

```python
import os
os.environ['KAGGLE_USERNAME'] = 'attabooi' # username
os.environ['KAGGLE_KEY'] = '' # key

!kaggle datasets download -d brynja/wineuci
!unzip wineuci.zip

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.optimizers import Adam, SGD
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('Wine.csv')
df = pd.read_csv('Wine.csv', names=[
  'name'
  ,'alcohol'
  ,'malicAcid'
  ,'ash'
  ,'ashalcalinity'
  ,'magnesium'
  ,'totalPhenols'
  ,'flavanoids'
  ,'nonFlavanoidPhenols'
  ,'proanthocyanins'
  ,'colorIntensity'
  ,'hue'
  ,'od280_od315'
  ,'proline'
])

# preprocessing
x_data = df.drop(columns=['name'], axis=1)
x_data = x_data.astype(np.float32)
y_data = df[['name']]
y_data = y_data.astype(np.float32)
scaler = StandardScaler()
x_data_scaled = scaler.fit_transform(x_data)

## one-hot encoding
encoder = OneHotEncoder()
y_data_encoded = encoder.fit_transform(y_data).toarray()
print(y_data.values[0])
print(y_data_encoded[0])

x_train, x_val, y_train, y_val = train_test_split(x_data_scaled, y_data_encoded, test_size=0.2, random_state=2021)

# maching learning
model = Sequential([
  Dense(3, activation='softmax') # softmax
])


model.compile(loss='categorical_crossentropy', 
optimizer=Adam(lr=0.02), metrics=['acc'])
# categorical crossentropy


model.fit(
    x_train,
    y_train,
    validation_data=(x_val, y_val), 
    epochs=20 
)
```