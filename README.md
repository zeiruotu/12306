### 12306

- python版本支持
  - 2.7
- 依赖库
  - 依赖打码兔 需要去打码兔注册（用户）账号，打码兔账号地址：http://www.dama2.com，一般充值1元就够用了，充值打码兔之后，首次运行是需要到官网黑白名单授权
  - 依赖若快 若快注册地址：http://www.ruokuai.com/client/index?6726 推荐用若快，打码兔在12306验证码更新之后识别率不是很高
  - 项目依赖包 requirements.txt
  - 安装方法 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt

- 项目使用说明
  - 需要配置邮箱，可以配置可以不配置，配置邮箱的格式在yaml里面可以看到ex
  - 提交订单验证码哪里依赖打码兔，所以如果是订票遇到验证码的时候，没有打码兔是过不了的，不推荐手动，手动太慢
  - 配置yaml文件的时候，需注意空格和遵循yaml语法格式，项目的yaml配置ex：
      - ticket_config.yaml 配置说明
        ```
        #station_date:出发日期改为多日期查询，格式ex：
                        - "2018-02-03"
                        - "2018-02-04"
                        - "2018-02-05"
        #station_trains:过滤车次，格式ex：
                        #    - "G1353"
                        #    - "G1329"
                        #    - "G1355"
                        #    - "G1303"
                        #    - "G1357"
                        #    - "G1305"
                        #    - "G1359"
                        #    - "G1361"
                        #    - "G1373"
                        #    - "G1363"
        #from_station: 始发站
        #to_station: 到达站
        #set_type: 坐席(商务座,二等座,特等座,软卧,硬卧,硬座,无座)
        #is_more_ticket:余票不足是否自动提交
        #select_refresh_interval:抢票刷新间隔时间，1为一秒，0.1为100毫秒，以此类推 如果捡漏推荐为1秒，刷票设置0.01
        #expect_refresh_interval:售票未开始，等待刷新间隔时间，1为一秒，0.1为100毫秒，以此类推
        #ticket_black_list:加入小黑屋的等待时间，默认3 min    小黑屋的功能是上次买票失败，证明此票已无机会，下次刷新看到此票跳过
        #enable_proxy:是否开启代理模式，代理速度比较慢，如果是抢票阶段，不建议开启
        #ticke_peoples: 乘客 ex: "张三"
        #damatu：打码兔账号，用于自动登录和订单自动打码
        #is_aotu_code是否自动打码，如果选择Ture,则调用打码兔打码，默认不使用打码兔
        #is_email: 是否需要邮件通知 ex: True or False 切记，邮箱加完一定到config目录下测试emailConf功能是否正常

        #邮箱配置 列举163
        #  email: "xxx@163.com"
        #  notice_email_list: "123@qq.com"
        #  username: "xxxxx"
        #  password: "xxxxx
        #  host: "smtp.163.com"
        #邮箱配置 列举qq  ，qq设置比较复杂，需要在邮箱--账户--开启smtp服务，取得授权码==邮箱登录密码
        #  email: "xxx@qq.com"
        #  notice_email_list: "123@qq.com"
        #  username: "xxxxx"
        #  password: "授权码"
        #  host: "smtp.qq.com"
        ```

- 项目开始
  - 修改config/ticket_config.yaml文件，按照提示更改自己想要的信息
  - 运行根目录run.py，即可开始

- 目录对应说明
  - agency - cdn代理
  - config - 项目配置
  - damatuCode - 打码兔接口
  - init - 项目主运行目录
  - myException - 异常
  - myUrllib - urllib库

