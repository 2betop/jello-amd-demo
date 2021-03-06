Jello amd 测试项目
============================

用来测试 amd 各种用法， 同时也演示了如何去让 Fis 去支持 amd。

目前已有 zrender 和 echarts 的使用示例。

**我们的目标是支持所有的 amd 用法，如果发现有不支持的用法，请提交示例，我们第一时间修复！**

## 为什么要用 amd

现有太多 amd 组件、模块都是以 amdjs 规范使用的，原来的 mod.js 不支持，导致每次都要手动修改后才能在 fis 中使用。
非常不便于维护与升级。

## 如何让 jello 项目 支持 amd?

默认 jello 采用的还是原来的 mod.js 方案，如果要完全支持 amd, 则需要做以下配置。

1. 修改 fis-conf.js

    ```javascript
    fis.config.set('modules.postprocessor.vm', 'amd');
    fis.config.set('modules.postprocessor.js', 'amd');
    ```
2. 修改 framework 为新的 [mod-amd.js](https://raw.githubusercontent.com/fex-team/mod/master/mod-amd.js)，**或者可以直接使用 [esl.js](https://github.com/ecomfe/esl) 或者 [require.js](https://github.com/jrburke/requirejs). 原来的 mod.js 已经不能使用了。**
3. 更多信息请查看 [fis-postprocessor-amd 说明](https://github.com/fex-team/fis-postprocessor-amd)

## 兼容性问题

除了 `require.async` 外，其他写法都兼容。其实原来的写法 `require.async(id | deps, callback)` 等价于 `require(deps, callback)`。

另外不在 `define` 中的 require(id) 这种用法，不是 amd 的标准用法，但是有很多老用户是这么写的，所以做了兼容处理，但是不推荐这么用, 因为这样你是没法把 mod-amd.js 直接换成 require.js 使用。

## 注意

由于 FIS 产出的 module id 为这种格式 ns:path/xxxx, require.js 认为它是 url 导致发送 script 请求而报错。

需要对 require.js 做修改，直接在最下面加上如下配置

```javascript
// 修改 require.js
// 由于 fis 中生成的 module id 格式为 xxx:path 这种格式，被认为是 url 导致 require.js 会发请求
require.jsExtRegExp = /^\/|:\/\/|\?/;
```
