# Preprocessing



### 📌Null

<br>

```python
print(df.isnull().sum()) # check a number of null data

print(len(df))

df = df.dropna() # drop null data

print(len(df))
```


<br><br><br>

### 📌Dependent and Independent Data


<br>

![image](https://user-images.githubusercontent.com/99746319/168439764-9c2926d9-cb55-464e-a291-14a58e2b276a.png)


<br><br><br>

### 📌Header

<br>

![image](https://user-images.githubusercontent.com/99746319/168439858-24b66818-fc0e-4eeb-ab63-f26d7917c0a6.png)

<br><br><br>

### 📌Standardization

<br>

```python
scaler = StandardScaler()
x_data_scaled = scaler.fit_transform(x_data)

print(x_data.values[0])
print(x_data_scaled[0])
```

