
<template>
  <div>
    <!-- 阿里云视频播放器样式 -->
    <link
      rel="stylesheet"
      href="https://g.alicdn.com/de/prismplayer/2.8.1/skins/default/aliplayer-min.css" 
    />
    <script src="https://g.alicdn.com/de/prismplayer/2.8.0/hls/aliplayer-vod-anti-min.js" />
    <script
      type="text/javascript"
      charset="utf-8"
      src="https://player.alicdn.com/aliplayer/presentation/js/aliplayercomponents.min.js"
    />
    <script
      type="text/javascript"
      charset="utf-8"
      src="https://g.alicdn.com/de/prismplayer/2.9.3/aliplayer-min.js"
    ></script>
    <!-- 定义播放器dom -->
    <div id="J_prismPlayer" class="prism-player" />
  </div>
</template>

<script>
import vod from "@/api/vod";
export default {
  layout: "video", //应用video布局
  asyncData({ params, error }) {
    return vod.getPlayAuth(params.vid).then((response) => {
      console.log(response.data.data);
      return {
        vid: params.vid,
        playAuth: response.data.data.playAuth,
      };
    });
  },
  mounted() {
    /* 弹幕组件集成了 CommentCoreLibrary 框架, 更多 Api 请查看文档 https://github.com/jabbany/CommentCoreLibrary/ */
    var danmukuList = [
      {
        mode: 1,
        text: "哈哈",
        stime: 1000,
        size: 25,
        color: 0xffff00,
      },
      {
        mode: 1,
        text: "前方高能",
        stime: 2000,
        size: 25,
        color: 0xff00ff,
      },
      {
        mode: 1,
        text: "灵魂歌手",
        stime: 30000,
        size: 25,
        color: 0x00ff00,
      },
      {
        mode: 1,
        text: "这是弹幕2",
        stime: 4000,
        size: 25,
        color: 0x00c1de,
      },
      {
        mode: 1,
        text: "神测试",
        stime: 5000,
        size: 25,
        color: 0x0000ff,
      },
      {
        mode: 1,
        text: "顺手一划",
        stime: 10000,
        size: 25,
        color: 0x00c1de,
      },
      {
        mode: 1,
        text: "哈哈",
        stime: 1000,
        size: 25,
        color: 0xffffff,
      },
      {
        mode: 1,
        text: "哈哈",
        stime: 1000,
        size: 25,
        color: 0xffffff,
      },
    ];
    var adCloseFunction = function (videoAd) {
      /* Register the pause event to pause the ad. */
      videoAd.pauseVideoAd();
      var result = confirm("Become a VIP and skip the ad?");
      if (result) {
        /* Register the skip event to skip the ad. */
        videoAd.closeVideoAd();
      } else {
        /* Register the play event to play the ad. */
        videoAd.playVideoAd();
      }
    };
    /* eslint-disable no-undef */
    const player =  new Aliplayer(
      {
        id: "J_prismPlayer",
        vid: this.vid, // 视频id
        playauth: this.playAuth, // 播放凭证
        encryptType: "1", // 如果播放加密视频，则需设置encryptType=1，非加密视频无需设置此项
        width: "100%",
        height: "500px",
        cover:
          "http://video.yaohuan.work/image/default/DF3E77AA7D894DA5975068C5F5201D63-6-2.jpg", // 封面
        qualitySort: "asc", // 清晰度排序
        mediaType: "video", // 返回音频还是视频
        autoplay: false, // 自动播放
        isLive: false, // 直播
        rePlay: false, // 循环播放
        preload: true,
        controlBarVisibility: "hover", // 控制条的显示方式：鼠标悬停
        useH5Prism: true, // 播放器类型：html5
        components: [
            {
       name: 'BulletScreenComponent',
       type: AliPlayerComponent.BulletScreenComponent,
       /** 跑马灯组件三个参数 text, style, bulletPosition
    * text: 跑马灯文字内容
    * style: 跑马灯样式
    * bulletPosition: 跑马灯位置, 可选的值为 'top' (顶部), 'bottom' (底部), 'random' (随机), 不传值默认为 'random'
    */
       args: ['guoch，欢迎使用阿里播放器', { fontSize: '16px', color: '#00c1de' }, 'random']

        },
          {
            name: "VideoADComponent",
            type: AliPlayerComponent.VideoADComponent,
            /* The video ad component has these parameters: adVideoSource, adLink, adCloseFunction, and closeText
             * adVideoSource {@String Required. The URL of the video ad. }
             * adLink {@String Required. The link of the ad page.}
             * adCloseFunction {@Function Optional. The click event handler for skipping the ad. The default action is skip ad.}
             * closeText {@String Optional. The text of the skip ad button. The default is 'Skip Ad'.}
             */
            args: [
              "https://player.alicdn.com/ad/citybrain.mp4",
              "https://et.aliyun.com/brain/city",
              adCloseFunction,
              "Skip Ad",
            ],
          },
          {
            name: "AliplayerDanmuComponent",
            type: AliPlayerComponent.AliplayerDanmuComponent,
            args: [danmukuList],
          },
        ],
      /* h5截图按钮 */
   'extraInfo': {
     'crossOrigin': 'anonymous'
   },
   'skinLayout': [
     { 'name': 'bigPlayButton', 'align': 'blabs', 'x': 30, 'y': 80 },
     { 'name': 'H5Loading', 'align': 'cc' },
     { 'name': 'errorDisplay', 'align': 'tlabs', 'x': 0, 'y': 0 },
     { 'name': 'infoDisplay' },
     { 'name': 'tooltip', 'align': 'blabs', 'x': 0, 'y': 56 },
     { 'name': 'thumbnail' },
     {
       'name': 'controlBar', 'align': 'blabs', 'x': 0, 'y': 0,
       'children': [
         { 'name': 'progress', 'align': 'blabs', 'x': 0, 'y': 44 },
         { 'name': 'playButton', 'align': 'tl', 'x': 15, 'y': 12 },
         { 'name': 'timeDisplay', 'align': 'tl', 'x': 10, 'y': 7 },
         { 'name': 'fullScreenButton', 'align': 'tr', 'x': 10, 'y': 12 },
         { 'name': 'subtitle', 'align': 'tr', 'x': 15, 'y': 12 },
         { 'name': 'setting', 'align': 'tr', 'x': 15, 'y': 12 },
         { 'name': 'volume', 'align': 'tr', 'x': 5, 'y': 10 },
         { 'name': 'snapshot', 'align': 'tr', 'x': 10, 'y': 12 }
       ]
     }

      ]
      },
      function (player) {
        console.log("播放器创建成功");
      }
    );
    player.on('snapshoted', function(data) {
   var pictureData = data.paramData.base64
   var downloadElement = document.createElement('a')
   downloadElement.setAttribute('href', pictureData)
   var fileName = 'Aliplayer' + Date.now() + '.png'
   downloadElement.setAttribute('download', fileName)
   downloadElement.click()
   pictureData = null

    })
  },
};
</script>