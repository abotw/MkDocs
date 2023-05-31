# 基于 ASP.NET 3.5 使用 DetailView 控件

## Step 1: 声明静态实体类

在页面的代码后台（.aspx.cs）文件中，声明一个静态的 Employee 类，用于存储员工的信息。

``` c#
public class Employee
{
    public int EmployeeID { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Title { get; set; }
}
```

## Step 2 : 添加 DetailsView 控件

更新 DetailsView 控件的模板，以显示 Employee 类的属性。

打开 ASP.NET 页面（.aspx），将以下代码添加到页面的主体中：

```HTML
<asp:DetailsView ID="DetailsView1" runat="server" AutoGenerateRows="False">
    <Fields>
        <asp:BoundField DataField="EmployeeID" HeaderText="Employee ID" />
        <asp:BoundField DataField="FirstName" HeaderText="First Name" />
        <asp:BoundField DataField="LastName" HeaderText="Last Name" />
        <asp:BoundField DataField="Title" HeaderText="Title" />
    </Fields>
</asp:DetailsView>
```

## Step 3: 使用 DetailView 控件绑定静态数据

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace DetailViewControl
{
    public class Employee
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Title { get; set; }
    }

    public partial class DetailViewControl : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            // 在没有数据库的情况下使用静态数据来学习 DetailView 控件的使用
            if (!IsPostBack)
            {
                // 创建一个 Employee 对象
                Employee employee = new Employee
                {
                    EmployeeID = 1,
                    FirstName = "John",
                    LastName = "Doe",
                    Title = "Developer"
                };
                List<Employee> employees = new List<Employee>();
                employees.Add(employee);
                // 将 Employee 对象绑定到 DetailView 控件
                DetailsView1.DataSource = employees;
                DetailsView1.DataBind();
            }
        }
    }

}
```

## Step 4: 创建数据库、数据库表、插入数据

在数据库中创建一个表来存储员工的信息。

``` sql
CREATE DATABASE TEMP;
USE TEMP;
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Title VARCHAR(50)
);
```

在 "Employees" 表中插入一些示例数据，以便在 DetailView 控件中显示。

``` SQL
INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (1, 'John', 'Doe', 'Developer');

INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (2, 'Jane', 'Smith', 'Manager');

INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (3, 'Mark', 'Johnson', 'Sales Representative');
```

## Step 5: 配置连接字符串

在 Web.config 文件中添加数据库连接字符串。打开 Web.config 文件，并在 <configuration> 标签下添加以下内容：

```xml
<connectionStrings>
    <add name="MyConnectionString" connectionString="Your Connection String" providerName="System.Data.SqlClient" />
</connectionStrings>
```

## Step 6: 连接数据库、获取数据和绑定数据

现在，我们需要在页面的代码后台（.aspx.cs）文件中连接到数据库并获取数据，然后绑定数据到 DetailView 控件。在页面的代码后台文件中，添加以下代码：

```C#
protected void GetDataByDB()
{
    if (!IsPostBack)
    {
        // string connectionString = "Integrated Security=SSPI;Persist Security Info=False;Initial Catalog=MyTempDB;Data Source=FREEBIRD\\SQLEXPRESS"; // 手动配置连接字符串
        string connectionString = WebConfigurationManager.ConnectionStrings["MyConnectionString"].ConnectionString; // 获取 Web.config 中的连接字符串

        string query = "SELECT EmployeeID, FirstName, LastName, Title FROM Employees WHERE EmployeeID = @EmployeeID";

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            using (SqlCommand command = new SqlCommand(query, connection))
            {
                command.Parameters.AddWithValue("@EmployeeID", 3);

                // UseSqlDataAdapter(command);
                UseSqlDataReader(connection, command);
            }
        }
    }
}

protected void UseSqlDataAdapter(SqlCommand command)
{
    using (SqlDataAdapter adapter = new SqlDataAdapter(command))
    {
        DataTable dataTable = new DataTable();
        adapter.Fill(dataTable);

        if (dataTable.Rows.Count > 0)
        {
            DetailsView1.DataSource = dataTable;
            DetailsView1.DataBind();
        }
    }
}

protected void UseSqlDataReader(SqlConnection connection, SqlCommand command)
{
    connection.Open();
    SqlDataReader reader = command.ExecuteReader();

    if (reader.HasRows)
    {
        reader.Read();
        Employee employee = new Employee
        {
            EmployeeID = reader.GetInt32(0),
            FirstName = reader.GetString(1),
            LastName = reader.GetString(2),
            Title = reader.GetString(3)
        };

        List<Employee> employees = new List<Employee>();
        employees.Add(employee);

        DetailsView1.DataSource = employees;
        DetailsView1.DataBind();
    }

    reader.Close();
    connection.Close();
}
```

