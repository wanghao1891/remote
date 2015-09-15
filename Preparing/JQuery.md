# 3、2015-05-10
## 3.2、使用Chrome的调试工具来选取需要的颜色值
鼠标点击HTML元素，然后右键选择“审查元素”，在调试窗口的右侧部分的“Styles”中可以看到颜色值，而且可以在上边修改，很方便。

## 3.1、使用JQuery来设置DIV的背景颜色
    var name = data.channel;
    var status = data.status;

    var color = "#2CB853";

    if(status === "Bad") {
        color = "#B82C41";
    }

    $("#" + name).css("background-color", color);

# 2、2015-04-01
* 控制元素显隐  
$("#add-live-channel-dialog").toggle();
* 显示元素  
$("#add-live-channel-dialog").show();
* 隐藏元素  
$("#add-live-channel-dialog").hide();

# 1、2015-03-30
* [jQuery - wikipedia](http://zh.wikipedia.org/wiki/JQuery)

* [How jQuery Works](http://learn.jquery.com/about-jquery/how-jquery-works/)
