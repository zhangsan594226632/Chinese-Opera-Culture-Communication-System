

# 资料获取  点击  [**《基于springboot+vue中国戏曲文化传播系统》资料**](https://nwqbsc0rm1n.feishu.cn/docx/QnFZdiPRloKSzwxY7hdc6MLUnlb)
---

## 一、项目概述
### 1\.1 项目背景

戏曲是中华优秀传统文化核心载体，包含京剧、越剧、黄梅戏、川剧等众多流派，当下存在戏曲资料分散、传播渠道单一、年轻受众接触门槛高、戏曲艺术家信息无统一展示平台等问题。为数字化传承、推广戏曲文化，搭建**中国戏曲文化传播系统**，实现戏曲作品在线浏览、戏曲资料检索、戏曲艺术家展示、音频播放、用户评论收藏、后台全维度管理一体化功能，助力传统戏曲线上传播。

### 1\.2 项目目标

1. **用户端**：普通游客可浏览戏曲首页推荐、检索戏曲资料、查看戏曲详情、播放戏曲音频、查看艺术家介绍、注册登录收藏戏曲、发布评论互动。

2. **管理端**：管理员实现系统用户、轮播图、戏曲分类、戏曲信息、戏曲资料、艺术家全生命周期管理，提供戏曲类型数据统计可视化图表。

3. **技术目标**：基于 SpringBoot\+Vue 前后端分离架构，保证系统高可用、易维护，支持图片 / 音频文件上传、分页查询、条件检索、富文本编辑、ECharts 数据可视化。

### 1\.3 技术栈选型

#### 后端技术（SpringBoot）

- 核心框架：SpringBoot 2\.7\.x

- 持久层：MyBatis\-Plus（简化 CRUD、分页插件）

- 数据库：MySQL 8\.0

- 工具组件：

    - Hutool：工具类封装（日期、文件、加密）

    - EasyExcel：后期可扩展戏曲资料批量导入导出

    - FFmpeg：音频播放兼容处理

    - JWT：登录身份认证、权限校验

    - Lombok：简化实体类 get/set

    - Redis（可选）：缓存热门戏曲、收藏数据

- 文件存储：本地文件存储 / 阿里云 OSS（图片、音频）

- 接口规范：RESTful API，统一返回结果封装

#### 前端技术（Vue）

- 核心框架：Vue2 \+ Element UI（管理后台深色主题）

- 前台页面：原生 Vue \+ Element UI 简约浅色页面

- 图表组件：ECharts（戏曲分类饼图统计）

- 富文本编辑器：wangeditor（艺术家简介、戏曲介绍）

- 音频组件：HTML5 Audio 播放器

- 路由：Vue Router（页面导航、面包屑）

- 网络请求：Axios（请求拦截、响应拦截、token 携带）

- 分页组件：Element Pagination 分页控件

- 上传组件：el\-upload（图片、音频文件上传）

#### 开发 \& 部署工具

- 开发工具：IDEA、VS Code

- 版本控制：Git

- 部署：Tomcat / SpringBoot 内置 Tomcat、Nginx 反向代理

- 数据库可视化：Navicat

### 1\.4 系统角色划分

1. **普通游客（未登录）**：仅浏览首页、戏曲资料、戏曲详情、艺术家页面，无收藏、评论权限。

2. **注册用户（前台用户）**：注册 / 登录、浏览全部内容、收藏戏曲、发布评论、查看个人收藏。

3. **管理员（后台用户 admin）**：拥有全部后台管理权限，管理用户、轮播图、戏曲分类、戏曲信息、戏曲资料、艺术家、查看数据统计。

## 二、系统功能模块设计
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ce4f58105efe4831ab47a34314deae96.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e6c4b7b995ca4614bb8ec8fd4a6685e3.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/270028d488704bc097b30ec1214af9fa.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d7b8828258ec4d3fae3e4bfc99ee7c6d.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/09c43bb4ba3740839b99fe56019e51af.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/19f634c7ea704c3c83bf49329bcaba00.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e5f7463bde234ae2ba5951f2dd18540e.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5692455066894e44817e26738d2202d2.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/251fbe25c797419eb1a70688c021a1af.png#pic_center)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/66e96ebe02db47cdb5e837986a58fd90.png#pic_center)


### 2\.1 前台用户模块（面向普通访客 / 注册用户）

