# 路由请求

在这一节中，你会了解到如何使用 `vercel.json` 文件将请求路由到 VCLight 实例。

{% hint style="info" %}
**小提示：** 关于 `vercel.json` 的详细文档，可以在[这里](https://vercel.com/docs/project-configuration)找到
{% endhint %}

## 路由所有请求到实例

在 `vercel.json` 中写入这些可以使所有流量全部经过实例。

```Json
{
  "builds": [
    {
      "src": "src/*",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "src/main.ts"
    }
  ]
}
```

其中，`builds` 项标示了项目将以什么方式被构建。

需要注意的是，如果你没有填写这一项， Vercel 会使用 `package.json` 中的 `build` 脚本来构建。

`routes` 项标示了请求应该被路由到哪里。

如果几项路由有冲突，Vercel 会使用在列表中排列更前面的那项。

## 路由部分请求到实例

如果你只是想让 `/api/*` 的部分经过实例，你应该这样填写 `routes` 项：

```Json
"routes": [
  {
    "src": "/api/(.*)",
    "dest": "src/main.ts"
  }
]
```

如果你想让静态文件不经过实例，你可以这样填写 `vercel.json`：

```Json
{
  "builds": [
    {
      "src": "public/assets/*",
      "use": "@vercel/static"
    },
    {
      "src": "src/*",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/assets/(?<file>[^/]*)",
      "dest": "public/assets/$file"
    },
    {
      "src": "/(.*)",
      "dest": "src/main.ts"
    }
  ]
}
```
