# HomeProxy 最新版本，全新配置文件,节点类型修改为`URLTest`
* ```homeproxy```为HomeProxy的配置文件，用本项目下的```homeproxy```替换```/etc/config/homeproxy```
- 节点设置
  - 订阅　　　//这里订阅自己的机场
- 客户端设置
  - 路由节点　　//编辑每一个节点，除`Apple`外，删除`URLTest节点`中的`[direct]Apple`，选择自己分流的节点，可以选择多个；`Apple`需要直连的，不用修改`Apple`，需要代理`Apple`时，删除`URLTest节点`中的`[direct]Apple`，选择自己分流的节点，可以选择多个，DNS规则中，`DNS_Rule_Apple`规则选择`DNS_Server_Apple`；`节点`为`URLTest`和`URLTest节点`有多个时，会自动测试哪个节点最快，优先选择快的出口。
  - 路由设置　　//默认出站，如何想要规则未命中全使用代理就选择`MainlyExport`，未命中直连:选择`直连`，默认选择直连即可
  - DNS规则    //`域名解析策略`中设置解析规则，当前代理的策略，默认全是只返回IPV4，如有需要可以修改
* 保存并应用,这时会显示HomeProxy正在运行
* 如果sing-box报配置错误,卸载插件,然后删除`/etc/homeproxy`和`/run/homeproyx/`目录,删除`/etc/config/homeproxy`文件，再安装`homeproxy`，再替换`/etc/config/homeproxy`文件
* 如果在意DNS泄漏，按如下方法修改
- 客户端设置
  - DNS设置　　//`默认DNS服务器`修改为`DNS_Server_MainlyExport`
  - DNS规则　　//`DNS_Rule_China`选择`DNS_Server_MainlyExport`，`DNS_Server_Apple`规则选择`DNS_Server_Apple`
# HomeProxy 自定义规则文件添加说明
- 客户端设置
  - 规则集　　//添加一个直连规则集,类型：远程，格式：源文件，规则集URL:```https://www.ghproxy.cn/```后面跟github地址，出站：直连；不要使用本仓库下的direct.json，里面域名地址是我随便写的，先fork到自己仓库，然后修改为自己想要的域名；添加代理规则和直连规则的方式一样。
* 只添加需要的规则，为空的规则要删除，不然HomeProxy会报错，```domain,domain_suffix,domain_keyword,ip_cidr```这四项，哪项有数据就保留哪一项，不要留空。
``` bash
{
  "version": 1,
  "rules": [
    {
      "domain": [
      ]
    },
    {
      "domain_suffix": [
      ]
    },
    {
      "domain_keyword": [
      ]
    },
    {
      "ip_cidr":[
      ]
    }
  ]
}
```
- 客户端设置
  - 路由规则　　//修改```NoChina```，规则集中添加刚添加的proxy规则集，修改```China```，规则集中添加刚添加的direct规则集
  - DNS规则　　//修改```DNS_Rule_China```，规则集中添加刚添加的direct规则集，修改```DNS_Rule_NoChina```，规则集中添加刚添加的proxy规则集
* 保存并应用
