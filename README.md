### CSS布局 and 技巧
###### position: absolute;
```css
    element {
        position: absolute;
        left: 0;
        right: 0;
    }
    /**
        left: 0;
        right: 0;
        将其左右满屏拉伸，对应效果为 width：100%；
    */
```

##### 常见的CSS布局

 ###### 两列布局（左右布局）
 1、float：
 ```css
    .layout {
        height: 100%;
        .left {
            float: left;
            width: 100px;
            height: 100%;
        }
        .right {
            margin-left: 100%;
            height: 100%;
        }
    }
 ```
2、float：（overflow：hidden 形成BFC）
```css
    .layout {
        height: 100%;
        .left {
            float: left;
            width: 100px;
            height: 100%;
        }
        .right {
            overflow: hidden;
            height: 100%;
        }
    }  
```
3、fix：（推荐）
```css
    .layout {
        height: 100%;
        display: flex;
        .left {
            width: 100px;
        }
        .right {
            flex: 1;
        }
    }
```

###### 三列布局（左中右布局）
1、float + overflow：
```css
    .layout {
        height: 100%;
        .left {
            float: left;
            width: 100px;
        }
        .center {
            float: left;
            width: 100px;
        }
        .right {
            overflow: hidden;
        }
    }
```
2、flex：(推荐)
```css
    .layout {
        height: 100%;
        display: flex;
        .left {
            width: 100px;
        }
        .center {
            width: 100px;
        }
        .right {
            flex: 1;
        }
    }
```

###### 圣杯布局 and 双飞翼布局
1、圣杯布局 float + margin-left/right + padding-left/right
```html
    <div class="layout">
        <div class="left"></div>
        <div class="right"></div>
        <div class="center"></div>
    </div>
    <!-- 注释：由于浮动节点在位置上不能高于前面或平级的非浮动节点，否则会导致浮动节点下沉-->
```
```css
    .layout {
        height: 100%;
        padding: 0 100px;
        .letf {
            width: 100px;
            float: left;
            margin-left: -100px;
        }
        .center{
            /*  */
        }
        .right {
            width: 100px;
            float: right;
            margin-right: -100px;
        }
    }
```
2、双飞翼布局float + margin-left/right
```html
    <div class="layout">
        <div class="left"></div>
        <div class="right"></div>
        <div class="center">
            <div></div>
        </div>
    </div>
    <!-- 注释：由于浮动节点在位置上不能高于前面或平级的非浮动节点，否则会导致浮动节点下沉-->
```
```css
    .layout {
        height: 100%;
        .left {
            float: left;
            width: 100px;
        }
        .right {
            float: right;
            width: 100px;
        }
        .center {
            height: 100%;
            margin: 0 100px;
        }
    }
```
3、圣杯||双飞翼 flex布局（推荐）
```html
    <div class="layout">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
    </div>
```
```css
    .layout {
        display: flex;
        height: 100%;
        .left {
            width: 100px;
        }
        .center {
            flex: 1;
        }
        .right {
            widht: 100px;
        }
    }
```

##### CSS 变量（存在严重的兼容问题）
例子：
```html
    <ul class="strip-loading">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
```
```css
    .strip-loading {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        li {
            border-radius: 3px;
            width: 6px;
            height: 30px;
            background-color: #f66;
            animation: beat 1s ease-in-out infinite;
            & + li {
                margin-left: 5px;
            }
            &:nth-child(2) {
                animation-delay: 200ms;
            }
            &:nth-child(3) {
                animation-delay: 400ms;
            }
            &:nth-child(4) {
                animation-delay: 600ms;
            }
            &:nth-child(5) {
                animation-delay: 800ms;
            }
            &:nth-child(6) {
                animation-delay: 1s;
            }
        }
    }
    @keyframes beat {
        0%,
        100% {
            transform: scaleY(1);
        }
        50% {
            transform: scaleY(.5);
        }
    }
```
使用css变量后
```html
    <ul class="strip-loading">
        <li style="--line-index: 1;"></li>
        <li style="--line-index: 2;"></li>
        <li style="--line-index: 3;"></li>
        <li style="--line-index: 4;"></li>
        <li style="--line-index: 5;"></li>
        <li style="--line-index: 6;"></li>
    </ul>
```
```css
    .strip-loading {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        li {
            --time: calc((var(--line-index) - 1) * 200ms);
            border-radius: 3px;
            width: 6px;
            height: 30px;
            background-color: #f66;
            animation: beat 1.5s ease-in-out var(--time) infinite;
            & + li {
                margin-left: 5px;
            }
        }
    }
    @keyframes beat {
        0%,
        100% {
            transform: scaleY(1);
        }
        50% {
            transform: scaleY(.5);
        }
    }
```

总结：
- Css 变量开头前缀为 --;
- Css 变量在子集（element）非常多的情况，影响性能；
- CSS 变量存在 全局 和 父集 作用域；:root {} 代表全局作用域；