#### 2\.1\.1 登录 / 注册模块

1. **登录功能**

    - 账号密码登录，支持记住密码本地存储；

    - 登录校验：账号密码非空、数据库匹配；

    - 登录成功生成 JWT Token，存储 localStorage，全局请求自动携带；

    - 登录失败提示：账号不存在、密码错误。

2. **注册功能**

    - 字段：用户账号、用户密码、确认密码、用户姓名、头像上传、性别下拉选择、手机号码；

    - 校验规则：两次密码一致、手机号格式校验、账号唯一、头像非必传；

    - 注册成功自动跳转登录页面。

3. 页面跳转：登录页底部【注册用户】跳转注册；注册页底部【已有账号，直接登录】跳转登录。

#### 2\.1\.2 首页模块

1. 顶部导航栏：首页、戏曲资料、戏曲信息、艺术家；右上角用户头像入口（登录 / 个人中心）。

2. 顶部轮播图：后台配置戏曲主题轮播海报，自动轮播。

3. 戏曲卡片推荐区：网格布局展示戏曲封面、戏曲名称、所属艺术家、收藏星级；卡片点击跳转戏曲详情页。

4. 更多详情按钮：跳转戏曲信息列表页。

#### 2\.1\.3 戏曲资料模块

1. 顶部搜索框：按标题关键词模糊检索戏曲资料；

2. 戏曲资料卡片列表：展示封面图、资料标题、简介文本；

3. 文字资料列表：展示标题、发布日期、正文简介，分页展示。

#### 2\.1\.4 戏曲信息详情模块

1. 面包屑导航：首页 \> 戏曲信息，右上角返回按钮；

2. 左侧：戏曲封面大图展示；

3. 右侧戏曲基础信息：戏曲名称、戏曲分类、艺术家、发展历史、戏曲流派、收藏数量、点击次数；

4. 收藏按钮：登录用户可点击收藏 / 取消收藏，收藏数量实时更新；

5. 音频播放器：内置 HTML5 音频控件，支持播放、暂停、进度拖拽；

6. 评论区域：展示所有用户评论，登录用户可新增评论。

#### 2\.1\.5 艺术家页面

前台艺术家列表，展示全部戏曲艺术家头像、姓名、代表作，点击进入艺术家详情。

### 2\.2 后台管理模块（管理员登录深色管理系统）

#### 2\.2\.1 后台首页仪表盘

1. 左侧侧边栏菜单：首页、系统用户管理、轮播图管理、戏曲资料、戏曲信息管理、艺术家管理；

2. 顶部标签页：多页面标签缓存；右上角显示登录管理员账号；

3. 数据看板：

    - 戏曲信息总数统计卡片；

    - 戏曲分类饼图 ECharts 可视化：展示各戏曲分类作品占比，悬浮显示分类名称、数量、百分比；

#### 2\.2\.2 系统用户管理

1. 子菜单：管理员管理、前台用户管理；

2. 功能：分页列表、条件搜索、新增、详情、修改、删除系统账号；

3. 字段：账号、姓名、头像、性别、手机号、角色（管理员 / 普通用户）。

#### 2\.2\.3 轮播图管理

1. 轮播图分页列表；

2. 新增轮播图：上传图片、填写跳转链接、排序权重；

3. 修改、删除、上下线轮播图。

#### 2\.2\.4 戏曲资料管理

1. 戏曲资料分页列表；标题检索；

2. 新增 / 编辑：上传封面、标题、简介、正文；

3. 详情查看、单条 / 批量删除。

#### 2\.2\.5 戏曲信息管理

##### \[2\.2\.5\.1\] 戏曲分类管理

1. 戏曲分类列表：序号、分类名称；

2. 操作按钮：新增、详情、修改、删除、批量删除；

3. 顶部检索框：按分类名称模糊搜索；

4. 分页控件。

##### \[2\.2\.5\.2] 戏曲信息主列表

1. 列表字段：序号、戏曲名称、封面图片、戏曲分类、艺术家、发展历史、戏曲流派、视频、音频；

2. 顶部功能按钮：新增、详情、修改、删除、【戏剧类型统计】（跳转仪表盘饼图）；

3. 检索条件：戏曲名称、戏曲流派双条件搜索；

4. 行操作：详情、查看评论；

