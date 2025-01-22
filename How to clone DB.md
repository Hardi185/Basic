# Exporting and Importing PostgreSQL Database

## Exporting the Database

### Update Environment Variables

Add the following path to your system's environment variables (in the `Path` variable):

```
C:\Program Files\PostgreSQL\{Your PostgreSQL Version}\bin
```

### Run the Export Command

1. Open Command Prompt (CMD) as Administrator.
2. Run the following command:

   ```bash
   pg_dump -U postgres -p 5432 -d CitySavvyDb -W -f "{Target Folder Path}\CitySavvyDb.sql"
   ```

   - `-U postgres`: Specifies the PostgreSQL user (default: postgres).
   - `-p 5432`: Specifies the port (default: 5432).
   - `-d CitySavvyDb`: Specifies the name of the database to export.
   - `-W`: Prompts for the PostgreSQL password.
   - `-f "{Target Folder Path}\CitySavvyDb.sql"`: Specifies the target file path to save the exported `.sql` file.

### Provide the Password

After pressing Enter, you will be prompted to enter the PostgreSQL password. Enter the password to complete the export.

### Verify the Export

Navigate to the target folder path you specified. The file `CitySavvyDb.sql` will be generated there.

---

## Importing the Database

### Prepare the SQL File

Copy the `.sql` file sent to you (e.g., `CitySavvyDb.sql`) to your desired location (e.g., `C:/Users/User/Desktop/CitySavvyDb.sql`).

### Create or Use an Existing Database

Create a new database in PostgreSQL or use an existing one.

### Run the Import Command

1. Open Command Prompt (CMD) as Administrator.
2. Run the following command:

   ```bash
   psql -U postgres -d {Your Database Name} -f "C:/Users/User/Desktop/CitySavvyDb.sql"
   ```

   - `-U postgres`: Specifies the PostgreSQL user (default: postgres).
   - `-d {Your Database Name}`: Specifies the database to import the data into.
   - `-f "C:/Users/User/Desktop/CitySavvyDb.sql"`: Specifies the file path of the `.sql` file.

### Verify the Import

Check the database in PostgreSQL to ensure the data has been imported successfully.

