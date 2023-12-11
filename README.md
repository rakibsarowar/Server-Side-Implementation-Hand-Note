"# Server-Side-Implementation-Hand-Note" 

>a detailed step-by-step guide for setting up a Node.js server using Express and MongoDB Atlas:

<br>

## Setting Up the Server

## 1. Initialize Your Project:
<br>

```
npm init -y

```
<br>

## 2. Install Dependencies:
<br>

```
npm i express cors dotenv mongodb

```
<br>

## 3. Create an index.js File:
Initialize your main server file.
<br>

## 4. Initialize Express:
<br>

```
const express = require('express');
const app = express();

```
<br>

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
<br>

## 7. Set the Port:
Define the port in the environment variable or default to 5000.
<br>

```
const port = process.env.PORT || 5000;

```