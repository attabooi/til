
---

# Login

There are several steps for having a token.

<br>

![image](https://user-images.githubusercontent.com/99746319/166879949-a09c1f3f-c952-4651-9b1a-de94c4c9587f.png)


<br>

1. <span style="color:rgb(241,196,15)">Inputs ID and PW</span>
2. <span style="color:rgb(181,21,21)">Server transfer PW to hashed PW</span>
3. <span style="color:rgb(171,130,221)">Compare to DB PW</span>
4. <span style="color:rgb(34,202,224)">If correct, giver a token to user</span>

<br>
<br>
<br>

## python code

```python
@app.route('/sign_in', methods=['POST'])
def sign_in():
    # Login
    username_receive = request.form['username_give']
    password_receive = request.form['password_give']  # input ID, PW

    pw_hash = hashlib.sha256(password_receive.encode('utf-8')).hexdigest()  
    # hashing it
    result = db.users.find_one({'username': username_receive, 'password': pw_hash}) 
    # comparing it to a DB data

    if result is not None:  # if correct
        payload = {
            'id': username_receive,
            'exp': datetime.utcnow() + timedelta(seconds=60 * 60 * 24)  
            # token will be expired after 24 hours
        } 
        # make a payload data
        
        token = jwt.encode(payload, SECRET_KEY, algorithm='HS256') 
        # make a token

        return jsonify({'result': 'success', 'token': token}) 
        # givt it to user
        
    # if incorrect
    else:
        return jsonify({'result': 'fail', 'msg': 'Incorrect ID or PW!!'})
```

<br>

## javascript code

<br>
<br>

```javascript

function sign_in() {
    let username = $("#input-username").val()
    let password = $("#input-password").val()

    if (username == "") {
        $("#help-id-login").text("Input ID")
        $("#input-username").focus()
        return;
    } else {
        $("#help-id-login").text("")
    }

    if (password == "") {
        $("#help-password-login").text("Input PW")
        $("#input-password").focus()
        return;
    } else {
        $("#help-password-login").text("")
    }
    $.ajax({
        type: "POST",
        url: "/sign_in",
        data: {
            username_give: username,
            password_give: password
        },
        success: function (response) {
            if (response['result'] == 'success') {
                $.cookie('mytoken', response['token'], {path: '/'});
                window.location.replace("/")
            } else {
                alert(response['msg'])
            }
        }
    });
}

```

-----


# Registration

A logic for a registration is much more simple than Login process.

So I'm gonna move on to code part.

## python code

```python
# registration
@app.route('/sign_up/save', methods=['POST'])
def sign_up():
    username_receive = request.form['username_give']
    password_receive = request.form['password_give']
    nickname_receive = request.form['nickname_give']
    password_hash = hashlib.sha256(password_receive.encode('utf-8')).hexdigest() 
    # transfer PW to a hashed value
    doc = {
        "username": username_receive,  # ID
        "password": password_hash,  # PW
        "nickname": nickname_receive,  # nickname
        "profile_pic": "",  # profile pic
        "profile_pic_real": "profile_pics/profile_placeholder.png",  # dafault profile pic
        "profile_info": ""  # profile bio
    }
    db.users.insert_one(doc) # save ID and hashed PW in my DB
    return jsonify({'result': 'success'})

```

## javascript code

```javascript
function sign_up() {
    let username = $("#input-username").val()
    let password = $("#input-password").val()
    let nickname = $("#input-nickname").val()
    let password2 = $("#input-password2").val()
    console.log(username, nickname, password, password2)
	
  	

    if ($("#help-id").hasClass("is-danger")) {
        alert("Please check your ID.")
        return;
    } else if (!$("#help-id").hasClass("is-success")) {
        alert("Please push the button for checking your ID is unique.")
        return;
    }

    if (password == "") {
        $("#help-password").text("Input your PW.").removeClass("is-safe").addClass("is-danger")
        $("#input-password").focus()
        return;
    } else if (!is_password(password)) {
        $("#help-password").text("PW should contain ----").removeClass("is-safe").addClass("is-danger")
        $("#input-password").focus()
        return
    } else {
        $("#help-password").text("Valid PW!").removeClass("is-danger").addClass("is-success")
    }
    if (password2 == "") {
        $("#help-password2").text("Input your PW.").removeClass("is-safe").addClass("is-danger")
        $("#input-password2").focus()
        return;
    } else if (password2 != password) {
        $("#help-password2").text("Different values between two.").removeClass("is-safe").addClass("is-danger")
        $("#input-password2").focus()
        return;
    } else {
        $("#help-password2").text("Same values").removeClass("is-danger").addClass("is-success")
    }
    $.ajax({
        type: "POST",
        url: "/sign_up/save",
        data: {
            username_give: username,
            password_give: password,
            nickname_give: nickname
        },
        success: function (response) {
            alert("Congrats!!")
            window.location.replace("/login")
        }
    });

}
```