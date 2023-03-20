# 创建一个插件

在这一节中，你将了解到如何创建并应用一个插件。

## 接口

插件接口是这样被声明的：

```Typescript
export default interface Plugin {
    init(request: VercelRequest, app: VCLight): Promise<void>;

    process(request: VercelRequest, response: ServerResponse, responseContent: Response, app: VCLight): Promise<void>;

}
```

这意味着，你需要让你的插件含有 `init` 和 `process` 两个函数。

其中，`init` 函数中应该包含插件被初始化时要做的事，`process` 函数应该包含对请求的处理。

我们建议你在 `init` 函数中放置初始化代码而不是在构造函数中包含。另外，你也可以在这里进行一些初始化操作（如拉取数据）。

## 创建插件类

插件类需要满足接口 `Plugin`。

```Typescript
class ExamplePlugin implements Plugin {
    async init(request: VercelRequest, app: VCLight): Promise<void> {}

    async process(request: VercelRequest, response: ServerResponse, responseContent: Response, app: VCLight): Promise<void> {}
}
```

接下来你可以在两个函数内填写相关代码。

例如，你想在访问 `/hello/` 时展示 "Hello!"，你可以这样做：

```Typescript
class ExamplePlugin implements Plugin {
    async init(request: VercelRequest, app: VCLight): Promise<void> {
        //We don't need to do things here
    }

    async process(request: VercelRequest, response: ServerResponse, responseContent: Response, app: VCLight): Promise<void> {
        if(url.parse(<string>request.url).pathname == "/hello/") {
            responseContent.response = "Hello!";
        }
    }
}
```

## 应用实例

要应用这个插件，我们需要先创建一个插件实例：

```Typescript
const examplePlugin = new ExamplePlugin();
```

使用 `app.use()` 来应用插件实例：

```Typescript
app.use(examplePlugin);
```

至此，你已经学会如何创建并应用一个插件了。
