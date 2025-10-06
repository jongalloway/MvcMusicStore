# MVC Music Store - ASP.NET MVC 5 Version

This is the ASP.NET MVC 5 version of the MVC Music Store sample application.

## Prerequisites

- **Visual Studio 2022** (or Visual Studio 2015 or later)
- **SQL Server LocalDB** - Installed with Visual Studio 2022

> **Note**: SQL Server LocalDB is automatically installed with Visual Studio 2022 as part of the "ASP.NET and web development" workload or the "Data storage and processing" workload.

## Database Setup

The MVC Music Store application uses SQL Server LocalDB with database files located in the `App_Data` folder:
- `MvcMusicStore.mdf` - Main application database
- `aspnet-MvcMusicStore-20131025034205.mdf` - ASP.NET Identity database

### Automatic Database Initialization (Recommended)

The application is configured to automatically create and seed the database on first run:

1. Open the solution in Visual Studio: `MvcMusicStore.sln`
2. Build the solution (Ctrl+Shift+B)
3. Run the application (F5)

The database will be automatically:
- Attached to LocalDB
- Initialized with the schema
- Seeded with sample music store data (albums, genres, artists)

This is handled by the `SampleData` class in `Models/SampleData.cs`, which is configured as the database initializer in `Global.asax.cs` and `App_Start/Startup.App.cs`.

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
     - For the identity database: `App_Data\aspnet-MvcMusicStore-20131025034205.mdf`

4. **Confirm and Connect**
   - Click `OK` to connect
   - Visual Studio will automatically attach the `.mdf` file to LocalDB
   - You'll see the database listed under `Data Connections` in Server Explorer

### Connection Strings

The application uses the following connection strings (configured in `Web.config`):

- **MusicStoreEntities**: Main application database for albums, artists, and genres
  ```
  Data Source=(LocalDb)\MSSQLLocalDB;
  AttachDbFilename=|DataDirectory|\MvcMusicStore.mdf;
  Integrated Security=True
  ```

- **DefaultConnection**: ASP.NET Identity database for user authentication
  ```
  Data Source=(LocalDb)\MSSQLLocalDB;
  AttachDbFilename=|DataDirectory|\aspnet-MvcMusicStore-20131025034205.mdf;
  Initial Catalog=aspnet-MvcMusicStore-20131025034205;
  Integrated Security=True
  ```

### Default Administrator Account

The application automatically creates a default administrator account on startup:
- **Username**: Administrator
- **Password**: YouShouldChangeThisPassword

These credentials can be modified in `Web.config`:
```xml
<add key="DefaultAdminUsername" value="Administrator"/>
<add key="DefaultAdminPassword" value="YouShouldChangeThisPassword"/>
```

## Troubleshooting

### LocalDB Not Found

If you encounter an error about LocalDB not being found:
1. Verify LocalDB is installed by running in Command Prompt: `sqllocaldb info`
2. If not installed, install it via Visual Studio Installer > Modify > Individual Components > SQL Server Express LocalDB

### Database Connection Errors

If you experience database connection issues:
1. Delete the `.mdf` and `.ldf` files from the `App_Data` folder
2. Rebuild and run the application - the database will be recreated automatically

### Cannot Attach Database

If the database is already attached to another instance:
1. Detach it from Server Explorer (right-click the connection > Delete)
2. Or use command line: `sqllocaldb stop MSSQLLocalDB` and `sqllocaldb start MSSQLLocalDB`

## Running the Application

1. Open `MvcMusicStore.sln` in Visual Studio
2. Press `F5` to build and run the application
3. The application will open in your default browser
4. Browse the music store, add items to cart, and test the checkout process
5. Login with the administrator account to access the store management features

## Additional Information

For more information about ASP.NET MVC, visit [Microsoft Learn](https://learn.microsoft.com/en-us/aspnet/mvc/).
