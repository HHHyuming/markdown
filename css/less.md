## 变量

```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}

编译后
#header {
  width: 10px;
  height: 20px;
}
```

## 混合

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}


// 使用方法
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```

## 嵌套

```css
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
# use ========================
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