5. 分页展示全部戏曲作品。

##### \[2\.2\.5\.3\]新增 / 编辑戏曲信息

表单字段：戏曲名称、下拉选择戏曲分类、下拉关联艺术家、发展历史、戏曲流派、上传封面图、上传音频文件、上传视频文件、富文本戏曲介绍。

#### 2\.2\.6 艺术家管理

1. 艺术家列表页面：

    - 列表字段：序号、姓名、头像图片、性别、年龄、出生地、毕业院校、戏曲成就、戏曲作品；

    - 检索：姓名、戏曲作品双条件搜索；

    - 操作：新增、详情、修改、删除、批量删除；单行详情按钮。

2. 新增艺术家弹窗（弹窗式表单）：

    - 左侧基础输入框：姓名（必填）、性别下拉、出生地、年龄、毕业院校、戏曲成就、戏曲作品；

    - 右侧：图片上传组件；

    - 底部富文本编辑器 wangeditor：填写个人简介，支持文字加粗、图片、排版；

    - 提交 / 取消按钮，表单校验必填项。

## 三、数据库设计

### 3\.1 数据库整体说明

数据库名：`opera_culture`，采用 InnoDB 引擎，utf8mb4 字符集，支持 emoji、中文全兼容。
核心数据表：

1. `sys_user` 系统用户表（管理员 \+ 前台注册用户）

2. `banner` 轮播图表

3. `opera_category` 戏曲分类表

4. `opera_artist` 戏曲艺术家表

5. `opera_info` 戏曲信息主表（核心表）

6. `opera_material` 戏曲资料表

7. `opera_comment` 戏曲评论表

8. `opera_collect` 用户戏曲收藏中间表

### 3\.2 核心数据表结构

#### （1）sys\_user 系统用户表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|用户主键 ID|
|username|varchar\(50\)|NOT NULL UNIQUE|登录账号|
|password|varchar\(100\)|NOT NULL|加密密码|
|real\_name|varchar\(30\)|NOT NULL|用户姓名|
|avatar|varchar\(255\)|NULL|头像图片路径|
|gender|tinyint|NULL|0 女 1 男 2 未知|
|phone|varchar\(11\)|NULL|手机号|
|role|tinyint|NOT NULL|1 管理员 2 普通用户|
|create\_time|datetime|DEFAULT NOW\(\)|创建时间|
|update\_time|datetime|ON UPDATE NOW\(\)|更新时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除 0 未删 1 已删|

#### （2）opera\_category 戏曲分类表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|分类 ID|
|category\_name|varchar\(50\)|NOT NULL UNIQUE|戏曲分类名称（京剧 / 越剧等）|
|create\_time|datetime|DEFAULT NOW\(\)|创建时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

#### （3）opera\_artist 戏曲艺术家表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|艺术家 ID|
|name|varchar\(30\)|NOT NULL|艺术家姓名|
|avatar|varchar\(255\)|NULL|艺术家照片|
|gender|tinyint|NULL|性别|
|age|int|NULL|年龄|
|birth\_place|varchar\(50\)|NULL|出生地|
|graduate\_school|varchar\(50\)|NULL|毕业院校|
|achievement|varchar\(500\)|NULL|戏曲成就|
|works|varchar\(500\)|NULL|代表戏曲作品|
|intro|text|NULL|富文本个人简介|
|create\_time|datetime|DEFAULT NOW\(\)|创建时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

#### （4）opera\_info 戏曲信息核心表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|戏曲 ID|
|opera\_name|varchar\(100\)|NOT NULL|戏曲名称|
|category\_id|bigint|NOT NULL|关联戏曲分类 ID|
|artist\_id|bigint|NOT NULL|关联艺术家 ID|
|history|text|NULL|发展历史|
|genre|varchar\(100\)|NULL|戏曲流派|
|cover\_img|varchar\(255\)|NULL|封面图片路径|
|audio\_url|varchar\(255\)|NULL|音频文件地址|
|video\_url|varchar\(255\)|NULL|视频文件地址|
|collect\_count|int|DEFAULT 0|收藏数量|
|click\_count|int|DEFAULT 0|点击浏览次数|
|content|text|NULL|戏曲详情富文本介绍|
|create\_time|datetime|DEFAULT NOW\(\)|创建时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

