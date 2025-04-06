## Connection Strings For SqlServer

There are two types of connection strings, absolute and relative. Absolute strings will only work on
your machine, not the markers. Relative strings are the better option because they will work on any machine.

However, they require a bit of setup to get working properly.

### Setup

First, you need to create an `App.config` file in the root of your solution. Once created, paste the following code into it.
This allows you to access the following string from anywhere in your program.

Make sure to change the `path\to\your\database.mdf` to the path ( from the root ) to your database.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
	<connectionStrings>
		<add name = "DatabaseConnectionString"
			 connectionString = "Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename={0}\path\to\your\database.mdf;Integrated Security=True"
				providerName = "System.Data.SqlClient" />
	</connectionStrings>
</configuration>
```

Next, you can either create the following variable in your DAL, or a global static variable. It makes sense to just have one connection string variable, and then reference
that in every DAL you create.

The following variable will take the connection string from the `App.config`, and join in with your root directory.

```cs
 public static string ConnectionString = string.Format(
        ConfigurationManager.
        ConnectionStrings["DatabaseConnectionString"].
        ConnectionString,
        Directory.GetParent(AppDomain.CurrentDomain.BaseDirectory)!.Parent!.Parent!.Parent!.Parent!.FullName
        );
```

Referencing this `ConnectionString` variable will allow you to access your database from anywhere in your program.
