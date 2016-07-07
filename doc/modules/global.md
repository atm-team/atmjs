```js
{
    // 获取 ~/.atmjs/server.json文件内容
    getServerConfig: function (callback) {},

    // 设置 ~/.atmjs/server.json文件内容
    setServerConfig: function (jsons, callback) {},

    // 关联站点
    relateSite: function (arg, callback) {},

    // 删除站点
    delSite: function (arg, callback) {},

    // 创建站点
    createSite: function (arg, callback) {},

    // 创建新版本
    createVersion: function (arg, callback) {},

    // 获取 ~/.atmjs/server.json里面的配置
    getServerSettings: function () {},

    // 获取站点与站点开发路径的关系地图
    getMaps: function () {},

    // 根据站点名称返回站点的开发路径
    getSitePath: function (siteName) {},

    // 获取http端口
    getHttpPort: function () {},

    // 判断是否需要logger
    getNeedLogger: function () {},

    // 获取https相关配置
    getHttpsConfig: function () {},

    // 获取https状态
    getHttpsStatus: function () {},

    // 获取静态文件根路径
    getAssetsRoot: function (sitePath, target) {},

    // 获取gui用到的全部数据
    getSiteDatas: function (siteName) {},

    getSiteSettings: function (sitePath) {},

    defaultSiteConfigs: {}

    // 获取站点的json文件配置
    getSiteConfigs: function (sitePath) {},

    // 获取版本下的相关信息
    getVersionInfo: function (arg) {},
}
```