#### （5）opera\_material 戏曲资料表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|资料 ID|
|title|varchar\(100\)|NOT NULL|资料标题|
|cover\_img|varchar\(255\)|NULL|资料封面|
|summary|text|NULL|简介|
|content|text|NULL|资料正文|
|create\_time|datetime|DEFAULT NOW\(\)|发布时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

#### （6）opera\_comment 评论表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|评论 ID|
|opera\_id|bigint|NOT NULL|关联戏曲 ID|
|user\_id|bigint|NOT NULL|评论用户 ID|
|comment\_text|text|NOT NULL|评论内容|
|create\_time|datetime|DEFAULT NOW\(\)|评论时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

#### （7）opera\_collect 收藏中间表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|收藏 ID|
|user\_id|bigint|NOT NULL|用户 ID|
|opera\_id|bigint|NOT NULL|戏曲 ID|
|create\_time|datetime|DEFAULT NOW\(\)|收藏时间|
|UNIQUE KEY uk\_user\_opera\(user\_id,opera\_id\)|\-|联合唯一|一个用户同一戏曲仅收藏一次|

#### （8）banner 轮播图表

|字段名|类型|约束|说明|
|---|---|---|---|
|id|bigint|PK 自增|轮播 ID|
|img\_url|varchar\(255\)|NOT NULL|轮播图片|
|link\_url|varchar\(255\)|NULL|跳转链接|
|sort|int|DEFAULT 1|排序权重|
|status|tinyint|DEFAULT 1|1 启用 0 停用|
|create\_time|datetime|DEFAULT NOW\(\)|创建时间|
|is\_delete|tinyint|DEFAULT 0|逻辑删除|

## 四、后端 SpringBoot 模块分层设计

### 4\.1 项目包结构

```Plain Text
com.opera
├── OperaApplication.java 启动类
├── config                 // 配置类
│   ├── CorsConfig         // 跨域配置
│   ├── JwtConfig          // JWT认证配置
│   ├── MyBatisPlusConfig  // MyBatis-Plus分页、逻辑删除
│   ├── UploadConfig       // 文件上传配置
│   └── WebConfig          // 拦截器、静态资源映射
├── controller             // 控制层（接收前端请求）
│   ├── AdminController    // 管理员相关接口
│   ├── ArtistController   // 艺术家接口
│   ├── BannerController   // 轮播图接口
│   ├── CategoryController // 戏曲分类接口
│   ├── CollectController  // 收藏接口
│   ├── CommentController  // 评论接口
│   ├── MaterialController // 戏曲资料接口
│   ├── OperaInfoController// 戏曲信息接口
│   └── UserController     // 用户登录注册接口
├── entity                 // 数据库实体类（对应数据表）
│   ├── SysUser
│   ├── OperaArtist
│   ├── OperaCategory
│   ├── OperaInfo
│   ├── OperaMaterial
│   ├── OperaComment
│   ├── OperaCollect
│   └── Banner
├── mapper                 // Mapper层（MyBatis-Plus）
│   ├── SysUserMapper
│   ├── OperaArtistMapper
│   └── ...其他Mapper
├── service                // 业务层接口
│   ├── impl               // 业务层实现类
│   │   ├── SysUserServiceImpl
│   │   └── ...其他实现类
│   ├── SysUserService
│   ├── OperaArtistService
│   └── ...其他Service接口
├── util                   // 工具类
│   ├── JwtUtil            // Token生成解析
│   ├── ResultUtil         // 统一返回工具
│   ├── UploadUtil         // 文件上传工具
│   └── ValidUtil          // 表单校验工具
├── vo                     // 前后端交互VO（返回视图对象）
│   ├── LoginVO            // 登录返回token、用户信息
│   ├── PageVO             // 通用分页返回
│   ├── OperaChartVO       // 戏曲分类统计饼图数据
│   └── ...其他VO
├── dto                    // 请求入参DTO
│   ├── UserRegisterDTO    // 注册接收参数
│   ├── OperaQueryDTO      // 戏曲分页检索参数
│   └── ...其他DTO
├── interceptor            // JWT登录拦截器
└── exception              // 全局异常处理
    ├── GlobalExceptionHandler
    └── BusinessException  // 自定义业务异常
```

### 4\.2 核心分层职责

