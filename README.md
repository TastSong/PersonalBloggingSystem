# Personal Blogging System

## 环境配置

* JDK 8
* Maven
* Escape EE

## 运行

1. 通过Escape导入项目，这是一个标准的Maven项目
2. 运行Application.java
3. 访问 [http://127.0.0.1:9000](http://127.0.0.1:9000/) 初始化站点和管理员信息

**注：初始化完成后会在你的项目根目录生成一个 `tale.db` 的数据库文件，这个是sqlite的数据库文件，你可以使用sqlite数据库软件打开查看。**

## 程序目录

```
.
├── package.xml
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
└── target
```

Java源码在 `src/main/java` 中，`src/main/resouces` 文件夹中存放了框架配置文件和主题文件。

## 端口更改

修改端口的方式有多种，简单最的的英文在`resources/application.properties`文件中添加一行

```
server.port = 9001
```

修改为你自定义的端口。

> 注意：*项目不可以运行在带有空格和中文的目录*

## 主题制作

### 文件目录规范

```
├── README.md
├── archives.html
├── categories.html
├── index.html
├── layout.html
├── page-category.html
├── page.html
├── partial
│   ├── comments.html
│   ├── disqus_count.html
│   ├── footer.html
│   ├── google_analytics.html
│   ├── head.html
│   ├── header.html
│   ├── page.html
│   ├── pagination.html
│   ├── post.html
│   └── sidebar.html
├── post.html
├── screenshot.png
├── setting.html
├── static
│   ├── favicon.ico
│   ├── main.js
│   └── style.css
└── tags.html
```

这里大概分这么几个部分，`README` 是主题的介绍文件，主题根目录下的 `static` 文件夹存储静态资源，`screenshot.png` 是主题的预览图，其他是一些模板文件。

模板文件分必须和非必须，一般我们建议如上格式的模板文件，方便管理和维护。

### 必须文件

- `index.html`：主题首页模板，展示首页，必须
- `page.html`：页面模板，自定义页面展示用，如：关于我，非必须
- `post.html`：文章详情页面，展示文章内容，必须

### 非必须文件

- `archives.html`：文章归档模板，非必须，如果主题有归档页需要编写（一般都有）
- `categories.html`：分类列表模板，非必须
- `tags.html`：标签列表模板，非必须（一般都有）
- `page-category.html`：分类，标签模板，展示分类和标签页面，非必须

### partial 公共信息

- `comments.html`：评论模板，非必须，除非你的页面不需要评论
- `footer.html`：主题公共页尾，为方便统一调用，非必须（一般都有）
- `header.html`：主题公共页头，为方便统一调用，非必须（一般都有）

### 函数库

#### 公共函数

```java
/**
 * 返回网站首页链接，如：http://tastsong.top
 * 
 * 使用：${site_url()}
 */
String site_url()

/**
 * 返回网站首页之下的链接，如：http://tastsong.top/about
 * @param sub 网站根目录下的子链接
 *            
 * 使用：${site_url()}
 */
String site_url(String sub)

/**
 * 返回网站标题 
 * 
 * 使用：${site_title()}
 */
String site_title()

/**
 * 返回网站配置项，数据库存储的
 * @param key 要返回的项目键
 * 
 * 使用：${site_option('site_title')}
 */
String site_option(String key)

/**
 * 返回网站配置项，数据库存储的
 * @param key 要返回的项目键
 * @param defaultValue 当不存在key的时候默认值
 * 
 * 使用：${site_option('social_facebook')}
 */
String site_option(String key, String defaultValue)

/**
 * 返回站点的描述信息
 * 
 * 使用：${site_description()}
 */
String site_description()

/**
 * 返回主题URL
 * 
 * 使用：${theme_url()}
 */
String theme_url()

/**
 * 返回主题下的文件路径，如：http://xxx.com/templates/theme/a/static/style.css
 * @param sub 主题目录下的子链接
 * 
 * 使用：${theme_url('/static/style.css')}
 */
String theme_url(String sub)

/**
 * 返回gravatar头像地址
 * @param email 邮件地址
 * 
 * 使用：${gravatar('tastsong.top@gmail.com')}
 */
String gravatar(String email)

/**
 * 格式化unix时间戳为日期
 * @param unixTime  时间戳
 * 
 * 使用：${fmtdate(1487969607)}
 * 返回：2018-02-28
 */
String fmtdate(Integer unixTime)

/**
 * 格式化unix时间戳为日期
 * @param unixTime  时间戳
 * @param fmt       格式化表达式
 * 
 * 使用：${fmtdate(1487969607, 'EEE, d MMM yyyy')}
 * 返回：Wed, 4 Jul 2017
 */
String fmtdate(Integer unixTime, String fmt)

/**
 * 格式化日期
 * @param date  时间戳
 * @param fmt   格式化表达式
 * 
 * 使用：${fmtdate(date, 'EEE, d MMM yyyy')}
 * 返回：Wed, 4 Jul 2018
 */
String fmtdate(Date date, String fmt)

文档：https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html

/**
 * 获取随机数，最小为1
 * @param max   最大值
 * @param str   拼接文本
 * 
 * 使用：${random(5, '.png')}
 * 返回：3.png | 2.png | 4.png
 */
String random(int max, String str)

/**
 * 获取文章第一张图片
 * @param content 文章内容
 * 
 * 使用：${show_thumb(content)}
 */
String show_thumb(String content)
```

#### 主题函数

```java
/**
 * 获取网站关键词
 * 
 * 使用：${meta_keywords()}
 */
String meta_keywords()

/**
 * 获取网站描述文本
 * 
 * 使用：${meta_description()}
 */
String meta_description()

/**
 * 获取当前页面标题，没有则返回网站标题
 * 
 * 使用：${head_title()}
 */
String head_title()

/**
 * 获取当前页面标题，没有则返回网站标题
 * 
 * 使用：${head_title()}
 */
String head_title()

/**
 * 返回文章链接地址
 *
 * 使用：${permalink()}
 */
String permalink()

/**
 * 返回某一篇文章的链接地址
 * @param contents 文章对象
 *                 
 * 使用：${permalink(article)}
 */
String permalink(Contents contents)

/**
 * 返回当前文章创建时间
 * @param fmt 格式化时间表达式
 * 
 * 使用：${created('yyyy-MM-dd')}
 */
String created(String fmt)

/**
 * 返回当前文章最后修改时间
 * @param fmt 格式化时间表达式
 * 
 * 使用：${modified('yyyy-MM-dd')}
 */
String modified(String fmt)

/**
 * 返回当前文章浏览数
 *
* 使用：${views()}
 */
Integer views()

/**
 * 返回当前文章标签显示
 * @param split 多个标签分隔字符
 * 
 * 使用：${show_tags(',')}
 * 返回：<a href='x'>生活</a>,<a href='x'>纪念</a>
 */
String show_tags(String split)

/**
 * 返回当前文章内容
 * 
 * 使用：${show_content()}
 * 返回解析markdown为html之后的内容
 */
String show_content()

/**
 * 获取文章摘要
 * @param len   截取的文本字数
 * 
 * 使用：${excerpt(200)}
 * 当有设置 <!--more-->标签则取它前面的字符串，否则取200个字符
 */
String excerpt(int len)

/**
 * 获取当前文章的上一篇
 * 
 * 使用：${article_prev()}
 */
Contents article_prev()

/**
 * 获取当前文章的下一篇
 * 
 * 使用：${article_next()}
 */
Contents article_next()

/**
 * 获取最新的limit条文章
 * 
 * 使用：${recent_articles(5)}
 */
List<Content> recent_articles(int limit)

/**
 * 随机获取limit条文章
 * 
 * 使用：${rand_articles(5)}
 */
List<Content> rand_articles(int limit)

/**
 * 获取最新的limit条评论
 * 
 * 使用：${recent_comments(5)}
 */
List<Comments> recent_comments(int limit)

/**
 * 获取limit个分类，按分类下的文章数倒序
 * 
 * 使用：${categories(5)}
 */
List<MetaDto> categories(int limit)

/**
 * 随机获取limit个分类
 * 
 * 使用：${rand_categories(5)}
 */
List<MetaDto> rand_categories(int limit)

/**
 * 获取所有分类
 * 
 * 使用：${categories()}
 */
List<MetaDto> categories()

/**
 * 获取limit个标签，按标签下的文章数倒序
 * 
 * 使用：${tags(5)}
 */
List<MetaDto> tags(int limit)

/**
 * 随机获取limit个标签
 * 
 * 使用：${rand_tags(5)}
 */
List<MetaDto> rand_tags(int limit)

/**
 * 获取所有标签
 * 
 * 使用：${tags()}
 */
List<MetaDto> tags()

/**
 * 显示当前页面文章标题
 *
 * 使用：${title()}
 */
String title()

/**
 * 显示文章标题
 * @param contents 文章对象
 *
 * 使用：${title(article)}
 */
String title(Contents contents)

/**
 * 返回所有友链
 * 
 * 使用：${links()}
 */
List<MetaDto> links()

/**
 * 返回社交账号链接
 * @param socialtype  社交账号类型
 * 
 * 使用：${social_link('github')}
 */
String social_link(String socialtype)

/**
 * 获取当前文章/页面的评论
 * @param limit 每页获取条数
 *              
 * 使用：${comments(6)}
 */
Paginator<Comment> comments(int limit)

/**
 * 显示评论
 * @param noComment 评论为0的时候显示的文本
 * @param value     评论组装文本
 *                  
 * 使用：${comments_num('没有评论', ' %d 条评论')}
 */
String comments_num(String noComment, String value)
```





