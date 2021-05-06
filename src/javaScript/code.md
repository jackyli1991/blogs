# 一些常用代码

## 动态加载js文件，并在加载完成后执行回调callback

```javascript
/**
* 动态加载js文件，并在加载完成后执行回调callback
* @params { String } url js文件地址
* @params { Function } callback js加载完毕的回调函数
*/
function loadScript（url, callback） {
    let script = document.createElement('script')
    script.type = 'text/javascript'
    script.src = url
    document.body.appendChild('script')
    if (script.readyState) { // IE
        script.onreadystatechange = function () {
            if (script.readyState === 'loaded' || script.readyState === 'complete') {
                script.onreadystatechange = null
                callback()
            }
        }
    } else { // 其他浏览器
        script.onload = function () {
            callback()
        }
    }
}
```

