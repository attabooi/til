# error: ![rejected] main -> main (non-fast-forward)

### **Cause of the error**

It simply occured because there is no README.me file in your local, but it is in your repository.

So the error can be fixed by making a README.md file in your local.


Use Git Bash to fix it.

<br>
<br>

### **How to fix**

```
$ git pull origin main

$ git add .

$ git commit -m ""

$ git push origin main
```


<br>


### How to fix **(not recommended)**

```
$ git init

$ git remote add origin https://~

$ git push origin +main
```

This will do a `forced push`; it will overwrite the repository. So you are gonna  loss your README.md.
