# JJ Single Page ToDo Web App
Web app built with NodeJS, Angular and MongoDB

Using:
1. Mongoose - ORM for MongoDB and NodeJS
2. Express - web framework for NodeJS

MongoDB is data store.

What is difference Web App and Web Site ? [link](https://blog.nodejitsu.com/single-page-apps-with-nodejs/)

Demo created based on this [blog](https://scotch.io/tutorials/creating-a-single-page-todo-app-with-node-and-angular)

## Build and start server locally
```bash
npm install
node server.js
```

Open web browser with link http://localhost:8080

## Azure deployment
### Deploy data store
Create Azure CosmosDB with MongoDB API
```bash
az group create --name myResourceGroup --location "West Europe"
az cosmosdb create --name jjcosmosdb --resource-group myResourceGroup --kind MongoDB
```
[Documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/tutorial-develop-mongodb-nodejs-part5)
[Geo distribution](https://docs.microsoft.com/en-us/azure/cosmos-db/distribute-data-globally)
[Data Consistency](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)

Create new database and collection
```bash
az cosmosdb database create --name jjcosmosdb --key <KEY> --db-name jjtodo
az cosmosdb collection create --name jjcosmosdb --key <KEY> --db-name jjtodo --collection-name todos --throughput 400
```

You can test conection to database
```bash
mongo jjcosmosdb.documents.azure.com:10255 -u jjcosmosdb -p <KEY> --ssl --sslAllowInvalidCertificates
    use jjtodo
    db.getCollectionNames()
```

Update connection string in database.js
```
remoteUrl : 'mongodb://jjcosmosdb:<KEY>@jjcosmosdb.documents.azure.com:10255/jjtodo?ssl=true&replicaSet=globaldb',
```

### Deploy website
Deploy web app nodejs [documentation](https://docs.microsoft.com/en-us/azure/app-service/containers/quickstart-nodejs)
1. Create new Web App for Linux with Node.js
2. Setup v Deployment options - Local Git repository
3. Copy git url, like https://jjeset@jjeset-todo.scm.azurewebsites.net:443/jjeset-todo.git
4. Add remote to your repository (in VS code terminal) and push
```ssh
    git remote add azure https://jjeset@jjeset-todo.scm.azurewebsites.net:443/jjeset-todo.git
    git push azure master
```
5. Test in browser https://jjeset-todo.azurewebsites.net/