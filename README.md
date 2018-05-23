# webapck-dev-server-https
webpack-dev-server搭建的服务器项目，启动本地https服务

1.首先在webpack项目中安装webpack-dev-server---------npm install --save-dev webpack-dev-server<br>
2.然后配置webpack.dev.conf.js(当然也可以其它名字)----为了开发规范，就命名当前名字<br>
3.然后贴配置，见webpack.dev.conf.js，跟平常项目配置一样，启动https服务，无非在配置devServer时，加一个https配置项，设置为true,开启https服务<br>

---------
原以为这样就一切万事大吉，，，no!你会发现浏览器会报不安全警告，然后就找原因，找呀找。。。。。
---------------------

