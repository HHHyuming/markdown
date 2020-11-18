官方DOC https://eslint.bootcss.com/docs/user-guide/configuring



### Configuration

```
eslint 配置

example ============== 示例
module.exports = {
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:vue/essential"
    ],
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "vue"
    ],
    "rules": {
    }
};

```

## 配置项 

#### parserOptions

```
ecmaVersion: 指定ecmaScript 版本
sourceType - 设置为 "script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块)。
ecmaFeatures - 这是个对象，表示你想使用的额外的语言特性:
globalReturn - 允许在全局作用域下使用 return 语句
impliedStrict - 启用全局 strict mode (如果 ecmaVersion 是 5 或更高)
jsx - 启用 JSX


```

#### parser 解析器

```

```

#### processor  处理器

```
1.插件可以提供处理器。处理器可以从另一种文件中提取 JavaScript 代码，然后让 ESLint 检测 JavaScript 代码。或者处理器可以在预处理中转换 JavaScript 代码。

{
    "plugins": ["a-plugin"],
    "processor": "a-plugin/a-processor"
}

2.要为特定类型的文件指定处理器，请使用 overrides 键和 processor 键的组合。例如，下面对 *.md 文件使用处理器 a-plugin/markdown。


{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        }
    ]
}
```



#### 环境 env

```

1.要在配置文件里指定环境，使用 env 关键字指定你想启用的环境，并设置它们为 true。例如，以下示例启用了 browser 和 Node.js 的环境：
{
    "env": {
        "browser": true,
        "node": true
    }
}


2.或在 package.json 文件中：

{
    "name": "mypackage",
    "version": "0.0.1",
    "eslintConfig": {
        "env": {
            "browser": true,
            "node": true
        }
    }
}


3.在一个特定的插件中使用一种环境
{
    "plugins": ["example"],
    "env": {
        "example/custom": true
    }
}
```



#### globals 全局变量

```
要在配置文件中配置全局变量，请将 globals 配置属性设置为一个对象，该对象包含以你希望使用的每个全局变量。对于每个全局变量键，将对应的值设置为 "writable" 以允许重写变量，或 "readonly" 不允许重写变量。例如：


{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}


2.  可以使用字符串 "off" 禁用全局变量。例如，在大多数 ES2015 全局变量可用但 Promise 不可用的环境中，你可以使用以下配置:

{
    "env": {
        "es6": true
    },
    "globals": {
        "Promise": "off"
    }
}
```



#### 插件 plugins

```
ESLint 支持使用第三方插件。在使用插件之前，你必须使用 npm 安装它。

在配置文件里配置插件时，可以使用 plugins 关键字来存放插件名字的列表。插件名称可以省略 eslint-plugin- 前缀。

{
    "plugins": [
        "plugin1",
        "eslint-plugin-plugin2"
    ]
}
```

#### rules 规则

```
还可以使用 rules 连同错误级别和任何你想使用的选项，在配置文件中进行规则配置。例如：

{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}


插件的形式

{
    "plugins": [
        "plugin1"
    ],
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"],
        "plugin1/rule1": "error"
    }
}
```

