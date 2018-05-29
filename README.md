# ScratchCard

A scratch card plug-in for HTML5 that can be used on PC or mobile.
一个可以用于PC端和移动端的HTML5刮刮卡插件。

### 刮刮卡插件使用说明

本插件不做UI,需要自己实现html、css部分，当然这部分极其简单。
本插件基于canvas实现，你需要自己定义canvas的css相关属性 position、width、height
例如：
div.box 作为父元素，奖品内容可以写在里面。
canvas#canvas作为刮奖遮罩，需要定位覆盖到 div.box上面。

#### html部分
```
<div class="box">
    <div class="boxin"></div>
    <canvas id="canvas" width="600" height="200"></canvas>
</div>
```
#### css部分
```
.box{ position: relative; width: 600px; height: 200px; border: 1px solid ;}
#canvas{ position: absolute; left: 0; top: 0; width: 100%; height: 100%; }
```


#### 调用方法
```
var scratchCard = new ScratchCard({

        id : '#canvas'  ,   //canvas 的选择器(可以是class，但最终只会选择1个)
        
        maskColor : '#ccc' ,    //canvas底色，（如果遮罩图片是jpg可以不写）
        
        maskImage   : './img/01.jpg',   //遮罩图片路径（推荐使用jpg，防止出现透明部分）(如果只需要纯色，可以只设置maskColor)
        
        //画完遮罩后，可以执行其他内容，如：从后台取数据、在遮罩上画其他内容
        maskBack : function(){
            var _this = scratchCard;    //使用实例scratchCard来调用各种方法
            console.log('去取数据。。。');
        },    
           
        openPercent : 0.6,  //刮开多少后全部显示 范围：0< x < 1，默认0.6
        
        //每次刮动后的回调
        progressBack : function(progress){
            console.log('刮开进度：',progress);
        },
        //完全刮开后的回调
        endBack : function(){
           console.log('刮奖完成。。。')
        }, 
           
        //endHide : true,   //画完后 canvas 是否会 display:none; 默认true

        //dpi : 1,    //设备dpi，默认会自动根据设备取出，会决定canvas真实大小(高清倍率)，也可强制设置。如果要在canvas上操作，坐标应该乘上dpi。

}) ;
```

#### 实例可用属性 
```
canvas :  canvas dom对象
paint :  画笔对象（ctx)
canvasWidth :  canvas修改后的 width 属性值，等于 canvas.width
canvasHeight :  canvas修改后的 height 属性值，等于 canvas.height
dpi :  设备dpi（高清倍率）
```
#### 可用api
```
reset : 完全重置刮刮卡
pause : 暂停使用刮刮卡，调用后无法擦除
continue : 恢复使用刮刮卡，调用后可用继续擦除
```
#### 封装的canvas功能,都需要用 ScratchCard实例为this来调用
```
paintImage (src,x,y,w,h,callback,dpi)
    //画一张图片。 注意：这是一个异步操作
    @src : 图片路径
    @x,y,w,h : 画布坐标x,y,图片宽高w,h
    @callback : 图片画完之后的回调
    @dpi : 默认会取设备dpi对参数进行修正，如果不需要，需设置为1
    return : 该图片dom对象
```

```
paintText (conf,dpi)
    //画文字
    @conf : {
        text : '',          // * 文本
        fillStyle : '',     // * 填充样式 二选一
        strokeStyle : '',   // * 描边样式 二选一
        x : 10,             // *文字坐标x
        y : 10,             // *文字坐标y
        
        font : '',          //合写的font属性，与css相同（顺序font-style font-variant font-weight font-size/line-height font-famil）
        textAlign : '',     //水平对齐方式
        textBaseline : '',  //基线
    }
    
    @dpi : 默认会取设备dpi对参数进行修正，如果不需要，需设置为1
```

```
paintLine (x1,y1,x2,y2,color,width,lineCap,dpi)
    @ x1,y1 第一个点的坐标
    @ x2,y2 第二个点的坐标
    @ color 线的颜色 默认 #000
    @ width 线宽 默认 1
    @ lineCap 线条末端线帽样式
    @dpi : 默认会取设备dpi对参数进行修正，如果不需要，需设置为1
```














