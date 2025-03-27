<!--
 * @Author: XXXX XXXX@XXX.com
 * @Date: 2025-03-25 15:23:02
 * @LastEditors: luoyang luoyang@stu.xidian.edu.cn
 * @LastEditTime: 2025-03-27 12:38:29
 * @FilePath: /project/PythonFileHeader/README.md
 * Copyright (c) 2025 by XXXX, All Rights Reserved.
 * # 
-->
# PythonFileHeader

### Fork from [OBKoro1/koro1FileHeader](https://github.com/OBKoro1/koro1FileHeader)

因为不会调`koro1FileHeader`的配置文件，所以阅读源码，注释修改了`python`的头部注释逻辑生成使用`#`的头文件注释，然后生成了我目前在使用的风格


### 注释实例如下：
```python
# coding=utf-8
# 
# Author: XXXX XXXX@XXX.com
# Date: 2025-03-25 15:23:02
# LastEditors: XXXX XXXX@XXX.com
# LastEditTime: 2025-03-27 12:11:15
# FilePath: /project/PythonFileHeader/template.py
# Copyright (c) 2025 by XXXX, All Rights Reserved.
# 
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
#     http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
```

使用时，需要修改本地`korofileheader`插件代码
> 一般在：`~/.vscode-server/extensions/obkoro1.korofileheader-4.9.3/src/languageOutPut/languageDifferent.js`

修改部分：124-145行
```js
  // 头部注释 头尾链接
  topHeadEnd: function () {
    const topHeadEndObj = {
      javascript: `/*\r\n${this.obj.str} */\r\n`,
      lua: `--[[\r\n${this.obj.str}--]]\r\n`,
      // python: `'''\r\n${this.obj.str}'''\r\n`,
      python: `${this.obj.str}`,
      html: `<!--\r\n${this.obj.str}-->\r\n`,
      vb: `'\r\n${this.obj.str}'\r\n`,
      shellscript: `###\r\n${this.obj.str}### \r\n`
    }
    return topHeadEndObj[this.obj.fileEnd]
  },
  // 头部注释 中间部分
  topMiddle: function () {
    const topMiddleObj = {
      '/^javascript$|^html$/': ` * ${this.atSymbol[0]}${this.obj.key}${this.colon[0]}${this.obj.value}\r\n`,
      // '/^python|^lua$/': `${this.obj.key}${this.colon[0]}${this.obj.value}\r\n`,
      '/^python|^lua$/': `# ${this.obj.key}${this.colon[0]}${this.obj.value}\r\n`,
      '/^vb$/': `' ${this.atSymbol[0]}${this.obj.key}${this.colon[0]}${this.obj.value}\r\n`,
      '/^shellscript$/': ` # ${this.atSymbol[0]}${this.obj.key}${this.colon[0]}${this.obj.value}\r\n`
    }
    return util.matchProperty(topMiddleObj, this.obj.fileEnd)
  },
