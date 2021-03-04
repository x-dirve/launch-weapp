公众号中拉起平台小程序
================================

由于官方的标签存在样式调整、动态内容渲染等方面的问题，因此封装了 html 页面中拉起平台小程序或 app 的开放标签的 web component 模块。

### 已支持平台
- [微信](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/Wechat_Open_Tag.html)

## 使用

- 直接使用
    1. 引入模块
        ```ts
        import xLaunch from "@x-drive/x-launch";
        ```
    1. 在 `html` 上使用
        ```html
        <x-launch
            type="wechat"
            path="/pages/custom/custom-page"
            username="gh_xxxxx"
            class="testWc"
        >
            <img src="../luckdraw/imgs/win/btn.png" />
        </x-launch>
        ```
    1. 参数
        - `type` 平台类型，目前只有微信一个渠道，且默认即为微信，可不填。支持取值：
            - `wechat`，拉起微信平台小程序
            - `wechat-app`，微信平台拉起 app
        - `path` 小程序落地页地址，选择 `wechat` 时必填
        - `username` 小程序原始 `id`，选择 `wechat` 时必填
        - `appid` 所需跳转的AppID，选择 `wechat-app` 时必填
        - `extinfo` 跳转所需额外信息，选择 `wechat-app` 时必填
        - `debug` 显示调试用的半透明浮层
    1. 事件
        事件返回值都在回传参数的 `detail` 字段中，可通过 `addEventListener` 监听事件或框架如果 `Vue` 的事件绑定方式监听。
        | 名称  | 返回值   | 备注                |
        | ----- | ------ | ------------------- |
        | ready | true   | 标签初始化完毕，可以进行点击操作 |
        | launch |       | 用户点击跳转按钮并对确认弹窗进行操作后触发 |
        | error | { errMsg: string } | 用户点击跳转按钮后出现错误 |
    1. 注意事项
        - 微信开放标签需要有一定内容才能正常使用，目前由于实现的问题暂时没能支持标签尺寸自动处理，因此需要在使用的时候 **`给标签设置合适的尺寸`**
    1. 当因为某些原因不能直接使用默认的标签名时，可以模块导出的类自行重新注册一个合适的自定义标签使用
        1. 重新注册名称
            ```ts
            import { XLaunch } from "@x-drive/x-launch"; 
            customElements.define("slaunch", XLaunch);
            ```
        1. 处理可能存在的告警问题（参照下方内容）
        1. 在项目中使用
            ```html
            <slaunch
                type="wechat"
                path="/pages/custom/custom-page"
                username="gh_xxxxx"
                class="testWc"
            >
                <img src="../luckdraw/imgs/win/btn.png" />
            </slaunch>
            ```
- Vue 中使用与直接使用类似。由于是自定义标签，直接使用的话 Vue 会有警告抛出来，可以有以下两种方式解决：
    - 在 `Vue.config.ignoredElements` 中加入 `x-launch-weapp`
        ```ts
        Vue.config.ignoredElements = ["x-launch"];
        ```
    - 使用 `Vue.use`
        ```ts
        import xLaunch from "@x-drive/x-launch";
        import Vue from "vue";
        Vue.use(xLaunch);
        ```
## TODO
    1. 增加平台检测