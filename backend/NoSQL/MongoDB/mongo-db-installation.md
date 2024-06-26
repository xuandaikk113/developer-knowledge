## MongoDB Installation
### Method 1: With Docker
- Step 1: Install Docker
- Step 2: Install mongosh (Mongo Shell - Manage database with CLI)
- Step 3: Copy the path of mongosh.exe inside bin folder and create System Environment Variables with this path
- Step 4: Install Mongo Compass (Mango GUI - Manage databases with GUI)
- Step 5: Run MongoDB docker container
- Step 6: Access to container shell and run: ```mongosh --port 27017```
- Step 7: Copy the connecting string: ```mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.2.6```
- Step 6: Connect the MongoDB with mongosh or Mongo Compass with the connecting string
### Method 2: Without Docker
- Step 1: Download the .msi setup file of MongoDB in the official website and install (included Mongo Compass)
- Step 2: Install mongosh (Mongo Shell - Manage database with CLI)
- Step 3: Copy the path of mongosh.exe inside bin folder and create System Environment Variables with this path
- Step 4: Connect the MongoDB with mongosh or Mongo Compass with the default port
- Step 5 (Optional): Install MongoDB for VS Code and using the shell of the extension instead of mongosh installed above