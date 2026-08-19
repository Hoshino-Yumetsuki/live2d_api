# Live2D API

Live2D 看板娘插件 (https://www.fghrsh.net/post/123.html) 上使用的后端 API

### 特性

- 静态文件部署，无需服务器端运行时
- 支持 模型、皮肤 的 顺序切换 和 随机切换
- 支持 单模型 单皮肤 切换、多组皮肤 递归穷举
- 支持 同分组 多个模型 或 多个路径 的 加载切换

## 使用

### 环境要求
无需 PHP 或其他服务器端运行时。

### 目录结构

```shell
│  model_list.json              // 模型列表
│
├─model                         // 模型路径
│  └─GroupName                  // 模组分组
│      └─ModelName              // 模型名称
│
└─_headers                      // 静态资源响应头
```

### 添加模型

- 单模型 单皮肤 切换
    - 单次加载只输出一个皮肤
    - 皮肤放在 `textures` 文件夹，自动识别

```shell
│  index.json
│  model.moc
│  textures.cache       // 可选的皮肤列表缓存
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
└─textures
        default-costume.png
        school-costume.png
        winter-costume.png
```

- 单模型 多组皮肤 递归穷举
    - 多组皮肤 组合模型、穷举组合
    - 皮肤文件夹按 `texture_XX` 命名
    - 添加 `textures_order.json` 列出组合
```shell
│  index.json
│  model.moc
│  textures.cache       // 可选的皮肤列表缓存
│  textures_order.json
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
├─texture_00
│      00.png
│
├─texture_01
│      00.png
│      01.png
│      02.png
│
├─texture_02
│      00.png
│      01.png
│      02.png
│
└─texture_03
       00.png
       01.png
```

textures_order.json

```json
[
    ["texture_00"],
    ["texture_01","texture_02"],
    ["texture_03"]
]
```

textures.cache 为可选缓存。缺失时，客户端直接使用 `index.json` 中的 `textures` 配置。

- 同分组 多个模型 或 多个路径 切换
    - 修改 `model_list.json` 添加多个模型

```shell
│
├─model
│  ├─Group1
│  │  ├─Model1
│  │  │      index.json
│  │  │
│  │  └─Model2
│  │          index.json
│  │
│  ├─Group2
│  │  └─Model1
│  │          index.json
│  │
│  └─GroupName
│     └─ModelName
│          │  index.json
│          │  model.moc
│          │
│          ├─motions
│          └─textures
│
```

model_list.json
```json
{
    "models": [
        "GroupName/ModelName",
        [
            "Group1/Model1",
            "Group1/Model2",
            "Group2/Model1"
        ]
    ],
    "messages": [
        "Example 1",
        "Example 2"
    ]
}
```

### 资源访问
模型配置通过静态文件直接访问，例如：
- `/model_list.json` 获取模型和服装列表
- `/model/GroupName/ModelName/index.json` 获取模型配置
- `index.json` 中声明的纹理、模型和动作文件均为静态资源

## 版权声明

> (>▽<) 都看到这了，点个 Star 吧 ~

**API 内所有模型 版权均属于原作者，仅供研究学习，不得用于商业用途**  

MIT © FGHRSH
