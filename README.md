# gbook-plugin-fontsettings
设置网站的字体和颜色主题以获得更好的阅读体验

### 禁用此插件

这是默认插件，可以使用以下 `book.json` 配置禁用它：

```
{
    plugins: ["-fontsettings"]
}
```

### 配置

可以在以下位置配置此插件 `book.json` ：

默认配置为：

```js
{
    "pluginsConfig": {
        "fontsettings": {
            "theme": 'white', // 'sepia', 'night' or 'white',
            "family": 'sans', // 'serif' or 'sans',
            "size": 2         // 1 - 4
        }
    }
}
```

### 插件API

此插件公开以下API，以轻松允许新主题管理插件行为。

所有的API函数使用前缀 `gbook.fontsettings.` ，例如 `gbook.fontsettings.enlargeFontSize()`

#### 文字大小设置

##### `gbook.fontsettings.enlargeFontSize()`

将文档的字体大小增加 `1` 。最大值为 `4`

##### `gbook.fontsettings.reduceFontSize()`

将文档的字体大小减 `1` 。最小值为 `1`

#### 字体设置

每个字体系列都应描述为：

```js
var fontFamily = {
    config: 'sans',  // 主题中book.json中字体家族的名称
    text: 'Sans',    // 在菜单中显示字体的名称 
    id: 0            // 此font-family附加到CSS类的id
};
```

该 `text` 属性将用于在fontsettings下拉菜单中显示字体名称。

`config` 属性允许主题的用户在其 `book.json` 中选择默认字体。您必须在主题的前端JavaScript中处理所选字体的设置。

例如：

```json
// book.json
{
    plugins: ["my-theme"],
    pluginsConfig: {
        "my-theme": {
            "font-family": "sans"
        }
    }
}
```

```js
// my-theme.js
require('gbook', function(gbook) {
    var FONT_FAMILIES = [
        {
            config: 'sans',
            text: 'Sans',
            id: 0
        },
        {
            config: 'serif',
            text: 'Serif',
            id: 1
        }
    ];

    gbook.events.on('start', function(e, config) {
        // 读取配置
        var themeConfig = config['my-theme'],
            defaultFont = themeConfig['font-family'];

        // 初始化新的字体
        gbook.fontsettings.setFamilies(FONT_FAMILIES);
        // 设置为已配置的font-family
        gbook.fontsettings.setFamily(defaultFont);
    });
});
```
该 `id` 属性使您可以定义用于CSS规则的特定ID，如下所述。

##### CSS规则

在菜单中选择字体后，CSS类 `font-family-<id>` 将应用于主题书的根元素 `<div class="book">`

然后可以使用父选择器轻松地定义字体的CSS规则 `.book.font-family-<id>`：

```CSS
.book.font-family-<id> {
  font-family: 'My Awesome Font';
}
```

##### 管理字体

##### `gbook.fontsettings.getFamilies()`

返回当前设置的字体。

默认情况下，字体为：

```js
// 默认字体
var FAMILIES = [
    {
        config: 'serif',
        text: 'Serif',
        id: 0
    },
    {
        config: 'sans',
        text: 'Sans',
        id: 1
    }
];
```

##### `gbook.fontsettings.setFamilies()`

设置新的字体配置，作为字体对象的数组，由 `plugin-fontsettings` 格式使用：

```js
var FONT_FAMILIES = [
    {
        config: 'sans',
        text: 'Sans',
        id: 0
    },
    {
        config: 'serif',
        text: 'Serif',
        id: 1
    }
];

gbook.fontsettings.setFamilies(FONT_FAMILIES);
```

这将重新创建fontsettings菜单以反映所做的更改

##### `gbook.fontsettings.setFamily()`

以 font-family `config` 键作为参数，并更新本书使用的 font-family。

这基本上将CSS类与相应的家族一起应用 `id`： `.font-family-<id>`

#### 颜色主题

设置和操作颜色主题遵循与字体完全相同的规则。

这是插件中颜色主题的默认值：

```js
// 默认主题
var THEMES = [
    {
        config: 'white',
        text: 'White',
        id: 0
    },
    {
        config: 'sepia',
        text: 'Sepia',
        id: 1
    },
    {
        config: 'night',
        text: 'Night',
        id: 2
    }
];
```

##### CSS规则

用于颜色主题的CSS类将采用以下形式：`.color-theme-<id>`

**注意**: 不会 `id: 0` 应用带有颜色主题的CSS类。基本上，第一个颜色主题对应于默认主题的颜色。

例如，使用默认的颜色主题：

```js
gbook.fontsettings.setTheme('night');
```

将导致根元素的以下HTML状态：

```HTML
<div class="book color-theme-2">
```

而：

```js
gbook.fontsettings.setTheme('white');
```

将重置根元素的HTML状态：

```HTML
<div class="book">
```

##### 管理颜色主题

##### `gbook.fontsettings.getFamilies()`

返回当前设置的颜色主题。

默认情况下，字体为：

```js
// 默认主题
var THEMES = [
    {
        config: 'white',
        text: 'White',
        id: 0
    },
    {
        config: 'sepia',
        text: 'Sepia',
        id: 1
    },
    {
        config: 'night',
        text: 'Night',
        id: 2
    }
];
```

##### `gbook.fontsettings.setThemes()`

将新的颜色主题配置设置为字体对象的数组，供 `plugin-fontsettings` 格式使用：

```js
var COLOR_THEMES = [
    {
        config: 'light',
        text: 'Light',
        id: 0
    },
    {
        config: 'dark',
        text: 'Dark',
        id: 1
    }
];

gbook.fontsettings.setThemes(COLOR_THEMES);
```

这将重新创建fontsettings菜单以反映所做的更改。

##### `gbook.fontsettings.setTheme()`

以颜色主题 `config` 键作为参数并更新本书使用的颜色主题。

这将基本上应用具有相应主题 `id`: `.color theme-<id>` 的CSS类，如果所选主题 `id` 为 `0` ，则删除应用的CSS类。