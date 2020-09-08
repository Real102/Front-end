# 换一种方式实现下拉刷新（IntersectionObserver）

先上代码

```html
<div class="container">
    <ul class="oul">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ul>
</div>
<div class="sign">loading···</div>
```

```css
* {
    margin: 0;
    padding: 0;
}
.container {
    width: 100%;
    height: auto;
    border-bottom: 1px solid coral;
}
.container ul li {
    height: 100px;
    width: 100%;
    list-style: none;
    text-align: center;
    line-height: 100px;
}
.sign {
    height: 30px;
    line-height: 30px;
    font-size-adjust: 20px;
    background: #ffffff;
    color: #000000;
    text-align: center;
}
```

```javascript
window.onload = function () {
    let index = 11
    let num = 5
    let sign = document.getElementsByClassName("sign")[0]
    let oul = document.getElementsByClassName("oul")[0]
    oli = oul.getElementsByTagName("li")    // 这里生成的是维数组，非真实数组
    Array.from(oli).forEach((i) => {    // 转换成数组方可使用forEach，或者直接使用for...of
        let c1 = Math.random(0, 1) * 255
        let c2 = Math.random(0, 1) * 255
        let c3 = Math.random(0, 1) * 255
        i.style.background = `rgb(${c1}, ${c2}, ${c3})`
    })
    if (window.IntersectionObserver) {  // 判断是否支持 IntersectionObserver API
        console.log("支持 IntersectionObserver API")
        let observer = new IntersectionObserver((entries) => {
            if (entries[0].isIntersecting) {
                console.log("i see you")
                setTimeout(function () {
                    let df = document.createDocumentFragment()
                    for (let i = 0; i < num; i++) {
                        let otext = document.createTextNode(index++)
                        let oli = document.createElement("li")

                        let c1 = Math.random(0, 1) * 255
                        let c2 = Math.random(0, 1) * 255
                        let c3 = Math.random(0, 1) * 255
                        oli.style.background = `rgb(${c1}, ${c2}, ${c3})`
                        oli.appendChild(otext)
                        df.appendChild(oli)
                    }
                    oul.appendChild(df)
                }, 2000)
            }
        })
        observer.observe(sign)
    } else {
        console.log("不支持 IntersectionObserver API")
    }
}
```

关键点：[IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)（MDN 文档）

> IntersectionObserver 接口（从属于 Intersection Observer API）提供了一种异步观察目标元素与其祖先元素或顶级文档视窗(viewport)交叉状态的方法
