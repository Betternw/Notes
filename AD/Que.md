1. 什么是ANR 如何避免它？
   * 在Android上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应 用程序无响应（ANR：Application NotResponding）对话框。
   * 不同的组件发生ANR的时间不一样，Activity是5秒，BroadCastReceiver是10秒，Service是20秒（均为前台）
   * 主线程中存在耗时的计算、BroadcastReceiver未在10秒内完成相关的处理、Service在特定的时间内无法处理完成 20秒
   * 将所有耗时操作，比如访问网络，Socket通信，查询大 量SQL 语句，复杂逻辑计算等都放在子线程中去，使用AsyncTask、handler.sendMessage等方式更新UI
2. Activity和Fragment生命周期有哪些？https://camo.githubusercontent.com/85eb508cd8e5d6077f3a9f0fe9e513182e2d8474/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f323839333133372d643633353337373033313933613664312e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f
3. 横竖屏切换时候Activity的生命周期
   * 不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。
   * 设置Activity的android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次
   * 设置Activity的android:configChanges=”orientation |keyboardHidden”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法