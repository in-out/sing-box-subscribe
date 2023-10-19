# sing-box-subscribe

[中文](https://github.com/Toperlock/sing-box-subscribe/blob/main/README.md)|[EN](https://github.com/Toperlock/sing-box-subscribe/blob/main/instructions/README.md)

根据配置模板生成 sing-box 使用的 `config.json`，主要用于将机场订阅节点添加到 config，对使用 `clash_mode` 的配置才有意义。

不适合完全不了解 sing-box 配置文件的人使用，最少要知道什么是出站、dns server、dns规则、路由规则。最好了解 clash 的分组方式。

请查看：[https://sing-box.sagernet.org/zh/configuration](https://sing-box.sagernet.org/zh/configuration)。

## 导航

[操作演示视频](https://github.com/Toperlock/sing-box-subscribe#-%E5%8A%9F%E8%83%BD%E6%BC%94%E7%A4%BA%E8%A7%86%E9%A2%91)

[参数填写含义](https://github.com/Toperlock/sing-box-subscribe#providersjson%E6%96%87%E4%BB%B6)

[模板内容详解](https://github.com/Toperlock/sing-box-subscribe#config%E6%A8%A1%E6%9D%BF%E6%96%87%E4%BB%B6)

[Windows使用](https://github.com/Toperlock/sing-box-subscribe#windows-sing-box-%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)

## 支持协议

|  协议 | V2格式 | Clash格式 | URI格式 | SingBox格式 |
|  :----  | :----: | :----: | :----: | :----: |
| http  | ✅ | ✅ | ✅ | ✅ |
| socks5  | ✅ | ✅ | ✅ | ✅ |
| shadowsocks  | ✅ | ✅ | ✅ | ✅ |
| shadowsocksR  | ✅ | ✅ | ✅ | singbox默认不支持此协议 |
| vmess  | ✅ | ✅ | ✅ | ✅ |
| trojan  | ✅ | ✅ | ✅ | ✅ |
| vless  | ✅ | ✅ | ✅ | ✅ |
| tuic  | ✅ | ✅ | ✅ | ✅ |
| hysteria  | ✅ | ✅ | ✅ | ✅ |
| hysteria2  | ✅ | ✅ | ✅ | ✅ |
| wireguard  | ✅ | ✅ | ✅ | ✅ |

~不支持转换 clash 订阅的解析~ 暂时只写了以上打勾协议的**分享链接**的解析（**v2订阅格式/clash订阅格式**），因为自己用的机场只有这几个协议。添加新的协议解析有能力可以自己写，比如 `vless.py`（文件名称必须为协议名称），写好后将其放入到 parsers 目录即可，`vless.py` 中必须包含 `parse` 函数。

**脚本为自用，本人使用 [yacd](https://yacd.metacubex.one) (ios请用http://yacd.metacubex.one) 进行节点切换管理（类型为urltest、selector的出站），配合规则像clash一样分流，非常方便。需求跟我一样的可以尝试，使用脚本过程中有新的功能需求或者出现任何错误请提出 issue，不要骚扰 sing-box。**

**脚本可以用vercel服务器部署在网页运行，也可以下载项目源码在本地运行。请使用自己部署的网站生成sing-box配置。**

# 一、服务器部署

## 开始使用

1. 点击此项目右上角的 fork 按钮，fork 本项目到自己仓库；
2. 点击右侧按钮开始部署：
   [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new)，直接使用 Github 账号登录即可；[请查看详细教程](./docs/vercel-cn.md#如何新建项目)。
3. 部署完毕后，即可开始使用；
4. （可选）[绑定自定义域名](https://vercel.com/docs/concepts/projects/domains/add-a-domain)：Vercel 分配的域名 DNS 在某些区域被污染了，绑定自定义域名即可直连。

### 打开自动更新

> 如果你遇到了 Upstream Sync 执行错误，请手动 Sync Fork 一次！

当你 fork 项目之后，由于 Github 的限制，需要手动去你 fork 后的项目的 Actions 页面启用 Workflows，并启用 Upstream Sync Action，启用之后即可开启每小时定时自动更新：

![自动更新](https://github.com/Toperlock/ChatGPT-Next-Web/raw/main/docs/images/enable-actions.jpg)

![启用自动更新](https://github.com/Toperlock/ChatGPT-Next-Web/raw/main/docs/images/enable-actions-sync.jpg)

### 手动更新代码

如果你想让手动立即更新，可以查看 [Github 的文档](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) 了解如何让 fork 的项目与上游代码同步。

你可以 star/watch 本项目或者 follow 作者来及时获得新功能更新通知。

## 页面操作步骤

[示例网站](https://sing-box-subscribe.vercel.app/)。打开自己部署的网站，编辑右侧`编辑服务器 TEMP_JSON_DATA`方框内容，点击`保存`，左上角选择配置模板，点击`生成配置文件`。👉🏻[参数填写查看](https://github.com/Toperlock/sing-box-subscribe#providersjson%E6%96%87%E4%BB%B6)

ios配合快捷指令复制网页内容，或者内容太多选择下载文稿后自行解决文稿后缀问题。👉🏻[快捷指令安装](https://www.icloud.com/shortcuts/75fd371e0aa8438a89f715238a21ee68)

Android使用chrome浏览器打开网页生成配置文件（请在浏览器 设置-无障碍 缩小网页），长按内容，全选，分享到代码编辑器里，检查编辑器是否显示内容完整。👉🏻[编辑器安装](https://mt2.cn/download/)

**注意点击保存后，尽快去生成配置文件，不然你填的内容会一直保留在网页上，别人打开网站也可以浏览到。目前想不到解决办法**

<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/95a79758-245b-4806-a483-b2993db7e62e" alt="how-to-use" width="50%" />
</div>

## 🎬 功能演示视频

<div align="center">
   
**网页解析通用订阅链接(v2/clash/sing-box)**

https://github.com/Toperlock/sing-box-subscribe/assets/86833913/9b3c006d-d554-435b-99c9-b28d4ccaad74

**网页批量解析URI**

https://github.com/Toperlock/sing-box-subscribe/assets/86833913/88b0fa0e-b732-4018-8003-21f1a65586a9

**安卓谷歌浏览器页面缩小**

https://github.com/Toperlock/sing-box-subscribe/assets/86833913/f534503c-ed3f-4d67-8302-d498cc3fc805

**本地解析通用订阅链接(v2/clash/sing-box)**

https://github.com/Toperlock/sing-box-subscribe/assets/86833913/1249bb6a-54e4-44ef-9eb2-6057108bc337

**本地批量解析URI**

https://github.com/Toperlock/sing-box-subscribe/assets/86833913/f88b392c-ea81-4460-b8af-00fe879affb0

</div>

# 二、本地安装
### PC安装3.10及以上的[python](https://www.python.org/)版本，注意安装步骤里把python添加到系统环境变量（google安装步骤）

<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/f387322b-a602-40df-b3b6-95561329f2f8" alt="install" width="60%" />
</div>

### 在终端输入下面指令安装依赖（Mac把pip改为pip3）：

```
pip install requests paramiko scp chardet Flask PyYAML ruamel.yaml
```

<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/0fc03b49-4c57-4ef3-a4fc-044c1a108d75" alt="install" width="60%" />
</div>

### 下载这个 `sing-box-subscribe` 项目，打开终端进入当前项目路径（可以直接在文件路径输入`cmd`）

<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/73f05ba8-105c-4f10-8e6c-16e27f26c084" alt="run" width="60%" />
</div>

### 把订阅链接放到 `providers.json` ，编辑好 `config_template_groups_tun.json` 模板使用下面的命令运行脚本：

```
python main.py
```

### 使用过程中提示python没有模块就安装对应的模块，例如下面提示就输入指令（Mac把pip改为pip3）：

```
pip install chardet
```
<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/1762db84-23f5-4cbd-a9d1-df3ca253396c" alt="install" width="60%" />
</div>

windows系统建议将命令添加到批处理程序运行。

使用前先编辑 `providers.json` 文件以及 config_template 目录下的 `.json` 模板文件。

已内置懒人 `config_template_groups_tun` 文件，请在模板里修改筛选节点
* 实现 `Openai` 分流
* 实现 `Google` 分流
* 实现 `Telegram` 分流
* 实现 `Twitter` 分流
* 实现 `Facebook` 分流
* 实现 `Amazon` 分流
* 实现 `Apple` 分流
* 实现 `Microsoft` 分流
* 实现 `Game` 分流
* 实现 `Bilibili` 分流
* 实现 `Youtube` 分流
* 实现 `Netflix` 分流
* 实现 `Hbo` 分流
* 实现 `Disney` 分流
* 实现 `Prime Video` 分流

# providers.json文件
在这个文件中添加订阅链接，以及基础设置。
```json
{
    "subscribes":[
        {
            "url": "订阅地址",
            "tag": "机场1",
            "enabled": true,
            "emoji": 1,
            "prefix": ""
        },
        {
            "url": "订阅地址",
            "tag": "机场2",
            "enabled": false,
            "emoji": 0,
            "prefix": "❤️机场前缀 - "
        }
    ],
    "auto_set_outbounds_dns":{
        "proxy": "",
        "direct": ""
    },
    "save_config_path": "./config.json",
    "auto_backup": false,
    "exlude_protocol": "ssr",
    "User-Agent":"clash",
    "Only-nodes": false
}
```
- `url`：必须。

> 支持机场普通的v2订阅链接（**内容为base64编码**）

> 支持机场clash订阅链接

> 支持机场sing-box订阅链接

> 本地文件路径（**内容为URI的链接**）
       
      本地文件比如txt文件，需要在文件中每行一个添加单节点分享链接，比如 `ss://` 开头（非订阅链接）。

      本地文件需要保存到相同盘符，本地路径格式： `/Desktop/sing-box-subscribe/xx.txt` 或者是与 `main.py` 相同文件夹里相对路径格式： `./xx.txt`。

- `tag`：必须。

> config 模板里填上此处写的tag才能添加此订阅。此处的 `"机场1"` 对应 config 模板中的 `"{机场1}"` 具体使用方法可以查看下方的 config 模板部分。

<details>
      <summary>tag截图参考</summary>
      
<div align="left">
<img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/b8673073-7160-429f-9ced-3eae7925036e" alt="download" width="65%" />
</div>

</details>
      
- `enabled`：非必需。**将其设置为 false 时，此订阅会被忽略**。

- `emoji`：非必需。**将其设置为 false 或 0 时，节点名称不会添加国旗emoji**。

- `prefix`：非必需。设置自定义前缀，前缀会添加到对应节点名称前。如果没有设置，则不添加前缀。

<details>
      <summary>prefix效果参考</summary>
      
![Snipaste_2023-05-02_12-53-27](https://user-images.githubusercontent.com/21310130/235582317-6bb3d0a6-916f-445f-999b-f17b3db41eea.png)

</details>

- `auto_set_outbounds_dns`：非必需。
> 包含 `proxy` 和 `direct` 设置项。

> `proxy` 和 `direct` 应该设置为 config 模板文件中存在的 `dns server` 的 `tag`。

> 设置此项后，脚本会自动适配 路由规则 到 dns 规则。

> 将路由规则中 `direct` 出站 的 `dns server` 设置为选项中指定的 `direct` 出站。

> 将路由规则中需要代理的 出站 设置为对应的 `proxy` 出站，脚本会自动创建对应出站的 `dns server`，以 `proxy` 设置项指定的 `dns server` 为模板。
 
- `save_config_path`：必需。设置生成的配置文件路径。
 
- `auto_backup`：非必需。
> 设置为 true 时，脚本会将当前使用的sing-box配置文件更名为 `原文件名称.当前时间.bak` 进行备份，避免生成错误的配置文件后无法挽回。
 
- `exlude_protocol`：非必需。
> 设置不解析的协议，多个使用英文逗号分隔，比如ssr,vmess。

> 使用此设置中的协议的分享链接会被忽略。

> sing-box release中的程序没有支持ssr（需要自己添加参数构建），所以此设置可能有用。

- `User-Agent`：非必需。
> 可以自定义UA，比如设置UA为"clash.meta"，或者"sing-box"

- `Only-nodes`：非必需。
> 将其设置为 true 或 1 时，只输出订阅链接 sing-box 格式的节点信息

# config模板文件
脚本会在 config_template 目录下查找 json 模板文件，脚本运行时可以选择使用的模板文件。

比如目录下存在 `tun.json` 和 `socks.json` 两个模板文件。

![Snipaste_2023-03-24_22-16-49](https://user-images.githubusercontent.com/21310130/227548643-ffbf3825-9304-4df7-9b65-82a935227aef.png)

脚本不会检查模板文件的正确性，模板文件不正确会出现错误并无法运行脚本。目录下自带有模板，根据需要修改。

模板文件基本等同于 sing-box config，不过有一些新的参数，比如 `{all}`、`{机场tag}`、`filter`，所有参数仅在 `clash_mode` 的出站方式下才会生效，出站类型为 `urltest`、`selector`。
```json
{
  "tag":"proxy",
  "type":"selector",
  "outbounds":[
    "auto",
    "{all}"//所有订阅所有节点添加到此标记所在位置
  ],
  "filter":[
    //此条过滤将会删除 机场1 中包含 ˣ² 的节点
    {"action":"exlude","keywords":["ˣ²"],"for":["机场1"]}
  ]
},
{
  "tag":"netflix",
  "type":"selector",
  "outbounds":[
    "{机场1}",//订阅tag为 机场1 的节点将添加到此标记所在位置
    "{机场2}"//订阅tag为 机场2 的节点将添加到此标记所在位置
  ],
  "filter":[
    //如果机场1，机场2有节点 sg、新加坡、tw、台湾，他们共同组成 netflix 组
    {"action":"include","keywords":["sg","新加坡","tw","台湾"]},
    //for里面设置为机场1，代表此条规则只对机场1起作用
    {"action":"exlude","keywords":["ˣ²"],"for":["机场1"]}
    //执行完第二个规则后 netflix 组将机场1 中包含 ˣ² 的节点删掉
  ]
}
```
- `{all}`：表示所有订阅中的所有节点。脚本会将所有节点添加到有此标识的 `outbounds` 中。

- `{机场tag}`：在 `providers.json` 中设置的机场 `tag` 可以用于此处，代表此订阅中的所有节点。

- `filter`：非必需。节点过滤，为一个数组对象，可以添加任意条规则，格式为:
```json
"filter":[
    {"action":"include","keywords":["保留关键字1","保留关键字2"]},
    {"action":"exlude","keywords":["排除关键字1","排除关键字2"],"for":["机场1tag","机场2tag"]}
  ]
```
- **关键字大小写敏感**

- `include`：后面添加要保留的关键字，名称中包含这些关键字的节点都将被保留，其他节点会被删除。

- `exlude`：后面添加要排除的关键字，名称中包含这些关键字的节点都将被删除，其他节点会被保留。

- `for`：非必需。设置机场 `tag`，可以多个，表示此规则只对指定的机场起作用，其他机场会忽略这个规则。

多个规则会按顺序执行过滤。

# Windows sing-box 使用方法

1. 下载Windows客户端程序[sing-box-windows-amd64.zip](https://github.com/SagerNet/sing-box/releases)。
2. 新建一个 `.bat` 批处理文件，内容为 `start /min sing-box.exe run`。
3. 参考[客户端配置](https://github.com/chika0801/sing-box-examples/blob/main/Tun/config_client_windows.json)示例，按需修改后将文件名改为 **config.json**，与 **sing-box.exe**，批处理文件放在同一文件夹里。
4. 右键点击 **sing-box.exe** 选择属性，选择兼容性，选择以管理员身份运行此程序，确定。
5. 运行批处理文件，在弹出的用户账户控制对话框中，选择是。

## 隐藏Windows运行sing-box弹出的cmd窗口

> 使用WinSW把sing-box.exe设置成Windows服务，[WinSW教程](https://blog.xuven.xyz/post/WinSW/)

> XML配置文件修改
```xml
<service>
  <id>sing-box</id>
  <name>sing-box</name>
  <description>sing-box Service</description>
  <executable>./sing-box.exe</executable>
  <log mode="reset"></log>
  <arguments>run</arguments>
</service>
```
<details>
      <summary>Windows sing-box 文件夹内容</summary>
 
<div align="left">
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/c6a815bf-b542-43c6-aeb6-84020586a1f1" alt="download" width="50%" />
</div>

</details>

<details>
      <summary><b>效果参考</b></summary>

具体效果根据个人的出站及规则设置决定。

<div align="left">
  <img src="https://user-images.githubusercontent.com/21310130/227577941-01c80cfc-1cd9-4f95-a709-f5442a2a2058.png" alt="download" width="50%" />
  <img src="https://user-images.githubusercontent.com/21310130/227577968-6747c7aa-db61-4f6c-b7cc-e3802e34cc3d.png" alt="download" width="50%" />
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/955968d7-98e7-4bd2-a582-02576877dba1" alt="download" width="50%" />
  <img src="https://github.com/Toperlock/sing-box-subscribe/assets/86833913/9e7c35ff-c6c4-46c4-a74b-624ff72c17ea" alt="download" width="50%" />
</div>

</details>

# 感谢
- [一佬](https://github.com/xream)
- [sing-box](https://github.com/SagerNet/sing-box)
- [yacd](https://github.com/haishanh/yacd)
- [clash](https://github.com/Dreamacro/clash)
- [sing-box-examples@chika0801](https://github.com/chika0801/sing-box-examples)

部分协议解析参考了[convert2clash](https://github.com/waited33/convert2clash)

部分clash2v2ray参考了[clash2base64](https://github.com/yuanyiwei/toys/blob/master/DEPRECATED/clash/clash2base64.py)

同步代码参考了[ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web)

感谢@SayRad的越南翻译
