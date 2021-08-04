### 北邮疫情自动填报（Python+Github Actions）长期维护
## 特点：

- 每天1：10和7：10自动填报（可通过main.yml修改）
- 运行失败会自动往邮箱（注册github所用邮箱）发送邮件
- 可以多人同时填报（可以帮小伙伴一起打卡哦）
- 填报情况可以使用上一次打卡数据，也可以使用固定数据（地点始终位于北邮）
- **（可选）**填报结果通过Server酱推送至微信

![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628064209023-bbdceebb-87e7-4ca6-ac57-547da961c1e3.png#clientId=u261c98e9-d654-4&from=paste&height=371&id=u10561721&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1516&originWidth=1077&originalType=binary&ratio=1&size=453600&status=done&style=none&taskId=u6303bc11-f11e-4a59-b778-21c40c6d2ef&width=263.5)
​

## 使用详解

1. 点击右上角的 **Use this template  **![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628065022255-80e778fc-97d3-4936-bfd1-874b4324d990.png#clientId=u261c98e9-d654-4&from=paste&height=51&id=ufcacae7d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=82&originWidth=496&originalType=binary&ratio=1&size=7087&status=done&style=none&taskId=ue31904dd-37e3-41d2-b06b-98456b599b3&width=308) 然后给仓库随便起一个名字，点击 **Create repository from template**
1. 点击 **Settings **，进入 **Secrets** 页面，点击右上角的 **New repository secret**，流程如下图所示

![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628065535109-af374281-31ec-4244-8c77-f6dfc2b6c779.png#clientId=u261c98e9-d654-4&from=paste&height=433&id=u822bfcbe&margin=%5Bobject%20Object%5D&name=image.png&originHeight=675&originWidth=1264&originalType=binary&ratio=1&size=82055&status=done&style=none&taskId=u8ea35f0e-45a6-47fd-82c2-a04317c9a4e&width=811)

3. 一共有**两**个secret，第一个Name填**USERS**，Value按照如下格式填写：
```python
[
    (学号:str,密码:str,用户名:str,0 or 1),
    (学号:str,密码:str,用户名:str,0 or 1),
    (学号:str,密码:str,用户名:str,0 or 1),
    ... 如果还有则继续往后面加
]
```
相当于**列表**里面有很多**元组**，每个元组代表一个用户，可以有任意多个。每个元组有**四个元素**，前两个分别为**学号**和**密码**，字符串格式（可自行通过 [https://app.bupt.edu.cn/ncov/wap/default/index](https://app.bupt.edu.cn/ncov/wap/default/index) 登陆验证账号密码正确性，密码一般为身份证后8位），第三个为**用户名**（随便填，用于控制台与Server显示），第四个为是否用上一次打卡数据，**0或者1**，0代表使用上一次打卡数据（某一次自己在脚本运行前打卡之后都采用这次打卡数据），1代表使用固定数据（固定数据的地点始终位于北邮），以下是一个样例：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628066635601-48a9812f-4c82-4a50-9b1c-dcbd09943de7.png#clientId=u261c98e9-d654-4&from=paste&height=247&id=ub0a210e5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=311&originWidth=386&originalType=binary&ratio=1&size=14325&status=done&style=none&taskId=u51450016-6564-45a1-aa6c-81cc0109419&width=307)

4. 第二个secret的Name填写**SERVER_KEY**，如果不配置Server酱微信推送，那么Values里填写**0**即可，如果想配置的话看下一点
4. **（可选）**Values填写Server酱的SendKey（在这里查看 [https://sct.ftqq.com/sendkey](https://sct.ftqq.com/sendkey)），在此之前需要微信注册企业号，并加入Server酱内部应用，具体流程见 [https://sct.ftqq.com/forward](https://sct.ftqq.com/forward)，看起来比较多，但也不是很麻烦，一步步照做即可
4. 最后Actions secrets效果：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628067489437-8e6c6d8a-c398-4f16-ba88-88f90e42156a.png#clientId=u261c98e9-d654-4&from=paste&height=407&id=u716694af&margin=%5Bobject%20Object%5D&name=image.png&originHeight=591&originWidth=935&originalType=binary&ratio=1&size=49124&status=done&style=none&taskId=u2a2581e4-777a-4b7b-be9f-57e034c7cd1&width=643.5)

7. 点击上方**Actions**按钮，![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628067577883-6fbb1950-88fe-494b-b38f-da7438740d99.png#clientId=u261c98e9-d654-4&from=paste&height=38&id=u0f26024e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=71&originWidth=934&originalType=binary&ratio=1&size=8530&status=done&style=none&taskId=ud68c9423-d399-41f2-93ac-54097fea5b3&width=505)，点击一个workflow，![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628067659877-d1990ae6-6031-4028-afb0-3b38e7a184fc.png#clientId=u261c98e9-d654-4&from=paste&height=204&id=uaf407981&margin=%5Bobject%20Object%5D&name=image.png&originHeight=381&originWidth=1282&originalType=binary&ratio=1&size=38893&status=done&style=none&taskId=u59c7092a-1f67-468f-a197-7a145466229&width=685)点击右上方**Re-run jobs**，开始重新执行，![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628067788251-2ba71911-b151-480a-86a9-c3cb25428753.png#clientId=u261c98e9-d654-4&from=paste&height=134&id=u6fe50fa6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=267&originWidth=413&originalType=binary&ratio=1&size=8528&status=done&style=none&taskId=ud3041661-eef0-4178-adb1-5fa058e434b&width=206.5)点进去查看执行情况
7. 如果准确按照上述步骤执行，你应该会看到类似的如下输出：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/567404/1628067954640-601edcb5-dd94-451a-b5f1-d05ede59958f.png#clientId=u261c98e9-d654-4&from=paste&height=538&id=u45e21369&margin=%5Bobject%20Object%5D&name=image.png&originHeight=868&originWidth=1311&originalType=binary&ratio=1&size=139021&status=done&style=none&taskId=u7b6cdc09-5f62-433f-b696-7dc29d6443d&width=812.5)
​

**恭喜你，你还有你的小伙伴不用为导员催促打卡而烦恼了~**
**​**

## 参数更改：
### 更改每日打卡时间
在 .github/workflows/main.yml 中来设置每天运行的时间：
```python
on:
  schedule:
    - cron: "10 1,7 * * *"
```
cron里的"10 1,7 * * *"代表每天的1：10和7点10，[https://crontab.guru/#10_1,7_*_*_*](https://crontab.guru/#10_1,7_*_*_*) 用这个网站来选取你想要的时间
### 更改打卡的固定数据
在 [https://app.bupt.edu.cn/ncov/wap/default/index](https://app.bupt.edu.cn/ncov/wap/default/index) 进行填报，全部填完后最后不要提交，f12打开控制台，在Console页面下输入代码vm.info回车得到填报数据，替换掉 constant.py 里的INFO变量
​

​

