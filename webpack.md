Webpack用来将es6,commonjs等语法的文件编译为浏览器可以识别的文件

1.npm i webpack webpack-cli -D      安装webpack和脚手架 -D是开发版本
2.npx webpack '入口文件' --mode=development        npx将webpack暂时放入环境变量里,执行命令

五大核心概念
1.entry 入口 从哪个文件开始打包
2.output 输出 打包到哪
3.loader 加载器  webpack本身只能处理js,json文件,借助loader解析其他文件
4.plugins 插件
5.mode 模式 development和production

处理css资源 loader 插件
npm i css-loader style-loader
<!-- 由于css被集成在js文件里,js解析需要时间,所以会出现闪屏情况,mini-css-extract-plugin会将css单独提取出成一个文件,后续html插件会自动插入link标签 -->
npm install --save-dev mini-css-extract-plugin
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
{
    test: /\.css$/,
    use: [
        //执行顺序从下到上
          <!-- // "style-loader",//将js中的css部分通过style标签添加到html中 -->
        MiniCssExtractPlugin.loader,
        "css-loader"//将css编译为commonjs模块到js中
    ]
},

处理图片 loader
{
    test:/\.(png|jpe?g|gif|webp|svg)$/,
    type:'asset',
    parser:{
        dataUrlCondition:{
            //小于10kb的图片转base64,减少请求数量
            //缺点,体积会变大
            maxSize:10*1024//单位从字节(B)开始算,小于10kb
        }
    }
}

处理字体 loader
{
    test:/\.(ttf|woff2?)$/,
    type:'asset/resource',
    generator:{
        filename:'static/font/[hash10][ext][query]'
    }
}

eslint 插件 语法规范和检查
npm i eslint eslint-webpack-plugin
1.创建.eslintrc.js文件
2.将规则写入文件
3.webpack-plugin配置eslint
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [new ESLintPlugin(options)],
  // ...
};

babel loader js兼容性处理
npm install -D babel-loader @babel/core @babel/preset-env
1.创建.babel.config.js文件
2.写入预设
3.webpack-loader配置babel
{
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,//不处理
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
}

处理html 插件
npm install --save-dev html-webpack-plugin
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  plugins: [new HtmlWebpackPlugin()],
};

开发服务器 直接配置 监视src 模拟自动打包
npm install --save-dev webpack-dev-server
devServer:{
  host:'localhost',
  port:3000,
  open:true//自动打开浏览器
}

postcss css兼容性处理

css-minimizer-webpack-plugin css压缩插件
$ npm install css-minimizer-webpack-plugin --save-dev
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

js&&html文件 webpack已经默认开启压缩


SourceMap 定位错误的行和列
devtool: "cheap-module-source-map" 打包速度快,但没有列映射 测试环境
devtool: "source-map", 生产环境