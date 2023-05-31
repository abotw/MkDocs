# ASP.NET Cheatsheet

- ASP.NET + IIS + Sql Server
- PHP + Apache + MySQL
- JSP + tomcat + MySQL

## Reference

- [Programming ASP .NET](https://www.oreilly.com/library/view/programming-asp-net/0596001711/)
- [ASP.NET项目开发 - 三门峡职业技术学院](https://www.icourse163.org/course/SMXPT-1002127015)
- [IIS: Internet Information Services](https://zh.wikipedia.org/zh-cn/%E7%B6%B2%E9%9A%9B%E7%B6%B2%E8%B7%AF%E8%B3%87%E8%A8%8A%E6%9C%8D%E5%8B%99)

## 1. 控件

### 1.1 什么是控件？

``` html
<asp:control_name ... ... />
```

### 1.2 控件可以怎么分类？

=== "Overall"
    <figure markdown>
        ![](./img/aa310919.webforms(en-us,vs.71).gif)     	<figcaption>Overall</figcaption>
    </figure>

=== "HTML Server Controls - HTML 服务器控件"
	HTML Server Controls = HTML Controls[runat=server]
	<figure markdown>
        ![](img/aa720135.htmlcontrols(en-us,vs.71).gif)     	<figcaption>HTML Server Controls Hierarchy</figcaption>
    </figure>

=== "Web Server Controls -  (Web)标准服务器控件"
    <figure markdown>
        ![](./img/aa310916.webcontrols(en-us,vs.71).gif)     	<figcaption>Web Server Controls Hierarchy</figcaption>
    </figure>
    
=== "HTML Controls HTML(浏览器)控件"
    ### ASP.NET Server Control v.s. HTML Element
    
    |ASP.NET Server Control|HTML Element|
    |--|--|
    |asp:TextBox|input type="text"|
    |asp:Button|input type="button"|
    |asp:CheckBox|input type="checkbox"|
    |asp:DropDownList|select option|
    |asp:HiddenField|input type="hidden"|
    |asp:HyperLink|a|
=== "User Controls"
=== "Custom Controls"

---

### 1.3 Properties in ASP.NET Server Controls 控件属性

* `CssClass` - 为标准服务器控件指定类选择器

### 1.4 基础控件

* Label 标签 - 用来显示提示信息

* TextBox - 接受用户输入的数据

  * `TextMode` - 将 TextBox 控件设置为单行文本框、多行文本框或密码框

    ``` html linenums=1
    <asp:TextBox ID="TextBox1" runat="server" TextMode="Password"></asp:TextBox>
    ```

    

* Button 按钮

  * **LinkButton**

    1. `PostBackURL` - 网页导航（优先执行）

    2. OnClick - 事件函数 - 触发服务器端事件

  * ImageButton

* **HyperLink**

  ``` html linenums="1"
  <asp:HyperLink ID="HyperLink1" runat="server">HyperLink</asp:HyperLink>
  ```

  * `NavigateUrl` - 设置目标网页

### 1.5 高级控件

#### 1.6.1 ImageMap

* HotSpot - 定义图像映射中的可点击区域
  * 圆形
  * 不规则图形
  * 矩形

#### 1.6.2 FileUpload 文件上传控件

文件上传

控件：`FileUpload`

1. 将图片直接存储在数据库中
2. 将图片放在服务器的文件系统里，将其文件路径保存在数据库中

删除文件

`File.Delete(Server.MapPath(filePath))`

File 类的命名空间

`using System.IO;`

Path 类

#### 1.6.3 Panel 容器

* BackImageUrl

#### 1.6.4 RadioButtonList 一组单选按钮

* Items - 所有的单选项
* SelectedItem - 返回用户的选中项

#### 1.6.5 Image v.s. img

如果想要通过服务器端代码来控制图片的显示，或者说，想要在模版页上控制图片的显示，则必须使用 Image 控件。

||Image|img|
|--|--|--|
|控件类型|Web Server Controls<br />标准服务器控件|HTML Controls/Tags<br />HTML(浏览器)控件/标签|
|链接图片地址的属性|ImageUrl|src|
|特点|能够识别`~`符号|只能使用相对地址|

#### SiteMapDataSource 站点地图数据源控件

* SiteMapDataSource控件是数据源控件，它可以从站点地图中提取数据，作为TreeView和Menu控件的数据源。

``` html linenums="1" hl_lines="2-3"
<asp:SiteMapDataSource
                       ID="SiteMapDataSource1"
                       runat="server" 
                       ShowStartingNode="false"
                       StartFromCurrentNode="false"
                       StartingNodeOffset="0"
                       ShowNodeLines="true" 
/>
```

??? SiteMapDataSource控件的常用属性
    - ID: 为控件指定唯一的标识符。
    - runat: 指定该控件在服务器端运行。
    - ShowStartingNode: 指定是否在菜单中显示起始节点。设置为false将隐藏起始节点，只显示其子节点。
    - StartFromCurrentNode: 指定是否从当前节点开始显示菜单项。设置为false将从根节点开始显示菜单项。
    - StartingNodeOffset: 指定起始节点的偏移量。可以使用正值或负值来指定相对于当前节点的偏移量。
    - ShowNodeLines: 指定是否显示节点之间的连接线。设置为true将显示节点之间的线条，以视觉上表示菜单项的层次结构。

#### 1.6.6 Menu 控件

- Menu控件不能自动绑定到站点地图，需要显式地将其绑定到 **SiteMapDataSource** 控件上，才能显示站点地图的节点。

``` html linenums="1" hl_lines="4-5"
<asp:Menu
          ID="Menu1"
          runat="server"
          DataSourceID="SiteMapDataSource1" 
          StaticDisplayLevels="2"
          Orientation="Horizontal"
          CssClass="menu" 
          StaticSubMenuIndent="16px"
          DynamicHorizontalOffset="0" 
          DynamicVerticalOffset="0"
          RenderingMode="List"
>
</asp:Menu>
```

??? Menu控件的常用属性
	- `ID`: 为控件指定唯一的标识符。
	- `runat`: 指定该控件在服务器端运行。
	- **`DataSourceID`: 指定控件使用的数据源，可以是一个`SiteMapDataSource`控件或其他支持的数据源。**
	- **`StaticDisplayLevels`: 指定菜单在静态显示模式下要显示的级数。例如，设置为2将显示两级子菜单。**
	- `Orientation`: 指定菜单的方向。可以设置为`Horizontal`（水平）或`Vertical`（垂直）。
	- `CssClass`: 指定应用于菜单的CSS类名称，用于自定义菜单的外观样式。
	- `StaticSubMenuIndent`: 指定静态子菜单相对于父菜单项的缩进量。可以使用像素（px）或其他CSS长度单位。
	- `DynamicHorizontalOffset`: 指定动态菜单项相对于鼠标指针的水平偏移量。
	- `DynamicVerticalOffset`: 指定动态菜单项相对于鼠标指针的垂直偏移量。
	- `RenderingMode`: 指定菜单的呈现模式。可以设置为`List`（默认）或`Table`，决定菜单项的呈现方式。

#### 1.6.7 TreeView 控件

- TreeView控件由**TreeNode**对象构成，所以只要向TreeView控件的Items集合属性中添加该对象就可以构造简单的TreeView控件。

``` html
<asp:TreeView ID="TreeView1" runat="server"> <!-- (1) -->
    <Nodes>
        <!-- Add your tree nodes here -->
    </Nodes>
    <NodeStyle Font-Names="Tahoma" Font-Size="8pt" ForeColor="Black" HorizontalPadding="2px" NodeSpacing="0px" VerticalPadding="2px" />
    <ParentNodeStyle Font-Bold="False" />
    <SelectedNodeStyle BackColor="#B5B5B5" Font-Underline="False" HorizontalPadding="0px" VerticalPadding="0px" />
</asp:TreeView>
```
1. 

#### 1.6.8 SiteMapPath 站点地图路径控件

1. 来呈现网站的站点地图导航路径
2. 只需要在页面中插入一个 **SiteMapPath** 控件，在浏览页面时它就会**直接自动读取**位于站点根目录下的站点地图 (Web.sitemap) 文件。

``` html linenums="1"
<asp:SiteMapPath ID="SiteMapPath1" runat="server" CssClass="breadcrumb" NodeStyle-CssClass="breadcrumbNode" CurrentNodeStyle-CssClass="currentNode" PathSeparator="&gt;">
    <PathSeparatorTemplate>
        <span class="separator">&gt;</span>
    </PathSeparatorTemplate>
    <NodeTemplate>
        <asp:HyperLink runat="server" ID="NodeLink" NavigateUrl='<%# Eval("Url") %>' Text='<%# Eval("Title") %>'></asp:HyperLink>
    </NodeTemplate>
    <RootNodeTemplate>
        <asp:HyperLink runat="server" ID="RootNodeLink" NavigateUrl='<%# Eval("Url") %>' Text='<%# Eval("Title") %>'></asp:HyperLink>
    </RootNodeTemplate>
</asp:SiteMapPath>
```

??? SiteMapPath控件常用属性
    - ID="SiteMapPath1"：控件的唯一标识符，用于在代码中引用控件。
    - CssClass="breadcrumb"：控件的 CSS 类名，用于样式控制。
    - NodeStyle-CssClass="breadcrumbNode"：节点样式的 CSS 类名，用于自定义节点的样式。
    - CurrentNodeStyle-CssClass="currentNode"：当前节点样式的 CSS 类名，用于自定义当前节点的样式。
    - PathSeparator="&gt;"：路径分隔符，这里使用大于符号(>)作为分隔符。
    - PathSeparatorTemplate：路径分隔符的模板，可以自定义分隔符的 HTML。
    - NodeTemplate：节点模板，用于呈现非当前节点的链接。
    - RootNodeTemplate：根节点模板，用于呈现站点地图的根节点。

### 1.6 数据验证控件

#### tree this

* Label
  * BaseValidator
    * BaseCompareValidator
      * CompareValidator 比较验证
    * CustomValidator
    * RangeValidator 范围验证
    * RegularExpressionValidator 正则表达式验证
    * RequiredFieldValidator

#### [Button.CausesValidation Property](https://learn.microsoft.com/en-us/dotnet/api/system.web.ui.webcontrols.button.causesvalidation?view=netframework-4.8.1)

禁止该按钮进行网页验证：Gets or sets a value indicating whether validation is performed when the [Button](https://learn.microsoft.com/en-us/dotnet/api/system.web.ui.webcontrols.button?view=netframework-4.8.1) control is clicked.

### 1.7 用户控件

#### 1. 用户控件是什么？

##### 用户控件文件有如下特点：

1. 用户控件中没有“@ page”指令，而是包含“@ Control”指令，该指令对配置及**其他**的属性进行定义。
2. 用户控件中没有html,body,或form元素，这些元素不许位于宿主中。

#### 2. 怎么创建用户控件？

1. Contrl + Shift + A 创建 Web Forms User Control: `WebUserControl1.ascx`

2. 初始代码

   ```html
   <%@ Control Language="C#" AutoEventWireup="true" CodeBehind="WebUserControl1.ascx.cs" Inherits="WebApplication.WebUserControl1" %>
       
   <%@ Control Language="C#" AutoEventWireup="true" CodeFile="Head.ascx.cs" Inherits="Head" %>
   ```

   - %@
     - `Control` 指令
     - `Language`: C#
     - `AutoEventWireup`: true
     - `CodeBehind` / `CodeFile`: path/to/cs/file
     - `Inherits`: `WebApplicationName.WebUserControlName` / `ControlName`

   ```C#
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using System.Web.UI;
   using System.Web.UI.WebControls;
   
   namespace WebApplication
   {
       public partial class WebUserControl1 : System.Web.UI.UserControl
       {
           protected void Page_Load(object sender, EventArgs e)
           {
   
           }
       }
   }
   ```



#### 3. 怎么使用用户控件？

##### 声明方式使用用户控件

注册用户控件（在页面中）

``` html
<%@ Register Src="Head.ascx" TagPrefix="uc1" TagName="Head" %>
```

* %@
  * Register 指令
  * Src source
  * TagPrefix
  * TagName

注册用户控件（在 `Web.config` 中）

```xml
<controls>
   <add src="~/Controls/Content.ascx" tagPrefix="uc1" tagName="Content"/>
</controls>
```

调用用户控件

``` html
<TagPrefix:TagName ID="TagName1" runat="server" />
```

#### 以编程方式加载用户控件

　　（6）可以使用System.WEB.UI.Page类的LoadControl方法以编程方式创建用户控件。用户控件的类型由ASP.NET运行库决定，遵循约定文件名_扩展名。

可以使用Page类的LoadControl方法以编程方式载入用户控件。

```
Content Content2 = (Content)this.LoadControl("Content.ascx");
Content2.hif = "123";
this.Controls.Add(Content2);
```

#### 4. 用户控件小结

| Name                   | Desc                                                        | Extension            | 指令名称 |
| ---------------------- | ----------------------------------------------------------- | -------------------- | -------- |
| Web Forms User Control | an ASP.NET server control created using the visual designer | WebUserControl1.ascx | Control  |
| Web Form               | a form for Web Application                                  | WebForm1.aspx        | Page     |
| Master Page            | A Master Page for Web Application                           | MasterPage.master    | Master   |

#### Reference

* [ASP.NET Web Forms Tutorial: User Controls Examples](https://www.guru99.com/asp-net-controls-webforms.html)
* [ASP.NET（99）：ASP.Net用户控件的使用](https://www.cnblogs.com/springsnow/p/9433910.html)

### References

* [ASP.NET Server Controls Hierarchy](https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-1.1/aa310919(v=vs.71))

## 2. 母版页技术

<figure markdown> 
    ![Image title](img/ASP.NET-Cheatsheet/master-page-01.png){ width="500" }
    <figcaption>The Requested Page's Markup is Fused into the Master Page</figcaption>
</figure>

### 2.0 Reference

* [ASP.NET 3.5 - Web Forms Master Pages](https://learn.microsoft.com/en-us/aspnet/web-forms/overview/older-versions-getting-started/master-pages/)

### 2.1 概述

1. 母版页的扩展名：`.master`
2. 母版页的页首指令：`<%@ Master %>`
3. 在母版页里显示图片、超链接时，使用 ASP.NET 标准服务器控件。

### 2.2 如何创建一个母版页？

### 2.3 如何使用母版页？

![](./img/ASP.NET-Cheatsheet/wtxbf3hh.vbmasterpages_merge(en-us,vs.100).gif)

``` html
<%@ Master %>

<asp:contentplaceholder runat="server" id="Main" />
<asp:contentplaceholder runat="server" id="Footer" />
```

``` html
<%@ Page MasterPageFile="A.master" %>
<asp:Content runat="server" ContentPlaceHolderId="main">
	Content here.
  内容页的所有私有内容都必须放在该控件中，如果将任何内容放在该控件之外，都会引发异常。
</asp:Content>
<asp:Content runat="server" ContentPlaceHolderId="Footer">
  Content here.
</asp:Content>
```

* `MasterPageFile` - 内容页通过该属性指定母版页的虚拟路径，从而与母版页建立起联系。
* `ContentPlaceHolderId` - 用来对应于母版页的 ContentPlaceHolder 控件。

web.config: 

``` xml linenums="1"
<system.web>
	<pages MasterPageFile="MasterPageName" />
</system.web>
```

## 3. 常用对象

- ASP.NET 的内置对象是 .NET 框架封装的类实现的，不需要使用 new 关键字创建，可以在程序中任何地方调用。

### 1. Page

asp.net 为网页对象 Page 提供了系列事件，事件在网页加载时，顺序触发，其顺序为 Page_Init 、Page_Load 和 Page_Unload。
Page_Init 事件是页面首次加载时触发，Page_Load 事件是每次页面加载都会触发。

Page.IsPostBack 属性返回网页是否是首次加载，属性值为 false 时，表示该网页是首次被加载。

Page对象表示当前网页对象，Page.IsValid属性用来表示该网页是否通过了所有验证。

``` C#
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        // 在 Page_Load 事件中进行初始化和设置
        lblMessage.Text = "欢迎来到学习 Page 的示例页面！";
    }
}
```

### 2. Response

Response对象主要用来向浏览器输出数据。

``` C# linenums="1" hl_lines="10 16 22"
protected void btnDownload_Click(object sender, EventArgs e)
{
    // 设置响应的内容类型
    Response.ContentType = "application/pdf";
    // 设置响应的文件名
    Response.AddHeader("Content-Disposition", "attachment; filename=sample.pdf"); // (1)
    // 发送文件给客户端
    Response.TransmitFile(Server.MapPath("~/Files/sample.pdf")); // (2)
    // 结束响应，确保不再发送任何内容
    Response.End();
}

protected void btnRedirect_Click(object sender, EventArgs e)
{
    // 重定向到其他页面
    Response.Redirect("https://www.example.com");
}

protected void btnWrite_Click(object sender, EventArgs e)
{
    // 向响应中写入文本，向浏览器输出字符串
    Response.Write("这是来自 Response 对象的文本输出！");
}

```

### 3. Request

Request对象主要用来读取浏览器数据。

``` C# linenums="1" hl_lines="6 18"
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        // 获取页面 URL 中的查询字符串参数的值
        string id = Request.QueryString["id"];
        if (!string.IsNullOrEmpty(id))
        {
            lblId.Text = "ID：" + id;
        }
        // 获取表单提交的值
        string name = Request.Form["txtName"];
        if (!string.IsNullOrEmpty(name))
        {
            lblName.Text = "名称：" + name;
        }
        // 获取客户端的 IP 地址
        string ipAddress = Request.UserHostAddress;
        lblIpAddress.Text = "IP 地址：" + ipAddress;
    }
}

```

### 4. Server

Server 对象用来提供访问服务器的方法和属性。

1. `string physicalPath = Server.MapPath(relativePath)` - 将相对/虚拟路径转换为物理路径
  * 相对于当前页面的虚拟路径
  * 相对于应用程序根目录的虚拟路径
2. `Server.MachineName()`
3. `Server.Transfer()` - 可以将请求转发到一个新的 ASP.NET 页面，新页面会处理请求并生成响应，但在处理完毕后会将控制权返回给当前页面。
4. `Server.ScriptTimeout` - 用来设置访问网页超时时间

``` C# linenums="1" hl_lines="10 13"
protected void btnServerInfo_Click(object sender, EventArgs e)
{
    // 获取服务器的主机名
    string serverName = Server.MachineName;
    lblServerName.Text = "服务器名：" + serverName;
    // 获取当前页面的虚拟路径
    string pageVirtualPath = Request.Url.AbsolutePath;
    lblPageVirtualPath.Text = "页面虚拟路径：" + pageVirtualPath;
    // 获取当前页面的文件/物理路径
    string pagePath = Server.MapPath(Request.Url.AbsolutePath); // (1)
    lblPagePath.Text = "页面路径：" + pagePath;
    // 获取应用程序的物理路径
    string appPath = Server.MapPath("~");
    lblAppPath.Text = "应用程序路径：" + appPath;
}

```

1. 使用 Server.MapPath 方法可以**将相对/虚拟路径转换为物理路径**。

### 5. Cookie

Cookie 是在客户端存储的，并且可以被用户修改或删除。因此，在处理敏感信息时，需要谨慎使用 Cookie，并进行适当的验证和保护。

``` C# linenums="1" hl_lines="4 9 11 20 41-42"
protected void btnSetCookie_Click(object sender, EventArgs e)
{
    // 创建一个新的 Cookie 对象
    HttpCookie cookie = new HttpCookie("UserInfo"); // (1)
    // 设置 Cookie 的值
    cookie["UserName"] = "JohnDoe"; // (2)
    cookie["Email"] = "johndoe@example.com";
    // 设置 Cookie 的过期时间
    cookie.Expires = DateTime.Now.AddDays(7); // (3)
    // 将 Cookie 添加到响应中
    Response.Cookies.Add(cookie); // (4)
}

protected void btnGetCookie_Click(object sender, EventArgs e)
{
    // 检查是否存在指定名称的 Cookie
    if (Request.Cookies["UserInfo"] != null) // (5)
    {
        // 获取 Cookie 对象
        HttpCookie cookie = Request.Cookies["UserInfo"]; // (6)
        // 获取 Cookie 的值
        string userName = cookie["UserName"]; // (7)
        string email = cookie["Email"];
        // 在标签中显示 Cookie 的值
        lblUserName.Text = "用户名：" + userName; // (8)
        lblEmail.Text = "邮箱：" + email;
    }
    else
    {
        lblUserName.Text = "Cookie 不存在";
        lblEmail.Text = "";
    }
}

protected void btnDeleteCookie_Click(object sender, EventArgs e)
{
    // 删除指定名称的 Cookie
    if (Request.Cookies["UserInfo"] != null) // (9)
    {
        HttpCookie cookie = new HttpCookie("UserInfo"); // (10)
        cookie.Expires = DateTime.Now.AddDays(-1); // (11)
        Response.Cookies.Add(cookie); // (12)
    }
}

```

1. 创建了一个名为"UserInfo"的新 Cookie 对象
2. 设置 Cookie 的值，设置了两个键值对，分别是"UserName"和"Email"
3. 通过设置 Expires 属性将 Cookie 的过期时间设置为当前时间加上七天
4. 使用 Response.Cookies.Add 方法将 Cookie 添加到响应中，以便将其发送给客户端
5. 检查是否存在名为 "UserInfo" 的 Cookie
6. 如果存在，我们使用 Request.Cookies["UserInfo"] 获取 Cookie 对象
7. 通过键值对的方式获取 Cookie 的值
8. 在标签中显示 Cookie 的值
9. 首先检查是否存在名为 "UserInfo" 的 Cookie
10. 如果存在，我们创建一个新的同名 Cookie 对象
11. 将其过期时间设置为当前时间减去一天
12. 使用 Response.Cookies.Add 方法将 Cookie 添加到响应中，以便将其发送给客户端

### 6. Session

当用户在浏览器中访问你的网站时，ASP.NET 会为每个用户创建一个唯一的 Session。在用户的整个会话期间，你可以使用 Session 来存储和检索用户特定的数据。

确保在你的应用程序中适当地处理 Session 变量的生命周期和内存占用，以避免潜在的性能问题和资源浪费。

``` C# linenums="1" hl_lines="6 13"
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        // 设置 Session 变量
        Session["UserName"] = "JohnDoe"; // (1)
    }
}

protected void btnGetSession_Click(object sender, EventArgs e)
{
    // 获取 Session 变量的值
    string userName = Session["UserName"] as string;
    // 在标签中显示 Session 变量的值
    lblUserName.Text = userName;
}

```

1. 将一个名为 "UserName" 的 Session 变量设置为 "JohnDoe"

### 7. Application

Application 对象提供了一种在整个应用程序范围内共享数据和状态的机制。它可以存储和访问应用程序级别的变量，可以在不同的页面和会话之间共享数据。

请注意，Application 对象是在应用程序级别的，而不是在单个用户或会话级别的。因此，在使用 Application 对象时需要注意并发访问的线程安全性。

``` C# linenums="1" hl_lines="1 7 15 21"
protected void Application_Start(object sender, EventArgs e)
{
    // 在应用程序启动时执行一些初始化代码
    Application["TotalVisitors"] = 0; // (1)
}

protected void Application_BeginRequest(object sender, EventArgs e)
{
    // 在每个请求开始时执行的代码
    int totalVisitors = (int)Application["TotalVisitors"]; // (2)
    totalVisitors++; // （3）
    Application["TotalVisitors"] = totalVisitors; // (4)
}

protected void Application_EndRequest(object sender, EventArgs e)
{
    // 在每个请求结束时执行的代码
    // 可以在这里处理一些清理或日志记录等操作
}

protected void Application_End(object sender, EventArgs e)
{
    // 在应用程序关闭时执行的代码
    // 可以在这里进行一些资源释放等操作
}

```

1. 将名为 "TotalVisitors" 的应用程序变量初始化为 0。
2. 获取应用程序变量 "TotalVisitors" 的当前值。
3. 将其递增。
4. 将新的值保存回应用程序变量中。

## 🧭 网站导航

### 站点地图

- **站点地图**用于表示网站页面之间的导航关系，该关系依赖于计算机文件系统中所存储的页面之间的物理关系。

``` xml linenums="1"
<?xml version="1.0" encoding="utf-8" ?>
<!-- Web.sitemap -->
<siteMap xmlns="http://schemas.microsoft.com/AspNet/SiteMap-File-1.0">
  <siteMapNode url="~/{URL}" title="{TITLE}" description="{DESCRIPTION}">
    {CHILD_NODES}
  </siteMapNode>
</siteMap>

<!-- {URL} (1) -->
<!-- {TITLE} (2) -->
<!-- {DESCRIPTION} (3) -->
<!-- {CHILD_NODES} (4) -->
```

1. `{URL}`: 节点的URL地址，替换为实际的URL。
2. `{TITLE}`: 节点的标题，替换为实际的标题。
3. `{DESCRIPTION}`: 节点的描述信息，替换为实际的描述。
4. `{CHILD_NODES}`: 子节点的占位符，在这里插入子节点的代码。

??? 站点地图代码结构树
    * siteMap: 根元素，定义了整个站点地图的结构。
    	* siteMapNode: 表示一个网站地图节点，可以包含子节点或子元素。
    		* 属性：
    			* url: 节点的URL地址。
    			* title: 节点的标题。
    			* description: 节点的描述信息。
    		* 子节点：
    			* siteMapNode: 表示当前节点的子节点。
    				* 属性：
    					* url: 节点的URL地址。
    					* title: 节点的标题。
    					* description: 节点的描述信息。

## ❌ 错误记录

### WebForms UnobtrusiveValidationMode 需要“jquery”ScriptResourceMapping。请添加一个名为 jquery (区分大小写)的 ScriptResourceMapping。

- [【.NET】请添加一个名为jquery（区分大小写）的ScriptResourceMapping，这怎么办呢？](https://blog.csdn.net/cxh6863/article/details/80870255)