- 思路图
     ![image](https://github.com/testerSunshine/12306/blob/master/uml/uml.png)

- 项目声明：
  - 本软件只供学习交流使用，务作为商业用途，交流群：286271084
  - 能为你抢到一张回家的票，是我最大的心愿

- 成功log，如果是购票失败的，请带上失败的log给我，我尽力帮你挑，也可加群一起交流，程序只是加速买票的过程，并不一定能买到票
    ```
    正在第355次查询  乘车日期: 2018-02-12  车次G4741,G2365,G1371,G1377,G1329 查询无票  代理设置 无  总耗时429ms
    车次: G4741 始发车站: 上海 终点站: 邵阳 二等座:有
    正在尝试提交订票...
    尝试提交订单...
    出票成功
    排队成功, 当前余票还剩余: 359 张
    正在使用自动识别验证码功能
    验证码通过,正在提交订单
    提交订单成功！
    排队等待时间预计还剩 -12 ms
    排队等待时间预计还剩 -6 ms
    排队等待时间预计还剩 -7 ms
    排队等待时间预计还剩 -4 ms
    排队等待时间预计还剩 -4 ms
    恭喜您订票成功，订单号为：EB52743573, 请立即打开浏览器登录12306，访问‘未完成订单’，在30分钟内完成支付！
    ```

- 2017.5.13跟新
    - 增加登陆错误判断（密码错误&ip校验）
    - 修改queryOrderWaitTime，校验orderId字段bug，校验msg字段bug，校验messagesbug
    - 修改checkQueueOrder  校验 data 字段的列表推导式bug
    - 增加代理ip方法，目前已可以过滤有用ip


- 2018.1.7 号更新
    - 增加自动配置
        ```
        #station_date:出发日期，格式ex：2018-01-06
        #from_station: 始发站
        #to_station: 到达站
        #set_type: 坐席(商务座,二等座,特等座,软卧,硬卧,硬座,无座)
        #is_more_ticket:余票不足是否自动提交
        #select_refresh_interval:刷新间隔时间，1为一秒，0.1为100毫秒，以此类推
        #ticke_peoples: 乘客
        #damatu：打码图账号，用于自动登录
        ```
    - 优化订票流程
    - 支持自动刷票，自动订票

- 2018.1.8 更新
    - 增加小黑屋功能
    - 修复bug若干
    - 增加多账号同时订票功能
    -  增加按照选定车次筛选购买车次

- 2018.1.9 更新

    - 增加手动打码，只是登录接口，完全不用担心提交票的效率问题，要挂linux系统的话，还是去注册个打码兔吧
    ```
    思路
    1.调用PIL显示图片
    2.图片位置说明，验证码图片中每个图片代表一个下标，依次类推，1，2，3，4，5，6，7，8
    3.控制台输入对应下标，按照英文逗号分开，即可手动完成打码，
    ```
    - 修改无座和硬座的座位号提交是个字符串的问题
    - 增加校验下单需要验证码功能
    - 增强下单成功判断接口校验

- 2018.1.10 更新
    - 优化查票流程
    - 修改二等座的余票数返回为字符串的问题
    - 优化订单查询bug
- 2018.1.12更新
    - 优化抢票页面逻辑
    -增强代码稳定性

- 2018.1.13更新
    - 修改下单验证码功能
    - 优化大量调用user接口导致联系人不能用，理论加快订票速度
    - 增加邮箱功能
    ```
    #is_email: 是否需要邮件通知 ex: True or False 切记，邮箱加完一定要到config目录下测试emailConf功能是否正常
    #email: 发送的邮箱地址 ex: 1@qq.com
    #notice_email_list: 被通知人邮箱 ex: 2@qq.com
    #username: 邮箱账号
    #password: 邮箱密码
    #host: 邮箱地址
    ```

- 2018.1.14更新
    - 优化订票流程
    - 优化挂机功能
        - 修改之前程序到11点自动跳出功能，现在修改为到早上7点自动开启刷票
        - 需要开启打码兔代码功能，is_aotu_code 设置为True
    - 增加异常判断

- 2018.1.15更新
    - 增加捡漏自动检测是否登录功能，建议捡漏不要刷新太快，2S最好，否则会封IP
    - 优化提交订单有很大记录无限排队的情况，感谢群里的小伙伴提供的思路
    - 修改休眠时间为早上6点

- 2018.1.20更新，好久没跟新了，群里的小伙伴说登录不行了，今晚抽空改了一版登录，妥妥的
    - 更新新版登录功能，经测试，更稳定有高效
    - 优化手动打码功能
    - 更新请求第三方库
    - 优化若干代码，小伙伴尽情的放肆起来

- 2018.1.21跟新
    - 修复若干bug
    - 合并dev
    - 恢复之前因为12306改版引起的订票功能
    - 增加派对失败自动取消订单功能
    - 优化接口请求规范
    - 增加多日期查询，请严格按照yaml格式添加 即可
        - 注意：如果多日期查询的话，可能查询时间会比较长
    - 增加如果排队时间超过一分钟，自动取消订单

- 2018.1.23更新
    - 增加若快平台打码，yaml新增字段aotu_code_type，1=打码兔，2=若快 若快注册地址：http://www.ruokuai.com/client/index?6726
    - 修改is_aotu_code字段为全部是否自动打码字段，也就是说字段为rue，则全部自动打码，为False全部手动打码，包括提交订单，注意centOs不可设置手动打码
    - 修复bug
    - 优化抢票功能

- 2018.1.25更新
    - 删除 expect_refresh_interval 无用字段，优化代码


