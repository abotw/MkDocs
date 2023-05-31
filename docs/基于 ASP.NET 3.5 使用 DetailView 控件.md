# 基于 ASP.NET 3.5 使用 DetailView 控件

## Step : 打开 ASP.NET 页面（.aspx），将以下代码添加到页面的主体中

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

## Step 1: 在页面的代码后台（.aspx.cs）文件中，声明一个静态的 Employee 类，用于存储员工的信息

```C#
public class Employee
{
    public int EmployeeID { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Title { get; set; }
}
```

## Step 2: 在 Page_Load 事件中，创建一个 Employee 对象，并将其绑定到 DetailView 控件

这个示例演示了如何在没有数据库的情况下使用静态数据来学习 DetailView 控件的使用。你可以根据需要修改静态数据或添加更多的 Employee 对象来显示多个记录。

```C#
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        Employee employee = new Employee
        {
            EmployeeID = 1,
            FirstName = "John",
            LastName = "Doe",
            Title = "Developer"
        };

        List<Employee> employees = new List<Employee>();
        employees.Add(employee);

        DetailsView1.DataSource = employees;
        DetailsView1.DataBind();
    }
}

```

## Step 3: 更新 DetailsView 控件的模板，以显示 Employee 类的属性

```C#
<asp:DetailsView ID="DetailsView1" runat="server" AutoGenerateRows="False">
    <Fields>
        <asp:BoundField DataField="EmployeeID" HeaderText="Employee ID" />
        <asp:BoundField DataField="FirstName" HeaderText="First Name" />
        <asp:BoundField DataField="LastName" HeaderText="Last Name" />
        <asp:BoundField DataField="Title" HeaderText="Title" />
    </Fields>
</asp:DetailsView>

```

## Step : 在 Web.config 文件中添加数据库连接字符串。打开 Web.config 文件，并在 <configuration> 标签下添加以下内容：

```xml
<connectionStrings>
    <add name="MyConnectionString" connectionString="Your Connection String" providerName="System.Data.SqlClient" />
</connectionStrings>
```

## Step 1: 创建数据库表

在数据库中创建一个表来存储员工的信息。

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Title VARCHAR(50)
);
```

## Step 2: 插入数据

在 "Employees" 表中插入一些示例数据，以便在 DetailView 控件中显示。

```sql
INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (1, 'John', 'Doe', 'Developer');

INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (2, 'Jane', 'Smith', 'Manager');

INSERT INTO Employees (EmployeeID, FirstName, LastName, Title)
VALUES (3, 'Mark', 'Johnson', 'Sales Representative');
```

## Step 3: 在页面的代码后台（.aspx.cs）文件中

现在，我们需要在页面的代码后台（.aspx.cs）文件中连接到数据库并获取数据。在页面的代码后台文件中，添加以下代码：

1. 连接到数据库并获取数据
2. 绑定数据到 DetailView 控件

这个示例假设你有一个名为 "Employees" 的表，其中包含有关员工的数据，并且你想要显示 ID 为 1 的员工的详细信息。

```C#
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        string connectionString = "Your Connection String"; // 替换为你自己的数据库连接字符串
        string query = "SELECT EmployeeID, FirstName, LastName, Title FROM Employees WHERE EmployeeID = @EmployeeID";

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            using (SqlCommand command = new SqlCommand(query, connection))
            {
                command.Parameters.AddWithValue("@EmployeeID", 1); // 替换为你想要显示的员工的ID

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
        }
    }
}
```

```C#
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        string connectionString = "Your Connection String"; // 更改为你自己的数据库连接字符串
        string query = "SELECT EmployeeID, FirstName, LastName, Title FROM Employees WHERE EmployeeID = @EmployeeID"; // 更改为你自己的查询语句

        SqlConnection connection = new SqlConnection(connectionString);
        SqlCommand command = new SqlCommand(query, connection);
        command.Parameters.AddWithValue("@EmployeeID", 1); // 更改为你想要显示的员工的ID

        SqlDataAdapter adapter = new SqlDataAdapter(command);
        DataTable dataTable = new DataTable();

        adapter.Fill(dataTable);

        if (dataTable.Rows.Count > 0)
        {
            DetailsView1.DataSource = dataTable;
            DetailsView1.DataBind();
        }
    }
}
```

```c#
using System;
using System.Data;
using System.Data.SqlClient;
using System.Web.Configuration;

public partial class YourPage : System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {
        if (!IsPostBack)
        {
            string connectionString = WebConfigurationManager.ConnectionStrings["MyConnectionString"].ConnectionString; // 获取 Web.config 中的连接字符串

            string query = "SELECT EmployeeID, FirstName, LastName, Title FROM Employees WHERE EmployeeID = @EmployeeID"; // 更改为你自己的查询语句

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                using (SqlCommand command = new SqlCommand(query, connection))
                {
                    command.Parameters.AddWithValue("@EmployeeID", 1); // 更改为你想要显示的员工的ID

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
            }
        }
    }
}
```
