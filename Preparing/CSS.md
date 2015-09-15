# 2、2015-05-24
* 设置字体大小

      font-size: 20%;
      font-size: 20px;
      font-size: 1.5em;

* 文本对齐
      text-align: left;

* 简单的左右布局，使div充满屏幕
      style.css:

      html, body {
          margin-top: 0;
          margin-left: 0;
          font-family: Helvetica, Arial, sans-serif;
          height: 100%;
          width: 100%;
          padding: 0;
          margin: 0;
      }

      .center {
          width: 100%;
          height: 100%;
          float: left;
          //border: 1px solid #000;
      }

      .center-left {
          width: 15%;
          height: 95%;
          float: left;
      //    border:1px solid #000;
          padding: 20px;
      }

      .center-separator {
          width: 0.5%;
          height: 100%;
          float: left;
          border-left: 1px solid #000;
          border-right: 1px solid #000;
      }

      .center-right {
          width: 45.6%;
          height: 95%;
          float: left;
      //    border: 1px solid #000;
          padding: 10px;
      //    margin-left: 10px;
      }

      index.html:

      <div id="center" class="center">
          <div id="menu-div" class="center-left">
            <div id="monitor-index" class="menu-left"><a onclick="getLiveChannels()">監控首頁</a></div>
            <div class="menu-left">事件檢視/查詢</div>
            <div class="menu-left">監控畫面設定</div>
          </div>

          <div id="separator" class="center-separator"></div>

          <div id="content-div" class="center-right">
            <div class="title-right">監控首頁</div>
            <div id="live-channel-list-div" class="">
            </div>
          </div>
      </div>

# 1、2015-04-13
* [css控制div显示／隐藏方法及２种方法比较](http://www.cnblogs.com/ndxsdhy/archive/2011/04/12/2013472.html)