1. **Controller**：接收前端 axios 请求，参数接收、简单校验，调用 Service，返回统一 JSON 结果；

2. **Service**：核心业务逻辑，关联多表查询、事务处理（收藏、评论新增等）；

3. **Mapper**：基于 MyBatis\-Plus 实现单表 CRUD，自定义多表联查 SQL；

4. **Entity**：数据库实体，映射数据表字段；

5. **DTO**：接收前端复杂表单、检索条件；

6. **VO**：封装返回前端的数据，避免直接返回实体类泄露敏感字段；

7. **Interceptor**：拦截所有需要登录的接口，校验 JWT Token 有效性；

8. **GlobalException**：捕获所有异常，统一返回错误信息，不抛出原生报错页面。

### 4\.3 统一返回结果封装（ResultUtil）

所有接口返回固定格式 JSON：

```json
{
  "code": 200, // 200成功 500业务异常 401未登录/Token失效
  "msg": "操作成功",
  "data": {} // 业务数据（分页列表、详情、token等）
}
```

## 五、前端 Vue 项目结构设计

### 5\.1 前台用户端（浅色展示页面）

```Plain Text
src/front
├── main.js                 // 前台入口
├── App.vue
├── router/index.js         // 前台路由
│   ├── /login 登录页
│   ├── /register 注册页
│   ├── /index 首页
│   ├── /material 戏曲资料列表
│   ├── /operaDetail/:id 戏曲详情
│   └── /artist 艺术家页面
├── views                   // 页面组件
│   ├── Login.vue
│   ├── Register.vue
│   ├── Index.vue
│   ├── Material.vue
│   ├── OperaDetail.vue
│   └── Artist.vue
├── components              // 公共组件
│   ├── Header.vue 顶部导航
│   ├── Banner.vue 轮播图
│   ├── OperaCard.vue 戏曲卡片
│   └── AudioPlayer.vue 音频播放器
├── api                     // 前台接口请求
│   ├── user.js 登录注册接口
│   ├── opera.js 戏曲相关接口
│   ├── material.js 资料接口
│   └── collect.js 收藏评论接口
├── utils
│   ├── request.js axios封装（请求拦截携带token）
│   └── storage.js localStorage工具
└── assets 静态资源、样式
```

### 5\.2 后台管理端（深色管理系统）

```Plain Text
src/admin
├── main.js                 // 后台入口
├── App.vue
├── router/index.js         // 后台路由（侧边栏菜单对应路由）
├── views
│   ├── Dashboard.vue 仪表盘（ECharts饼图）
│   ├── user/ 系统用户管理页面
│   ├── banner/ 轮播图管理
│   ├── material/ 戏曲资料管理
│   ├── category/ 戏曲分类管理
│   ├── opera/ 戏曲信息列表、新增弹窗
│   └── artist/ 艺术家列表、新增弹窗
├── components
│   ├── Sidebar.vue 左侧菜单栏
│   ├── Tabs.vue 顶部标签页
│   ├── PageTable.vue 通用分页表格
│   ├── UploadImg.vue 图片上传组件
│   └── RichText.vue 富文本编辑器封装
├── api                     // 后台管理接口
│   ├── admin.js 管理员接口
│   ├── category.js 戏曲分类
│   ├── artist.js 艺术家CRUD
│   └── opera.js 戏曲信息管理
└── utils
    ├── request.js 后台axios请求
    └── echarts.js ECharts初始化封装
```

### 5\.3 核心前端功能说明

1. **Axios 请求拦截**：每次请求自动从 localStorage 读取 token 放入请求头 Authorization；响应拦截捕获 401 未登录，自动跳转登录页；

2. **文件上传**：el\-upload 组件上传图片 / 音频，后端接收文件返回访问路径，回填表单；

3. **ECharts 图表**：仪表盘请求后端统计接口，拿到各分类戏曲数量，渲染饼图；

4. **富文本编辑器**：艺术家新增页面 wangeditor，支持图文排版，提交时传递 HTML 文本至后端；

5. **音频播放**：HTML5 Audio 标签，传入后端返回音频 URL，实现播放控制；

6. **分页封装**：统一封装分页组件，传递 pageNum、pageSize 参数，后端 MyBatis\-Plus 分页返回总条数、当前页数据；

7. **弹窗表单**：新增 / 修改艺术家、戏曲信息使用 el\-dialog 弹窗，表单校验必填项，关闭弹窗清空表单。

