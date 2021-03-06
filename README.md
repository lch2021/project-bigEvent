# bigEvent 大事件项目文档
##1.项目介绍：

- 大事件是一个具有前、后台的**文章内容管理网站**
  - 前台：是用来给用户查看的页面，只能用于**数据的展示**
  - 后台：是用来给网站管理员使用的页面，可以**进行网站数据管理（增删改查）**

##2.后台功能说明

## 登录页面功能

- 基础步骤：
  - 1 对提交按钮进行处理
    - 方式1：阻止默认的提交行为   方式2：修改为普通按钮
  - 2 给按钮设置点击事件
  - 3 检测两个输入框内容是否为空
  - 4 如果内容检测通过，将内容发送给接口进行检测
  - 5 接收响应内容
    - 5.1 成功时跳转到index.html
    - 5.2 失败时进行错误提示
- 使用boot的模态框对提示内容进行美化
  - 在boot文档中找到js插件，找到模态框部分
  - 复制结构到html中，并进行内容修改
  - 替换代码中的alert，更换为模态框的modal方法
- **使用config.js统一管理接口地址**
  - 作用：后期维护时，接口地址变化可以在config.js中统一修改，无需到每个html文件中操作
  - 操作步骤：
    - 在根目录下创建tools目录，并新建config.js，例如/web_back/tools/config.js
    - 通过全大写、使用_连接的变量名进行接口地址管理
      - 将基地址使用变量统一保存，并拼接其他接口地址
    - 在页面例如login.html中引入config.js并将原始接口地址替换
- **使用user.js统一管理用户接口的ajax操作：**
  - 作用：后期维护时，接口使用方式变化可以在user.js中管理
  - 操作步骤：
    - 在tools目录中创建user.js，例如 /web_back/tools/user.js
    - 创建对象user，设置login方法
    - 设置options参数，需要传入data/success/fail属性
      - data代表请求参数
      - success保存成功登录时的操作
      - fail保存登录失败时的操作
    - 将login.html中的$.ajax()移动到user.login中
      - 并在login.html中原始位置调用user.login
      - 进行传参(写法见login.html代码)

## 首页功能

- 退出功能
  - 退出按钮点击事件
  - 使用**confirm()**询问是否确认退出
    - confirm()类似alert()与prompt()，是提示框效果
      - 具有确定和取消按钮，**点击确定返回true，点击取消返回false**
  - 请求接口
  - 对响应进行处理
    - 退出成功，跳转登录页
  - 将请求操作提取到user.js中(步骤同登录功能，见小节2.1
- 用户基本信息展示功能
  - 页面加载时发送请求，获取用户的基本信息
  - 将图片地址设置到左侧和右侧顶部头像区域
  - 将用户名展示到左侧
  - 将请求操作提取到user.js中
- 首页的iframe标签介绍
  - iframe标签用来进行页面内的小页面引入操作
    - **通过设置iframe标签的src可以修改要展示的页面**
  - 首页中设置了iframe用来进行其他子页面的演示
    - 查看功能使用index.html，但是功能书写在每个子页面中即可

## 用户页功能

- 用户的信息获取与展示
  - 请求用户的详细信息
    - 给表单中的元素设置与相应数据相同的id，方便获取
  - 将信息展示在页面中即可
- 用户的信息编辑功能
  - 检测是否完整填写表单
    - 输入框检测val()，文件域检测是否选取文件
      - **文件检测，使用DOM对象的files属性，进行length检测**
        - length为0，说明没有选择文件
  - 给表单中的元素设置name属性，否则无法提交
    - 进行提交按钮处理
      - 将数据发送给服务端处理（使用FormData即可）
        - 传入参数必须为DOM对象形式的form标签
  - 提交到服务端进行编辑
    - 提交formdata时，给$.ajax()设置两个属性
      - contentType: false
      - processData: false
    - 提交成功将父页面index.html跳转到login.html
      - 使用**window.parent**属性获取父页面的window对象
  - **图片的本地预览**
    - 使用change事件监测用户的文件选择操作
    - 通过 **URL.createObjectURL()**进行本地图片地址获取
      - URL是window对象的属性
      - 用户在本地选择的图片地址我们不可能提前知道
      - 使用方式：
        - **URL.createObjectURL(文件域的files中的文件信息)**
          - 例如：URL.createObjectURL(iptFile.files[0]);   // 实例中iptFile为文件域，DOM对象
      - 返回值是浏览器自动生成的**临时图片地址，可以设置在src中查看图片**
- 优化：将接口功能提取到user.js与config.js中

## 文章分类页面功能

- 分类数据展示功能
      - 发送请求，获取数据
  - 使用模板引擎进行结构生成
        - 设置模板
    - 调用模板方法，将数据和模板结合得到要生成的结构字符串
    - 生成到页面中即可
- 分类数据新增功能
  - 点击新增按钮，进行内容检测
  - 填写完毕，发送请求
  - 新增成功后，调用**location.reload()**刷新页面(iframe中的小区域)
- 分类数据编辑功能
  - 在模态框中设置一个提交编辑按钮，用来进行编辑操作
  - 进行操作效果处理：
    - 点击新增，将提交编辑按钮隐藏，点击编辑，将新增按钮
  - 在点击表格中的编辑按钮时
    - 需要获取到要编辑的数据数据的id
      - 可以在创建结构时给编辑按钮添加data-id属性保存id
      - 为了方便获取编辑按钮，添加了edit类名
      - 在模态框中设置**隐藏域**，用来保存编辑的id
        - 隐藏域是表单域中的一种，特点是隐藏，一般用于展示无需用户查看的数据
          - 格式:   **\<input type="hidden" value="示例内容">**
    - 将当前要编辑的数据内容取出，填写到表单中
      - 使用到一些jQuery的选择器，回顾一下用法
        - **parents(selector)** 获取祖先元素，传入selector用于筛选，例如'tr'，表示选择外部tr
        - **children(selector)** 获取子元素
        - **eq(index)** 根据索引获取一组jQuery对象中的某个，index为索引值
        - siblings(selector)  获取同级元素
  - 点击提交编辑，检测表单内容内容是否为空并发送请求
    - 编辑成功，使用location.reload()刷新页面
- 分类数据删除功能
  - 点击删除按钮，获取data-id
  - 将data-id发送给接口进行删除操作
    - 删除成功，使用location.reload()刷新页面
- **设置article.js用于提取文章功能的接口操作**
  - 代码中演示article.getCate()功能设置方式