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

### Database Initialization Using SQL Script (Recommended)

The MVC4 version includes a SQL script file (`MvcMusicStore-Create.sql`) that creates the database schema and populates it with sample data. This is the recommended approach for setting up the database.

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
   - After creating the database, ensure the connection string in `Web.config` points to the correct database:
     ```xml
     <add name="MusicStoreEntities"
          connectionString="Data Source=(LocalDB)\v11.0;Initial Catalog=MvcMusicStore;Integrated Security=True"
          providerName="System.Data.SqlClient" />
     ```
   
   > **Note**: If you're using Visual Studio 2022, you may want to use `(LocalDB)\MSSQLLocalDB` instead of `(LocalDB)\v11.0` for better compatibility with newer versions of LocalDB.

### Alternative: Automatic Database Initialization

You can also use the automatic database initialization approach using the `SampleData` class:

1. Open the solution in Visual Studio: `MvcMusicStore.sln`
2. Build the solution (Ctrl+Shift+B)
3. Run the application (F5)

The database will be automatically:
- Attached to LocalDB
- Initialized with the schema
- Seeded with sample music store data (albums, genres, artists)

This is handled by the `SampleData` class in `Models/SampleData.cs`.

### Connection Strings

The application uses the following connection strings (configured in `Web.config`):

- **MusicStoreEntities**: Main application database for albums, artists, and genres
  ```xml
  <add name="MusicStoreEntities"
       connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\MvcMusicStore.mdf;Integrated Security=True"
       providerName="System.Data.SqlClient" />
  ```

- **DefaultConnection**: SimpleMembership database for user authentication
  ```xml
  <add name="DefaultConnection"
       connectionString="Data Source=(LocalDB)\v11.0;Initial Catalog=aspnet-MvcMusicStore-20120831200627;Integrated Security=SSPI;AttachDBFilename=|DataDirectory|\aspnet-MvcMusicStore-20120831200627.mdf"
       providerName="System.Data.SqlClient" />
  ```

> **Note**: MVC4 uses LocalDB v11.0 by default. If you're using Visual Studio 2022, you may want to update the connection strings to use `(LocalDB)\MSSQLLocalDB` instead of `(LocalDB)\v11.0` for better compatibility.

## Troubleshooting

### LocalDB Not Found

If you encounter an error about LocalDB not being found:
1. Verify LocalDB is installed by running in Command Prompt: `sqllocaldb info`
2. If not installed, install it via Visual Studio Installer > Modify > Individual Components > SQL Server Express LocalDB
3. If you see v11.0 is not available, update your connection strings to use `(LocalDB)\MSSQLLocalDB`

### Database Connection Errors

If you experience database connection issues:
1. Delete the `.mdf` and `.ldf` files from the `App_Data` folder
2. Re-run the SQL script or rebuild and run the application - the database will be recreated

### Cannot Attach Database

If the database is already attached to another instance:
1. Detach it from Server Explorer (right-click the connection > Delete)
2. Or use command line: `sqllocaldb stop MSSQLLocalDB` and `sqllocaldb start MSSQLLocalDB`

### SQL Script Execution Errors

If you encounter errors when running the SQL script:
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
