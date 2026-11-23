# 赛事摄影队交换资料

本地资料交换采用虚构机构、业务编号和文件，目录、原件与接收端响应均为合成数据。服务仅监听本机回环地址。

## 运行环境

使用 Node.js 22。启动前用 `npm ci` 恢复锁定清单，`npm start` 启动监听，`npm test` 检查来源接口及原件。监听端口可用 `PORT` 环境变量指定。申请示例保存于 `request.json`。

## 资料交换约定

相机事件以设备单调时刻标注，时标给出当地时间和每个相机批的参考点，按毫秒等比例解释。联系单中的盒坐标以原图坐标为准，矩形左、上、宽、高。号码配对须同时适用项目、站点范围与跑者棒次窗口；时标公差内不唯一则进入人工确认。原件的 `claim` 只适用于本相机批内的图，`/camera/original` 按 `photo` 和 `claim` 领取。各相机日额度从 `/camera/quota` 取得。私人领取可按同框框位遮挡没有给予私人共享同意的人，公开用图要求所有未遮挡可辨认者有公开许可。无法完整遮挡时不得交付给对应人群。

来源入口用 HTTP 返回资料。选择 `first` 或 `change` 可以观察同一批业务材料的两个时点，请向 `PUT /_source/phase` 提交包含 `phase` 的 JSON 对象。启动时默认 `first`。每个监听实例的演练时点及回执相互独立，新进程不继承演练端的状态。

接口路径、请求键及返回材料由 `wire.json` 描述，附属原件在 `materials` 中。路径表列出本地演练内容，不含外部网络服务。对返回文件的地址应领取对应响应，获取时的状态、时间及内容归属以来源声明为准。

| 方法和路径 | 查询字段 | 录入字段 |
| --- | --- | --- |
| `GET /camera/catalog` | 无 | 无 |
| `GET /camera/clock` | 无 | 无 |
| `GET /camera/quota` | 无 | 无 |
| `GET /previews/a10.svg` | 无 | 无 |
| `GET /camera/original` | `claim`、`photo` | 无 |
| `GET /previews/a11.svg` | 无 | 无 |
| `GET /previews/b41.svg` | 无 | 无 |
| `GET /registration` | 无 | 无 |
| `GET /consent` | 无 | 无 |
| `GET /published` | 无 | 无 |
| `POST /photography/withdraw` | 无 | `link`、`requestId` |

回执可以按客户端办理号查询 `GET /receipts?requestId=…`。有时源端接受后会在返回前断开，查询仍能看到真实演练回执。
