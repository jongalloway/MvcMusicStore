# MVC Music Store - ASP.NET MVC 4 Version

This is the ASP.NET MVC 4 version of the MVC Music Store sample application.

## Prerequisites

- **Visual Studio 2022** (or Visual Studio 2012 or later)
- **SQL Server LocalDB** - Installed with Visual Studio 2022

> **Note**: SQL Server LocalDB is automatically installed with Visual Studio 2022 as part of the "ASP.NET and web development" workload or the "Data storage and processing" workload.

## Database Setup

The MVC Music Store application uses SQL Server LocalDB with database files located in the `App_Data` folder:
- `MvcMusicStore.mdf` - Main application database
- `aspnet-MvcMusicStore-20120831200627.mdf` - SimpleMembership database for user authentication

### Automatic Database Initialization (Recommended)

The application is configured to automatically create and seed the database on first run:

1. Open the solution in Visual Studio: `MvcMusicStore.sln`
2. Build the solution (Ctrl+Shift+B)
3. Run the application (F5)

The database will be automatically:
- Attached to LocalDB
- Initialized with the schema
- Seeded with sample music store data (albums, genres, artists)

This is handled by the `SampleData` class in `Models/SampleData.cs`.

### Manual Database Connection (Optional)

If you need to manually connect to the database using Server Explorer in Visual Studio:

1. **Open Server Explorer**
   - Go to `View` > `Server Explorer` (or press `Ctrl+Alt+S`)

2. **Add a Data Connection**
   - Right-click `Data Connections` > `Add Connection…`

3. **Configure the Connection**
   - **Data Source**: Select `Microsoft SQL Server (SqlClient)`
   - **Server Name**: Enter `(LocalDB)\MSSQLLocalDB`
   - **Authentication**: Use `Windows Authentication`
   - **Database**: Click `Browse…` and select the `.mdf` file from the `App_Data` folder:
     - For the main database: `App_Data\MvcMusicStore.mdf`
     - For the identity database: `App_Data\aspnet-MvcMusicStore-20120831200627.mdf`

4. **Confirm and Connect**
   - Click `OK` to connect
   - Visual Studio will automatically attach the `.mdf` file to LocalDB
   - You'll see the database listed under `Data Connections` in Server Explorer

### Database Initialization Using SQL Script (Alternative)

The MVC4 version includes a SQL script file (`MvcMusicStore-Create.sql`) that creates the database schema and populates it with sample data. This is an alternative approach for setting up the database.

> **Note**: For most users, the automatic database initialization (recommended above) is simpler and sufficient. Use this SQL script approach only if you need to create a named database instance or have specific database setup requirements.

#### Running the SQL Script in Visual Studio 2022

1. **Open the Solution**
   - Open `MvcMusicStore.sln` in Visual Studio 2022

2. **Open Server Explorer**
   - Go to `View` > `Server Explorer` (or press `Ctrl+Alt+S`)

3. **Create a Data Connection**
   - Right-click `Data Connections` > `Add Connection…`
   - **Data Source**: Select `Microsoft SQL Server (SqlClient)`
   - **Server Name**: Enter `(LocalDB)\MSSQLLocalDB`
   - **Authentication**: Use `Windows Authentication`
   - **Database name**: Click `Test Connection` to verify the connection
   - Click `OK`

4. **Execute the SQL Script**
   - In Solution Explorer, right-click on `MvcMusicStore-Create.sql`
   - Select `Open With...` > `SQL Query Editor` (or use any SQL query editor)
   - Alternatively, double-click the file to open it in the SQL editor
   - Update the `USE` statement at the top of the file to point to your LocalDB instance:
     ```sql
     -- Replace the existing USE statement with:
     CREATE DATABASE MvcMusicStore;
     GO
     USE MvcMusicStore;
     GO
     ```
   - Click `Execute` or press `Ctrl+Shift+E` to run the script
   - The script will create all tables (Albums, Artists, Genres, Orders, OrderDetails, Carts) and populate them with sample data

5. **Update the Connection String**
   - After creating the database using the SQL script, you need to update the connection string in `Web.config` to use the database you just created.
   - Find the `MusicStoreEntities` connection string in `Web.config` and replace it with:
     ```xml
     <add name="MusicStoreEntities"
          connectionString="Data Source=(LocalDB)\MSSQLLocalDB;Initial Catalog=MvcMusicStore;Integrated Security=True"
          providerName="System.Data.SqlClient" />
     ```
   
   > **Note**: This connection string uses `Initial Catalog=MvcMusicStore` to connect to the database you created with the SQL script. If you're not using Visual Studio 2022, replace `(LocalDB)\MSSQLLocalDB` with `(LocalDB)\v11.0`.

### Connection Strings

The application uses the following connection strings (configured in `Web.config`):

- **MusicStoreEntities**: Main application database for albums, artists, and genres
  ```
  Data Source=(LocalDb)\MSSQLLocalDB;
  AttachDbFilename=|DataDirectory|\MvcMusicStore.mdf;
  Integrated Security=True
  ```

- **DefaultConnection**: SimpleMembership database for user authentication
  ```
  Data Source=(LocalDb)\MSSQLLocalDB;
  AttachDbFilename=|DataDirectory|\aspnet-MvcMusicStore-20120831200627.mdf;
  Integrated Security=True
  ```

> **Note**: Replace `(LocalDB)\MSSQLLocalDB` with `(LocalDB)\v11.0` if you're not using Visual Studio 2022, or if you encounter compatibility issues with MSSQLLocalDB.

If you're using the SQL script approach (alternative method above), you'll need to use a different connection string format that references the database by name:

```xml
<add name="MusicStoreEntities"
     connectionString="Data Source=(LocalDB)\MSSQLLocalDB;Initial Catalog=MvcMusicStore;Integrated Security=True"
     providerName="System.Data.SqlClient" />
```

## Troubleshooting

### LocalDB Not Found

If you encounter an error about LocalDB not being found:
1. Verify LocalDB is installed by running in Command Prompt: `sqllocaldb info`
2. If not installed, install it via Visual Studio Installer > Modify > Individual Components > SQL Server Express LocalDB
3. If you see v11.0 is not available, update your connection strings to use `(LocalDB)\MSSQLLocalDB`

### Database Connection Errors

If you experience database connection issues:
1. Delete the `.mdf` and `.ldf` files from the `App_Data` folder
2. Rebuild and run the application - the database will be recreated automatically

### Cannot Attach Database

If the database is already attached to another instance:
1. Detach it from Server Explorer (right-click the connection > Delete)
2. Or use command line: `sqllocaldb stop MSSQLLocalDB` and `sqllocaldb start MSSQLLocalDB`

### SQL Script Execution Errors

If you encounter errors when running the SQL script (alternative method):
1. Make sure the `USE` statement at the top of the file is updated correctly
2. Ensure you have permissions to create databases on your LocalDB instance
3. Check that no other instance of the application is using the database
4. Try running the script sections one at a time to identify which part is failing

## Running the Application

1. Open `MvcMusicStore.sln` in Visual Studio
2. Press `F5` to build and run the application
3. The application will open in your default browser
4. Browse the music store, add items to cart, and test the checkout process
5. Register a new account or login to test the authentication features

## Additional Information

For more information about ASP.NET MVC 4, visit [Microsoft Learn](https://learn.microsoft.com/en-us/aspnet/mvc/).
