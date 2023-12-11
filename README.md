"# Server-Side-Implementation-Hand-Note" 

>a detailed step-by-step guide for setting up a Node.js server using Express and MongoDB Atlas:

<br>

## Setting Up the Server

## 1. Initialize Your Project:
<br>

```
npm init -y

```

## 2. Install Dependencies:
<br>

```
npm i express cors dotenv mongodb

```

## 3. Create an index.js File:
Initialize your main server file.

## 4. Initialize Express:
<br>

```
const express = require('express');
const app = express();

```

## 5. Enable CORS Middleware:
<br>

```
const cors = require('cors');
app.use(cors());

```

## 6. Use JSON Middleware:
<br>

```
app.use(express.json());

```

## 7. Set the Port:
Define the port in the environment variable or default to 5000.
<br>

```
const port = process.env.PORT || 5000;

```

## 8. Create a Basic Route:
Create a simple route to check if the server is running.
<br>

```
app.get('/', (req, res) => {
    res.send('Server is running');
});

```

## 9. Start the Server:
<br>

```
app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});

```

<br>

## MongoDB Atlas Setup
 
## 1.Create a MongoDB Atlas Account:
=> Sign up for MongoDB Atlas and log in.

## 2. Set Up a Cluster:
=> Create a new cluster and configure it.

## 3. Create Database User and Name the Database:
=> Create a database user with privileges and name your database.

## 4. Environment Configuration:
=> Create a .env file and add it to .gitignore.

## 5. Store Database Credentials in the Environment File:
<br>

```
DB_USER=yourDatabaseUser
DB_PASS=yourDatabasePassword

```
## 6. Get Connection URI:
Go to "Database Access" and copy the connection string provided by MongoDB Atlas.

## 7. Use Environment Variables in Code:
Replace the MongoDB URI in your code with environment variables.

## 8. Connect to the MongoDB Cluster:
Inside your server code, establish a connection to MongoDB using the MongoClient.
<br>

```
const { MongoClient } = require('mongodb');

const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });

client.connect(err => {
    if (err) {
        console.error('Error connecting to MongoDB:', err);
        return;
    }
    
    const menuCollection = client.db('bistroDb').collection('menu');
    const reviewCollection = client.db('bistroDb').collection('reviews');

    app.get('/menu', async (req, res) => {
        const result = await menuCollection.find().toArray();
        res.send(result);
    });
});

```
## 9. Handle Proper Closing of Connection:
It's important to manage connections properly. Ensure you close the client when your server stops:
<br>

```
process.on('SIGINT', () => {
    client.close();
    process.exit();
});

```

<br>
Note: This hand note walks you through setting up a basic Node.js server using Express and connecting it to MongoDB Atlas. Remember to replace placeholder values with your actual credentials and adapt the code as needed for your project.


