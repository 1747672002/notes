## 问题：使用jquery动态加载的HTML元素，使用动态的方式绑定事件后，在IOS无法生效的问题

示例：
```html
<html>
  <head>
    <title>IOS问题</title>
  </head>
  <body>
    <div id="box"></div>
  </body>
</html>
```
```javascript
  $(document).ready(function(){
  
      var arr = {
        {id : 1},
        {id : 2},
        {id : 3}
      };
      
      // 动态添加元素节点
      var html = '';
      for(var i=0; i<arr.length; i++){
        html += '<span class="span_clas">'+arr[i]['id']+'<span><br/>'
      }
      
      // 给动态添加的元素绑定事件
      $('body').delegate(".span_class",'click',function(){
        alert($(this).text());
      });
  
  });

```

上面的代码在`浏览器`或`安卓`端是可以正常运行的，但是如果在Apple的`IOS`上就无法生效点击了

## 原因
造成该问题的原因是，IOS的端`document`、`div`、`body`这些并没有可被点击的属性的元素，不能作为托管点击事件的父元素，所以使用这种方法进行事件托管，`IOS`会获取不到你的`body`的点击事件，也就无法获取到想要绑定的元素了

## 解决方式
很简单，在我们要动态添加的元素上，添加一个`CSS属性`就可以了：`cursor:pointer;`
示例：
```javascript
...
      // 动态添加元素节点
      var html = '';
      for(var i=0; i<arr.length; i++){
        html += '<span style="cursor:pointer;" class="span_clas">'+arr[i]['id']+'<span><br/>'
      }
...

```
这样，就可以在`IOS`上正常的实现我们的需求了