## 六、核心业务流程

### 6\.1 用户登录流程

1. 用户输入账号密码，点击登录；

2. 前端 axios 携带账号密码请求后端`/user/login`；

3. 后端校验账号密码，匹配成功生成 JWT Token；

4. 返回 token \+ 用户角色（管理员 / 普通用户）；

5. 前端存储 token 至 localStorage，根据角色跳转页面：管理员跳后台仪表盘，普通用户跳前台首页；

6. 后续所有需要权限的接口自动携带 token。

### 6\.2 戏曲收藏流程

1. 登录用户进入戏曲详情页，点击收藏按钮；

2. 前端传递`userId、operaId`请求收藏接口；

3. 后端先查询`opera_collect`表是否存在该用户 \+ 戏曲记录；

    - 存在：删除记录，收藏数量 \- 1；

    - 不存在：新增收藏记录，收藏数量 \+ 1；

4. 更新`opera_info`表 collect\_count，返回最新收藏数，前端刷新展示。

### 6\.3 后台新增艺术家流程

1. 管理员进入艺术家列表，点击【新增】弹出弹窗；

2. 填写表单、上传头像、富文本编辑个人简介；

3. 点击提交，前端校验姓名字段非空；

4. 上传图片至后端，后端返回图片访问路径，连同表单字段一起提交；

5. 后端接收参数插入`opera_artist`表，返回成功提示；

6. 弹窗关闭，刷新艺术家列表。

### 6\.4 戏曲分类统计饼图流程

1. 管理员登录后台首页仪表盘；

2. 页面加载请求`/opera/countChart`统计接口；

3. 后端关联`opera_category`与`opera_info`分组统计每个分类戏曲数量；

4. 返回包含分类名称、数量、占比的 VO 集合；

5. 前端 ECharts 接收数据渲染饼图，悬浮显示分类信息。

## 七、系统部署方案

1. **数据库部署**：安装 MySQL8\.0，执行项目 sql 脚本创建库表，配置账号密码；

2. **后端部署**：Maven 打包 SpringBoot 项目为 jar 包，`java -jar opera.jar`启动；配置文件修改数据库连接、文件上传路径、JWT 密钥；

3. **前端部署**：前台、后台分别执行`npm run build`打包 dist 静态资源；

4. **Nginx 反向代理**：

    - 配置 /api 转发至后端 SpringBoot 端口；

    - 前台静态资源绑定域名根路径；

    - 后台管理页面分配 /admin 路由；

    - 配置静态图片、音频文件访问路径，设置跨域、缓存策略；

5. **文件存储**：本地服务器文件夹存储上传图片音频，Nginx 映射资源访问地址；生产环境可替换阿里云 OSS 对象存储。

## 八、系统扩展优化方向

1. **视频点播**：扩展戏曲视频流媒体播放，接入七牛云 / 阿里云视频点播；

2. **戏曲检索优化**：整合 Elasticsearch 实现全文检索，支持戏曲剧情、艺术家、流派模糊搜索；

3. **权限细分**：后台增加多角色权限（超级管理员、运营人员），菜单按钮级权限控制；

4. **移动端适配**：前台页面做响应式适配手机、平板；开发小程序端；

5. **数据导出**：后台支持戏曲、艺术家数据 Excel 批量导出；

6. **评论审核**：新增评论审核功能，管理员屏蔽违规评论；

7. **消息通知**：用户收藏戏曲更新、新评论推送站内消息；

8. **推荐算法**：基于用户收藏、浏览记录做个性化戏曲推荐。

## 九、项目总结

本系统基于 SpringBoot\+Vue 前后端分离架构，完整覆盖**戏曲文化线上展示**与**后台数字化管理**两大核心场景，兼顾普通用户的浏览、互动需求与管理员的全数据维护需求。系统模块化清晰，分层解耦，功能贴合戏曲文化传播业务，具备良好的可维护性与扩展性，能够有效实现戏曲资源数字化存档、线上传播推广，助力中华传统戏曲文化传承。
系统完整实现登录注册、首页推荐、戏曲检索、音频播放、收藏评论、艺术家展示、后台数据管理、可视化统计等全部需求，适配毕业设计、中小型文化场馆戏曲数字化平台落地使用。

