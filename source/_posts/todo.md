---
title: web小提示
---
<!-- 这里是平时做项目的时候踩过的一些坑，希望能帮到大家吧！ -->
<br/>
### webpack的一些问题

#### html页面引用图片路径出错的问题
``` javascript
可以在基础配置里添加如下:

{
    test: /\.html$/,
    loader: "html-loader",
    options: {
        ignoreCustomFragments: [/\{\{.*?}}/],
        // root: path.resolve(__dirname, '../dist/static/img'),
        // publicPath: '../',
        attrs: ['img:src', 'link:href']  // 这句配置是在尝试了多次之后，加上网上查找各种资料后添加(目前来看没问题)。
    }
}
```

#### 其他资源路径问题

``` javascript
    可以在分离css、sass等资源文件的时候添加下面配置

    test:  /\.(css|scss)$/,
    use: ExtractTextPlugin.extract({
        fallback: 'style-loader',
        use: [{
                loader: 'css-loader',
                options: {
                    importLoaders: 1
                }
            },
            'postcss-loader',
            {
                loader: 'sass-loader'
            },
        ],
        publicPath:'../../'   // 这个配置必须要有(以防找不到路径的情况)
    })
```

#### 这里放一个多页面配置(入口、出口)
可以加在你需要的文件（我是放在utils.js文件）

``` javascript
    // 页面模板
    var PAGE_PATH = path.resolve(__dirname, '../src') // 这里可以根据自己情况配置入口
    // var HTML_PATH = path.resolve(__dirname, '../src')
    var merge = require('webpack-merge')

    // 多入口配置
    exports.entries = function () {
    var entryFiles = glob.sync(PAGE_PATH + '/*.js') // 同步匹配文件
    var map = {}
    entryFiles.forEach((filePath) => {
        var filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        map[filename] = filePath
    })
    return map
    }

    // 多页面输出配置
    exports.htmlPlugin = function () {
    let entryHtml = glob.sync(PAGE_PATH + '/*.html')
    let arr = []
    entryHtml.forEach((filePath) => {
        let filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        let conf = {
        // 模板来源
        template: filePath,
        // 文件名称
        filename: filename + '.html',
        // 页面模板需要加对应的js脚本，如果不加这行则每个页面都会引入所有的js脚本
        chunks: ['manifest', 'vendor', filename],
        inject: true
        }
        if (process.env.NODE_ENV === 'production') {
            conf = merge(conf, {
                minify: {
                removeComments: true,
                collapseWhitespace: true,
                removeAttributeQuotes: true
                },
                chunksSortMode: 'dependency'
            })
        }
        arr.push(new HtmlWebpackPlugin(conf))
        })
        return arr
    }

    调用方法(入口配置如下)
    // entry: {
    // 	index: './src/index.js'
    // },
    entry: utils.entries(),

    开发环境配置（生产环境也如此）
    plugins: [
		new webpack.DefinePlugin({
			'process.env': config.dev.env
		}),
		// https://github.com/glenjamin/webpack-hot-middleware#installation--usage
		new webpack.HotModuleReplacementPlugin(),
		new webpack.NoEmitOnErrorsPlugin(),
		// https://github.com/ampedandwired/html-webpack-plugin
		// new HtmlWebpackPlugin({
		// 	filename: 'index.html',
		// 	template: 'index.html',
		// 	inject: true
		// }),
		new FriendlyErrorsPlugin()
	].concat(utils.htmlPlugin())  // todo:这里需要加上(匹配相应的文件)
```
