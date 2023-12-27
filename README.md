# Hand Note of Server Side Implementation by Node.js
<br>

>a detailed step-by-step guide for setting up a Node.js server using Express and MongoDB Atlas:

<br>
<div align=center>
  
| Key Note                    |                      |                |                       |
|-----------------------------|----------------------|----------------|-----------------------|
| **Emoji**                    | **Description**          | **Emoji**   | **Description**       |
| 🌴                           | **Main Topic**       | 📌             | **Regular Note**      |
| 🌿                           | **Paragraph**        | 💎             | **High Value info**   |
| 📕                           | **Heavy Note**       | 🧨             | **Careful this**      |
| 🍂                           | **Attention Note**   | ✋             | **Stop! check the point** |
| 🏷️                          | **Regular Note**     | 🎯             | **Focus**             |

</div>

<!-- NO COMMENT -->

<details>

<summary><b> Expand Table of Contents </b></summary>

 [🌴 Setting Up the Server](#-setting-up-the-server)
  - [🌿 1. Initialize Your Project](#-1-initialize-your-project)
  - [🌿 2. Install Dependencies](#-2-install-dependencies)
  - [🌿 3. Create an index.js File](#-create-an-index-js-file)
  - [🌿 4. Initialize Express](#-4.-initialize-express)
  - [🌿 5. Enable CORS Middleware](#-5-enable-cors-middleware)
  - [🌿 6. Use JSON Middleware](#-6-create-an-index.js-file)
  - [🌿 7. Set the Port](#-7-create-an-index.js-file)
  - [🌿 8. Create a Basic Route](#-8-create-an-index.js-file)
  - [🌿 9. Start the Server](#-9-create-an-index.js-file)
  - [🌿 Final Output](#-create-an-index-js-file)
[🌴 MongoDB Atlas Setup](#-4.-initialize-express)
  - [🌿 1.Create a MongoDB Atlas Account](#-5-enable-cors-middleware)
  - [🌿 2. Set Up a Cluster](#-6-create-an-index.js-file)
  - [🌿 3. Create Database User and Name the Database](#-7-create-an-index.js-file)
  - [🌿 4. Environment Configuration](#-8-create-an-index.js-file)
  - [🌿 5. Store Database Credentials in the Environment File](#-9-create-an-index.js-file)

  </details>

<br>

## 🌴 Setting Up the Server

## 🌿 1. Initialize Your Project:
<br>

```
npm init -y

```

## 🌿 2. Install Dependencies:
<br>

```
npm i express cors dotenv mongodb

```

## 🌿 3. Create an index.js File:
Initialize your main server file.

## 🌿 4. Initialize Express:
<br>

```
const express = require('express');
const app = express();

```

## 🌿 5. Enable CORS Middleware:
<br>

```
const cors = require('cors');
app.use(cors());

```

## 🌿 6. Use JSON Middleware:
<br>

```
app.use(express.json());

```

## 🌿 7. Set the Port:
Define the port in the environment variable or default to 5000.
<br>

```
const port = process.env.PORT || 5000;

```

## 🌿 8. Create a Basic Route:
Create a simple route to check if the server is running.
<br>

```
app.get('/', (req, res) => {
    res.send('Server is running');
});

```

## 🌿 9. Start the Server:
<br>

```
app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});

```

<br>

## Final Output:

```
const express = require ('express')
const app = express();
const cors = require('cors');
const port = process.env.PORT || 5000;

// middleware
app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
    res.send('server is running')
})

app.listen(port, () => {
    console.log( `Server is running on port ${port}`)
})

```

## 🌴 MongoDB Atlas Setup
 
## 🌿 1.Create a MongoDB Atlas Account:
=> Sign up for MongoDB Atlas and log in.

## 🌿 2. Set Up a Cluster:
=> Create a new cluster and configure it.

## 🌿 3. Create Database User and Name the Database:
=> Create a database user with privileges and name your database.

## 🌿 4. Environment Configuration:
=> Create a .env file and add it to .gitignore.

## 🌿 5. Store Database Credentials in the Environment File:
<br>

```
DB_USER=yourDatabaseUser
DB_PASS=yourDatabasePassword

```
## 🌿 6. Get Connection URI:
Go to "Database Access" and copy the connection string provided by MongoDB Atlas.

## 🌿 7. Use Environment Variables in Code:
Replace the MongoDB URI in your code with environment variables.

## 🌿 8. Connect to the MongoDB Cluster:
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
## 🌿 9. Handle Proper Closing of Connection:
It's important to manage connections properly. Ensure you close the client when your server stops:
<br>

```
process.on('SIGINT', () => {
    client.close();
    process.exit();
});

```
## 🌿 Full code 
<br>

```
const express = require ('express')
const app = express();
const cors = require('cors');
require('dotenv').config()
const port = process.env.PORT || 5000;

// middleware
app.use(cors());
app.use(express.json());


const { MongoClient, ServerApiVersion } = require('mongodb');
const uri = `mongodb+srv://${process.env.DB_USER}:${process.env.DB_PASS}@cluster0.d94f49k.mongodb.net/?retryWrites=true&w=majority`;

// Create a MongoClient with a MongoClientOptions object to set the Stable API version

const client = new MongoClient(uri, {
  serverApi: {
    version: ServerApiVersion.v1,
    strict: true,
    deprecationErrors: true,
  }
});

async function run() {
  try {
    // Connect the client to the server	(optional starting in v4.7)
    await client.connect();

    const menuCollection = client.db("redCafe").collection("menu");
    const reviewCollection = client.db("redCafe").collection("reviews");

    app.get('/menu', async(req, res) =>{
        const result = await menuCollection.find().toArray();
        res.send(result);
    })
    
    // Send a ping to confirm a successful connection
    await client.db("admin").command({ ping: 1 });
    console.log("Pinged your deployment. You successfully connected to MongoDB!");
  } finally {
    // Ensures that the client will close when you finish/error
    // await client.close();
  }
}
run().catch(console.dir);


app.get('/', (req, res) => {
    res.send('server is running')
})

app.listen(port, () => {
    console.log( `Server is running on port ${port}`)
})

```

<br>
Note: This hand note walks you through setting up a basic Node.js server using Express and connecting it to MongoDB Atlas. Remember to replace placeholder values with your actual credentials and adapt the code as needed for your project.


 ## CRUD Operations (Sample Implementation)
Here's an example of how CRUD operations can be implemented:
<br>

 ## 🌿 Create (POST):

```
app.post('/menu', async (req, res) => {
    try {
        const newItem = req.body;
        const result = await menuCollection.insertOne(newItem);
        res.json(result.ops[0]);
    } catch (err) {
        console.error('Error creating item:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```

```
    app.post('/carts', async (req, res) => {
      const item = req.body;
      console.log(item);
      const result = await cartCollection.insertOne(item);
      res.send(result);
    })
```

## 🌿 Read (GET)

```
app.get('/menu', async (req, res) => {
    try {
        const result = await menuCollection.find().toArray();
        res.json(result);
    } catch (err) {
        console.error('Error fetching menu:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```
<br>
**Another example:** 

```
    app.get('/reviews', async(req, res) =>{
        const result = await reviewCollection.find().toArray();
        res.send(result);
    })

```

## 🌿 Update (PUT)

```
app.put('/menu/:id', async (req, res) => {
    try {
        const itemId = req.params.id;
        const updatedItem = req.body;
        const result = await menuCollection.updateOne({ _id: itemId }, { $set: updatedItem });
        res.json(result);
    } catch (err) {
        console.error('Error updating item:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```

## Partially update (Patch)
The PATCH method in HTTP is used to apply partial modifications to a resource. It's commonly used to update specific fields of an existing resource without requiring the client to send the entire representation of that resource.

In the context of a RESTful API, the PATCH method is often used when you want to update specific attributes or properties of an existing resource identified by a unique identifier (like an ID).

```
 app.patch('/users/admin/:id', async (req, res) => {
      const id = req.params.id;
      console.log(id);
      const filter = { _id: new ObjectId(id) };
      const updateDoc = {
        $set: {
          role: 'admin'
        },
      };

      const result = await usersCollection.updateOne(filter, updateDoc);
      res.send(result);

    })

```


## 🌿 Delete (DELETE)

```
app.delete('/menu/:id', async (req, res) => {
    try {
        const itemId = req.params.id;
        const result = await menuCollection.deleteOne({ _id: itemId });
        res.json(result);
    } catch (err) {
        console.error('Error deleting item:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```

```
app.delete('/carts/:id', async (req, res) => {
      const id = req.params.id;
      const query = { _id: new ObjectId(id) };
      const result = await cartCollection.deleteOne(query);
      res.send(result);
    })
```

## 🌴 Extended CRUD Operations with MongoDB Queries

## 🌿 Aggregation:
Demonstrates aggregating data, such as calculating the total sum of prices for all items in the menu.
<br>

```
app.get('/menu/pricesummary', async (req, res) => {
    try {
        const pipeline = [
            {
                $group: {
                    _id: null,
                    total: { $sum: '$price' }
                }
            }
        ];
        const result = await menuCollection.aggregate(pipeline).toArray();
        res.json(result);
    } catch (err) {
        console.error('Error aggregating prices:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```

## 🌿 Filtering and Sorting 
Shows how to filter documents based on criteria (here, filtering by a specific category) and sort the results.
<br>

```
app.get('/menu/sorted', async (req, res) => {
    try {
        const filter = { category: 'Coffee' }; // Example filter
        const sortCriteria = { price: -1 }; // Sort by price descending
        const result = await menuCollection.find(filter).sort(sortCriteria).toArray();
        res.json(result);
    } catch (err) {
        console.error('Error filtering and sorting menu:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```
## 🌿 Projection 
Illustrates how to project specific fields in the result and exclude others using projection in MongoDB.
<br>

```
app.get('/menu/projected', async (req, res) => {
    try {
        const projection = { name: 1, price: 1, _id: 0 }; // Only retrieve name and price, exclude _id
        const result = await menuCollection.find().project(projection).toArray();
        res.json(result);
    } catch (err) {
        console.error('Error projecting menu:', err);
        res.status(500).json({ error: 'Internal server error' });
    }
});

```
