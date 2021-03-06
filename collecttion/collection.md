# 常见js函数收集

#### js获取url中的参数

#### 判断数据类型
```
       //判断数据类型    
       function type(elem){
           var reg = /^\[object\s(.*)\]$/
           var type = Object.prototype.toString.call(elem).match(reg)[1].toLowerCase();
           return type;
       }
```
#### 判断两个对象是否相等
```
   function isObjectValueEqual(a, b) {
      if(typeof a == 'number' && typeof b == 'number'){
          return a == b
      }
      var aProps = Object.getOwnPropertyNames(a);
      var bProps = Object.getOwnPropertyNames(b);

      if (aProps.length != bProps.length) {
          return false;
      }

      for (var i = 0; i < aProps.length; i++) {
          var propName = aProps[i];
          if(Object.prototype.toString(a[propName]) == '[Object Object]'||Object.prototype.toString(b[propName]) == '[Object Object]'){
              isObjectValueEqual(a[propName],b[propName])
          }
          if (a[propName] !== b[propName]) {
              return false;
          }
      }
      return true;
  }
```
#### deep clone
```
   //deepclone with date
    export function deepCloneWithDate(source) {
        if (!source && typeof source !== 'object') {
            throw new Error('error arguments', 'shallowClone');
        }
        const targetObj = source.constructor === Array ? [] : {};
        for (const keys in source) {
            if (source.hasOwnProperty(keys)) {
                if (source[keys] && typeof source[keys] === 'object') {
                    targetObj[keys] = source[keys].constructor === Array ? [] : {};
                    if(source[keys].constructor === Date){
                        targetObj[keys] = new Date(source[keys]);
                        continue;
                    }
                    targetObj[keys] = deepClone(source[keys]);
                } else {
                    targetObj[keys] = source[keys];
                }
            }
        }
        return targetObj;
    }
```
#### 获取一个DIV的绝对坐标的功能函数,即使是非绝对定位,一样能获取到
```
     //获取元素相对于屏幕绝对位置
        function getAbsPosition(element){
           var abs={x:0,y:0}
           //如果浏览器兼容此方法
           if (document.documentElement.getBoundingClientRect){
               //如果不用jQuery对象，可以使用else分支。
               abs.x = element.getBoundingClientRect().left;
               abs.y = element.getBoundingClientRect().top;

               <!-- abs.x += window.screenLeft +
                           document.documentElement.scrollLeft -
                           document.documentElement.clientLeft; -->
               <!--带滚动条的情况-->
               abs.x += window.screenLeft +
                document.body.clientLeft - document.body.scrollLeft;
               abs.y += window.screenTop +
                document.body.clientTop - document.body.scrollTop;
             }
           //如果浏览器不兼容此方法
           else{
               while(element!=document.body){
                   abs.x+=element.offsetLeft;
                   abs.y+=element.offsetTop;
                   element=element.offsetParent;
               }
            //计算想对位置
                abs.x += window.screenLeft +
                       document.body.clientLeft - document.body.scrollLeft;
                abs.y += window.screenTop +
                       document.body.clientTop - document.body.scrollTop;
              }
              return abs;
          }
```
#### 网页拖拽
```
    function drag(obj) {
        var dragEle = obj;
        var _move = false;//移动标记
        var _x, _y;//鼠标离控件左上角的相对位置
        dragEle.click(function () {
            //alert("click");//点击（松开后触发）
        }).mousedown(function (e) {
            _move = true;
            _x = e.pageX - parseInt(dragEle.css("left"));
            _y = e.pageY - parseInt(dragEle.css("top"));
            // dragEle.fadeTo(20, 0.9);//点击后开始拖动并透明显示
        });
        $(document).mousemove(function (e) {
            if (_move) {
                var x = e.pageX - _x;//移动时根据鼠标位置计算控件左上角的绝对位置
                var y = e.pageY - _y;
                dragEle.css({ top: y, left: x });//控件新位置
            }
        }).mouseup(function () {
            _move = false;
            dragEle.fadeTo("fast", 1);//松开鼠标后停止移动并恢复成不透明
        });
    }
```
#### 将#XXXXXX颜色格式转换为RGB格式，并附加上透明度
```
        function brgba(hex, opacity) {
            if( ! /#?\d+/g.test(hex) ) return hex; //如果是“red”格式的颜色值，则不转换。//正则错误，参考后面的PS内容
            var h = hex.charAt(0) == "#" ? hex.substring(1) : hex,
                r = parseInt(h.substring(0,2),16),
                g = parseInt(h.substring(2,4),16),
                b = parseInt(h.substring(4,6),16),
                a = opacity;
            return "rgba(" + r + "," + g + "," + b + "," + a + ")";
        }
```

#### 前端生成UUID：
```
   function uuid() {
        var s = [];
        var hexDigits = "0123456789abcdef";
        for (var i = 0; i < 36; i++) {
            s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
        }
        s[14] = "4";  // bits 12-15 of the time_hi_and_version field to 0010
        s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);  // bits 6-7 of the clock_seq_hi_and_reserved to 01
        s[8] = s[13] = s[18] = s[23] = "-";
    
        var uuid = s.join("");
        return uuid;
    }
```
#### 网页滚动到顶部
```
   function scrollTo(element, to, duration) {
        if (duration <= 0) return;
        const difference = to - element.scrollTop;
        const perTick = difference / duration * 10;
        setTimeout(() => {
            console.log(new Date())
            element.scrollTop = element.scrollTop + perTick;
            if (element.scrollTop === to) return;
            scrollTo(element, to, duration - 10);
        }, 10);
    } 
```