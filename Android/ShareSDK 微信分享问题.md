##ShareSDK 微信分享的一些问题

*  首先在微信开放平台注册时包名要一致，app打包的签名要一致（MD5的那一串）然后在ShareSDK.xml 文件中填入微信平台相应的appID、appSecret;
*  根据[不同平台的分享内容详细说明](http://wiki.mob.com/%E4%B8%8D%E5%90%8C%E5%B9%B3%E5%8F%B0%E5%88%86%E4%BA%AB%E5%86%85%E5%AE%B9%E7%9A%84%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E/#h1-3) 在ShareParams 中设置相应的字段。
*  分享时遇到[error=-6]()的错误 一般是签名不一致的问题。如果签名一致，就在手机设置中将微信的缓存清掉，或者直接重启设备。