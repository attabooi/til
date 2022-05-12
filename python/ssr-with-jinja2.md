# Server-side-rendering with jinja2

<br>

This work is about building a profile pages on web based on local files. 

However, I think the logic I used from this work is worth writing down on TIL for beginners like me.

Using jinja2 make us write much less code than when we do it with ajax.

<br>
<br>


![image](https://user-images.githubusercontent.com/99746319/167431192-6fdd5773-19c7-49df-bba3-eb95917d950e.png)


1. Take `user_id` from `index.html` hyperlink info 
2. Make `contents` variable from all contents in DB which has `user_id`
3. Make `content` variable for sending the info to `profile.page.html`
4. Do `for loop` and load all img info 


