## 使用Jquery实现的一个回到顶部的功能
```html
<div id="go_top_btn" style="position: fixed;right: 1.8rem; bottom: 4.3rem;width: 2.6rem;height: 2.6rem;border-radius: 2rem;background-color: #fff;box-shadow: 0px 0px 10px 0px #cc5b5b;display: none;">
    <img src="__PUBLIC__/v2/img/go_top.png?v=4886" alt="回到顶部" style="width: 100%;height: 100%;">
</div>
```

```javascript
        // 该部分主要用于，让回到顶部的按钮在滚动条向下滑动一定距离后在用动画慢慢显示出回到顶部的按钮
        $(window).scroll(function(){
            if ($(window).scrollTop() > 100) {
                $("#go_top_btn").fadeIn(800);
            }
            else {
                $("#go_top_btn").fadeOut(800);
            }
        })

        // 点击回到顶部的具体功能实现
        $("#go_top_btn").on('click',function(){
            $('html , body').animate({scrollTop: 0},'slow');
        })
```

