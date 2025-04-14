简体中文 | [English](README_en.md)

## valaxy-yun-external-link

`valaxy-yun-external-link` 是一个用于安全验证的Vue组件，用于验证用户输入的 URL 是否安全。它支持Valaxy原生主题Yun主题

### 实现功能

- **安全验证**：通过Vue组件劫持用户点击文章超链接的行为，重定向到站内告知页面的链接。
- **自定义配置**：允许用户自定义配置，如自定义跳转页面的CSS样式、跳转页面的标题、跳转页面的描述等。
- **Base64加密**：使用Base64加密算法对地址栏URL进行加密，以保护用户隐私。
- **控制台信息显示**：在控制台输出调试信息，方便开发者调试，当点击一个链接时，控制台会输出"Opening external link: "。
- **兼容性**：兼容Twikoo评论区，防止恶意链接。
- **智能性**：会自动检测本站URL，如果用户点击本站URL，则不会跳转，而是直接打开本站页面。

### 使用方法

1. Clone这个仓库的源码valaxy-yun-external-link仓库到本地。
2. 将本仓库内的文件除`README.md`和`node_modules`外的所有文件复制到你的Valaxy主题Yun主题的根目录下。
3. 在控制台重新运行`pnpm valaxy`以加载新增的组件。
4. 进入网站查看组件是否生效。


> [!TIP]
> 如果放入`components`和`layouts`文件插件并未加载或进入首页报错,请将`components`和`layouts`内的文件删除  
> 将`node_modules`文件放入Valaxy主题根目录内


### 文件夹结构

```
/--
  |-- components/                            # Vue组件文件夹
    |-- LinkInterceptor.vue                  # Vue组件文件
  |-- layouts/                               # Vue布局文件夹
    |-- post.vue                             # 文章发页面的Vue布局文件
  |-- node_modules/                          # Node模块文件夹（如果无报错请不要使用这个文件）
  |-- README.md                              # 帮助文档
  |-- public                                 # 公共资源文件夹
    |-- external-link.html                   # 中转页面的Html文件
```

### 修改建议

1. 如果你想要修改中转页面的背景颜色，请修改
`public/external-link.html`文件中的`<body>`标签的`style`属性中的`background-color`值 和  
`public/external-link.html`文件中第171-197行的对应值  
对应代码
```html
  // 根据模式设置背景颜色、文字颜色和背景线条
  if (mode === 'normal') {
    document.body.style.backgroundColor = '#FFFFFF'; // 白色背景
    document.body.style.color = '#000'; // 文字颜色改为黑色

    // 调整按钮样式
    const buttons = document.querySelectorAll('.btn');
    buttons.forEach(button => {
      button.style.backgroundColor = '#333'; // 深色背景
      button.style.color = '#FFF'; // 浅色文字
    });

    // 调整背景线条为灰色
    document.body.style.backgroundImage = `
      linear-gradient(rgba(128, 128, 128, 0.1) 1px, transparent 1px),
      linear-gradient(90deg, rgba(128, 128, 128, 0.1) 1px, transparent 1px)
    `;
  } else {
    document.body.style.backgroundColor = '#222'; // 默认黑色模式背景
    document.body.style.color = '#FFF'; // 文字颜色改为白色

    // 恢复默认背景线条
    document.body.style.backgroundImage = `
      linear-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255, 255, 255, 0.1) 1px, transparent 1px)
    `;
  }
```

2. 如果你想要修改中转页面的标题和描述，请修改  
`public/external-link.html`文件中第83-95行的对应值  
对应代码

```html
<body>
  <div class="main-container">
    <div class="button-group">
      <button id="direct-link" class="btn">直接跳转</button>
      <a href="/" class="btn">返回首页</a>
    </div>
    
    <div class="content-box">
      <h2>即将跳转到外部网站</h2>
      <p>您即将访问: <strong id="external-url"></strong></p>
      <p>将在 <span id="countdown">5</span> 秒后自动跳转...</p>
    </div>
  </div>
```


> [!WARNING]
> 如果不是个人需求，请不要修改Vue代码，否则可能会导致组件失效。  
> 或者出现重复跳出新标签页的严重Bug


### 问题反馈

如果有任何问题，请提 issue 或者联系作者邮箱 Mete0r_xsc@hotmail.com，或者在网站中进行提问：[https://www.xscnas.top](https://www.xscnas.top)