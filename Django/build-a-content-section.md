# How to build a simple content section in Django

## **tweet.urls.py**

<br>

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('tweet/', views.tweet, name='tweet'),    
    path('tweet/delete/<int:id>', views.delete_tweet, name='delete-tweet'),
]
```


## **tweet.views.py**

```python
from django.shortcuts import render, redirect
from .models import TweetModel
from django.contrib.auth.decorators import login_required
# Create your views here.

def home(request):
    user = request.user.is_authenticated  # is user logged in?
    if user:
        return redirect('/tweet')
    else:
        return redirect('/sign-in')

def tweet(request):
    if request.method == 'GET':
        user = request.user.is_authenticated

        if user:
            all_tweet = TweetModel.objects.all().order_by('-created_at') # load recent contents first
            return render(request, 'tweet/home.html', {'tweet':all_tweet})
        else:
            return redirect('sign-in')

    elif request.method == 'POST': # writing a content
        user = request.user
        my_tweet = TweetModel()
        my_tweet.author = user
        my_tweet.content = request.POST.get('my-content','') # read 'my-content' from html id='my-content' which brings texts for the content
        my_tweet.save()
        return redirect('/tweet')

@login_required
def delete_tweet(request,id): # delete a content

    my_tweet = TweetModel.objects.get(id=id)
    my_tweet.delete()
    return redirect('/tweet')
```

## **home.html**
<br>

```html
<!-- templates/tweet/home.html -->

{% extends 'base.html' %}
{% block title %}
    메인페이지
{% endblock %}

{% block content %}
    <div class="container timeline-container">
        <div class="row">
            <!-- 왼쪽 컬럼 -->
            <div class="col-md-3">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title">{{ user.username }}</h5>
                        <p class="card-text"> {{ user.bio }}</p>

                    </div>
                </div>
            </div>
            <!-- 오른 쪽 컬럼-->
            <div class="col-md-7">
                <!-- 글을 작성 하는 곳 -->
                <div class="row mb-2">
                    <div class="col-md-12">
                        <div class="card">
                            <div class="card-body">
                                <div class="media">
                                    <div class="media-body">
                                        <h5 class="mt-0">나의 이야기를 적어주세요</h5>
                                        <p>
                                        <form action="/tweet/" method="post">
                                            {% csrf_token %}
                                            <div class="form-group mb-2">
                                                <textarea class="form-control" style="resize: none" name='my-content'
                                                          id="my-content"></textarea>
                                            </div>
                                            <button type="submit" class="btn btn-primary" style="float:right;">작성하기
                                            </button>
                                        </form>
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <hr>
                <!-- 작성 된 글이 나오는 곳 -->
                <div class="row">
                    {% for tw in tweet %}
                        <div class="col-md-12 mb-2">
                            <div class="card">
                                <div class="card-body">
                                    {% if tw.author == user %}
                                        <div style="text-align: right">
                                            <a href="/tweet/delete/{{ tw.id }}">
                                                <span class="badge rounded-pill bg-danger">삭제</span>
                                            </a>
                                        </div>
                                    {% endif %}
                                    <div style="text-align: right">
                                        <a href="/tweet/{{ tw.id }}">
                                            <span class="badge rounded-pill bg-success">보기</span>
                                        </a>
                                    </div>

                                    <div class="media">
                                        <div class="media-body">
                                            <h5 class="mt-0">{{ tw.content }}</h5>
                                        </div>
                                        <div style="text-align: right">
                                            <span style="font-size: small">{{ tw.author.username }}-{{ tw.created_at|timesince }} 전</span>
                                        </div>

                                    </div>
                                </div>
                            </div>
                        </div>
                    {% endfor %}
                </div>
            </div>
            <div class="col-md-2"></div>
        </div>
    </div>
{% endblock %}
```