```

对于`.vscode/settings.json`的相关设置
```json
{
  // 头部注释
  "fileheader.customMade": {
    // Author字段是文件的创建者 可以在specialOptions中更改特殊属性
    // 公司项目和个人项目可以配置不同的用户名与邮箱 搜索: gitconfig includeIf  比如: https://ayase.moe/2021/03/09/customized-git-config/
    // 自动提取当前git config中的: 用户名、邮箱
    "Author": "git config user.name && git config user.email", // 同时获取用户名与邮箱
    // "Author": "git config user.name", // 仅获取用户名
    // "Author": "git config user.email", // 仅获取邮箱
    // "Author": "OBKoro1", // 写死的固定值 不从git config中获取
    "Date": "Do not edit", // 文件创建时间(不变)
    // LastEditors、LastEditTime、FilePath将会自动更新 如果觉得时间更新的太频繁可以使用throttleTime(默认为1分钟)配置更改更新时间。
    "LastEditors": "git config user.name && git config user.email", // 文件最后编辑者 与Author字段一致
    // 由于编辑文件就会变更最后编辑时间，多人协作中合并的时候会导致merge
    // 可以将时间颗粒度改为周、或者月，这样冲突就减少很多。搜索变更时间格式: dateFormat
    "LastEditTime": "Do not edit", // 文件最后编辑时间
    // 输出相对路径，类似: /文件夹名称/src/index.js
    "FilePath": "Do not edit", // 文件在项目中的相对路径 自动更新
    // 插件会自动将光标移动到Description选项中 方便输入 Description字段可以在specialOptions更改
    // "Description": "", // 介绍文件的作用、文件的入参、出参。
    // custom_string_obkoro1~custom_string_obkoro100都可以输出自定义信息
    // 可以设置多条自定义信息 设置个性签名、留下QQ、微信联系方式、输入空行等
    // "custom_string_obkoro1": "",
    // 版权声明 保留文件所有权利 自动替换年份 获取git配置的用户名和邮箱
    // 版权声明获取git配置, 与Author字段一致: ${git_name} ${git_email} ${git_name_email}
    // "custom_string_obkoro1_copyright": "Copyright (c) ${now_year} by ${git_name_email}, All Rights Reserved. "
    "custom_string_obkoro1_copyright": "Copyright (c) ${now_year} by XXXX, All Rights Reserved.\n# "
  },
  // 函数注释
  "fileheader.cursorMode": {
    "description": "", // 函数注释生成之后，光标移动到这里
    "param": "", // param 开启函数参数自动提取 需要将光标放在函数行或者函数上方的空白行
    "return": ""
  },
  // 插件配置项
  "fileheader.configObj": {
    "autoAdd": true, // 检测文件没有头部注释，自动添加文件头部注释
    "autoAddLine": 100, // 文件超过多少行数 不再自动添加头部注释
    "autoAlready": true, // 只添加插件支持的语言以及用户通过`language`选项自定义的注释
    "supportAutoLanguage": [], // 设置之后，在数组内的文件才支持自动添加
    // 自动添加头部注释黑名单
    "prohibitAutoAdd": ["json"],
    "prohibitItemAutoAdd": [
      "项目的全称禁止项目自动添加头部注释, 使用快捷键自行添加"
    ],
    "folderBlacklist": ["node_modules"], // 文件夹或文件名禁止自动添加头部注释
    "wideSame": false, // 头部注释等宽设置
    "wideNum": 13, // 头部注释字段长度 默认为13
    "functionWideNum": 0, // 函数注释等宽设置 设为0 即为关闭
    // 头部注释第几行插入
    "headInsertLine": {
      "php": 2 // php文件 插入到第二行
    },
    "beforeAnnotation": {
      "py": "# coding=utf-8\n# "
    }, // 头部注释之前插入内容
    "afterAnnotation": {
      "py": "# Licensed under the Apache License, Version 2.0 (the \"License\");\n# you may not use this file except in compliance with the License.\n# You may obtain a copy of the License at\n# \n#     http://www.apache.org/licenses/LICENSE-2.0\n# \n# Unless required by applicable law or agreed to in writing, software\n# distributed under the License is distributed on an \"AS IS\" BASIS,\n# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.\n# See the License for the specific language governing permissions and\n# limitations under the License."
    }, // 头部注释之后插入内容
    "specialOptions": {}, // 特殊字段自定义 比如: Author、LastEditTime、LastEditors、FilePath、Description、Date等
    "switch": {
      "newlineAddAnnotation": true // 默认遇到换行符(\r\n \n \r)添加注释符号
    },
    "moveCursor": false, // 自动移动光标到Description所在行
    "dateFormat": "YYYY-MM-DD HH:mm:ss",
    "atSymbol": ["@", "@"], // 更改所有文件的自定义注释中的@符号
    "atSymbolObj": {}, //  更改单独语言/文件的@
    "colon": [": ", ": "], // 更改所有文件的注释冒号
    "colonObj": {}, //  更改单独语言/文件的冒号
    "filePathColon": "路径分隔符替换", // 默认值： mac: / window是: \
    "showErrorMessage": false, // 是否显示插件错误通知 用于debugger
    "writeLog": false, // 错误日志生成
    "CheckFileChange": false, // 单个文件保存时进行diff检查
    "createHeader": false, // 新建文件自动添加头部注释
    "useWorker": false, // 是否使用工作区设置
    "designAddHead": false, // 添加注释图案时添加头部注释
    "headDesignName": "random", // 图案注释使用哪个图案
    "headDesign": false, // 是否使用图案注释替换头部注释
    // 自定义配置是否在函数内生成注释 不同文件类型和语言类型
    "cursorModeInternalAll": {}, // 默认为false 在函数外生成函数注释
    "openFunctionParamsCheck": true, // 开启关闭自动提取添加函数参数
    "functionParamsShape": ["{", "}"], // 函数参数外形自定义
    // "functionParamsShape": "no type" 函数参数不需要类型
    "functionBlankSpaceAll": {}, // 函数注释空格缩进 默认为空对象 默认值为0 不缩进
    "functionTypeSymbol": "*", // 参数没有类型时的默认值
    "typeParamOrder": "type param", // 参数类型 和 参数的位置自定义
    "NoMatchParams": "no show param", // 没匹配到函数参数，是否显示@param与@return这两行 默认不显示param
    "functionParamAddStr": "", // 在 type param 后面增加字符串 可能是冒号，方便输入参数描述
    // 自定义语言注释，自定义取消 head、end 部分
    // 不设置自定义配置language无效 默认都有head、end
    "customHasHeadEnd": {}, // "cancel head and function" | "cancel head" | "cancel function"
    "throttleTime": 60000, // 对同一个文件 需要过1分钟再次修改文件并保存才会更新注释
    // 自定义语言注释符号，覆盖插件的注释格式
    "language": {
      // js后缀文件
      "js": {
        "head": "/$$",
        "middle": " $ @",
        "end": " $/",
        // 函数自定义注释符号：如果有此配置 会默认使用
        "functionSymbol": {
          "head": "/******* ", // 统一增加几个*号
          "middle": " * @",
          "end": " */"
        },
        "functionParams": "typescript" // 函数注释使用ts语言的解析逻辑
      },
      // 一次匹配多种文件后缀文件 不用重复设置
      "h/hpp/cpp": {
        "head": "/*** ", // 统一增加几个*号
        "middle": " * @",
        "end": " */"
      },
      // 针对有特殊要求的文件如：test.blade.php
      "blade.php": {
        "head": "<!--",
        "middle": " * @",
        "end": "-->"
      }
    },
    // 默认注释  没有匹配到注释符号的时候使用。
    "annotationStr": {
      "head": "/*",
      "middle": " * @",
      "end": " */",
      "use": true
    }
}
}
```