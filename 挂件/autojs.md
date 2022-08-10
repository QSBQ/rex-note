# autojs

launchApp("QQ");==>自动打开软件

启动按键监听：

```
//启动按键监听
events.observeKey();
//监听音量上键按下
events.onKeyDown("volume_up",function(event){
    toast("音量上键被按下了!")
})
```

