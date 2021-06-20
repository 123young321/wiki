
# React基于Ant-Design3.x环境搭建

## 配置镜像源
原本镜像
```
npm get registry
```
设置淘宝镜像
```
npm config set registry http://registry.npm.taobao.org/
```
镜像还原
```
npm config set registry https://registry.npmjs.org/
```

## 安装脚手架
```
npm install create-react-app -g

```
## 创建项目
```
npx create-react-app antd-app
cd antd-app
npm start
```
## 安装Ant-Design
```
npm install antd@"^3.26.20" --save
```
## 项目配置

### Ant-Design按需加载
安装所需要的包
```
npm install react-app-rewired@"^2.1.8" --save
npm install customize-cra@"^1.0.0" --save
```
修改配置文件 `package.json`
源文件：
```
"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test",
+   "test": "react-app-rewired test",
}
```
修改为：
```
"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-scripts eject"
},
```

安装 `babel-plugin-import@"^1.13.3`
```
npm install babel-plugin-import@"^1.13.3" --save
```
项目跟目录下创建 `config-overrides.js`
```
const { override, fixBabelImports } = require("customize-cra");
module.exports = override(
  fixBabelImports("import", {
    libraryName: "antd",
    libraryDirectory: "es",
    style: "css",
  })
);
```
### Ant-Design支持Less

参考地址：https://www.cnblogs.com/esofar/p/9631657.html
https://blog.csdn.net/qq_36209248/article/details/88684516
