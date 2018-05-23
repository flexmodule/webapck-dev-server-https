# webapck-dev-server-https
webpack-dev-server搭建的服务器项目，启动本地https服务

1.首先在webpack项目中安装webpack-dev-server---------npm install --save-dev webpack-dev-server<br>
2.然后配置webpack.dev.conf.js(当然也可以其它名字)----为了开发规范，就命名当前名字<br>
3.然后贴配置，跟平常项目配置一样，启动https服务，无非在配置devServer时，加一个https配置项，设置为true,开启https服务<br>
---------
原以为这样就一切万事大吉，，，no!你会发现浏览器会报不安全警告，然后就找原因，找呀找。。。。。
---------------------




'use strict'<br>
const utils = require('./utils')<br>
const webpack = require('webpack')<br>
const config = require('../config')<br>
const merge = require('webpack-merge')<br>
const baseWebpackConfig = require('./webpack.base.conf')<br>
const HtmlWebpackPlugin = require('html-webpack-plugin')<br>
const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')<br>
const portfinder = require('portfinder')<br>
const devWebpackConfig = merge(baseWebpackConfig, {<br>
    module: {<br>
        rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap, usePostCSS: true })<br>
    },<br>
    // cheap-module-eval-source-map is faster for development<br>
    devtool: config.dev.devtool,<br>
    // these devServer options should be customized in /config/index.js<br>
    devServer: {<br>
        clientLogLevel: 'warning',<br>
        historyApiFallback: true,<br>
        hot: true,<br>
        https: true,<br>
        host: process.env.HOST ||  config.dev.host,<br>
        port: process.env.PORT ||  config.dev.port,<br>
        open: config.dev.autoOpenBrowser,<br>
        overlay: config.dev.errorOverlay ? {<br>
            warnings: false,<br>
            errors: true,<br>
        publicPath: config.dev.assetsPublicPath,<br>
        proxy: config.dev.proxyTable,<br>
        quiet: true, // necessary for FriendlyErrorsPlugin<br>
        watchOptions: {<br>
            poll: config.dev.poll,<br>
        }<br>
    },<br>
    plugins: [<br>
        new webpack.DefinePlugin({<br>
            'process.env': require('../config/dev.env'),<br>
        }),<br>
        new webpack.HotModuleReplacementPlugin(),<br>
        new webpack.NamedModulesPlugin(), // HMR shows correct file names in console on update.<br>
        new webpack.NoEmitOnErrorsPlugin(),<br>
        // https://github.com/ampedandwired/html-webpack-plugin<br>
        new HtmlWebpackPlugin({<br>
            filename: 'index.html',<br>
            template: 'index.html',<br>
            inject: true<br>
        }),<br>
        new webpack.ProvidePlugin({<br>
        'window.Quill': 'quill'<br>
        })<br>
    ] <br>
})<br>
module.exports = new Promise((resolve, reject) => {<br>
    portfinder.basePort = process.env.PORT || config.dev.port<br>
    portfinder.getPort((err, port) => {<br>
        if (err) {<br>
            reject(err)<br>
        } else {<br>
            // publish the new Port, necessary for e2e tests<br>
            process.env.PORT = port<br>
                // add port to devServer config<br>
            devWebpackConfig.devServer.port = port<br>
            // Add FriendlyErrorsPlugin<br>
            devWebpackConfig.plugins.push(new FriendlyErrorsPlugin({<br>
                compilationSuccessInfo: {<br>
                    messages: [`Your application is running here: http://${config.dev.host}:${port}`],<br>
                },<br>
                onErrors: config.dev.notifyOnErrors ?<br>
                    utils.createNotifierCallback() :<br>
                    undefined<br>
            }))<br>
            resolve(devWebpackConfig)<br>
        }<br>
    })<br>
})<br>


