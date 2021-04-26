![Ironhack Logo](https://i.imgur.com/1QgrNNw.png)

# DE | Crunchbase Database

![](https://i.imgur.com/FqGd9jz.jpg)

## Introduction

You will build an small application to perform queries to crunchbase database.

Crunchbase is the premier destination for discovering industry trends, investments, and news about hundreds of thousands of companies globally. From startups to Fortune 500s, Crunchbase is recognized as the primary source of company intelligence by millions of users globally.

## Quering Crunchbase

Inside the starter-code folder you will find the crunchdatabase file.

First, we need to import the database we will be using for the `lab`. We will use the Crunchbase database. Crunchbase is the premier destination for discovering industry trends, investments, and news about hundreds of thousands of companies globally. From startups to Fortune 500s, Crunchbase is recognized as the primary source of company intelligence by millions of users globally.

The database contains more than 18k documents. Each document holds the data about each of the companies. A document looks like the following:

![image](https://user-images.githubusercontent.com/23629340/36494916-d6db1770-1733-11e8-903e-5119b3c1b688.png)

1. You will find the `.zip` file of the database on the **lab** folder.
2. Unzip the file
3. Navigate to this lab's folder in your terminal, and when inside, import the database to Mongo using the following command:

**When running the `mongoimport` you should be located in the same folder as the `data.json` file.**

Download MongoDB database tools from this [link](https://www.mongodb.com/try/download/database-tools?tck=docs_databasetools)

```bash
$ mongoimport --db companiesDB --collection companies --file data.json
```

So your terminal command should be something like 
```
$ .\mongoimport.exe "mongodb+srv://root:root@cluster0.lo8dg.mongodb.net/crunchbase" .\companies.json
```

What this mongoimport will do for us is to create a database named _companiesDB_, and inside the database will create a collection named _companies_ which will be fed with _data.json_.

<img src="https://oscarm.tinytake.com/media/fe6df6?filename=1619452214971_TinyTake26-04-2021-05-50-11_637550490141923431.png&sub_type=thumbnail_preview&type=attachment&width=1199&height=260" title="Powered by TinyTake Screen Capture"/><br><a href="https://www.tinytake.com">Powered by TinyTake Screen Capture</a>

_Side note_: In case errors or hanging with no response when running this command, add [--jsonArray](https://docs.mongodb.com/manual/reference/program/mongoimport/#cmdoption-mongoimport-jsonarray) at the end of the previous command.


💡 Check mongoimport documentation if you don't remember how to do it. 💡


First of all perform in MongoShell the following query in order to know how the documents are structured.

```javascript
db.companies.find({name: "Facebook"}).pretty()
```

## Let's start

Create a new folder for you project.

```javascript
$ npm init
```

Now that we have created the project, add the following packages`clear` and `mongodb`

```javascript
$ npm install --save mongodb
$ npm install --save clear
```

Create a new file called `main.js`

```javascript
const MongoDB = require('mongodb');
const mongoClient = MongoDB.MongoClient;
const clear = require('clear');
const readline = require('readline');
const rl = readline.createInterface({
	input: process.stdin,
	output: process.stdout
});

const url = `mongodb://MONGODB-ATLAS/`

mongoClient.connect(url, (error, db) => {
  if (error) {
    console.log('Error trying to connect to the Database');
    console.log(error);
  } else {
    console.log('Connection established correctly!! 😬');

  }
});

```

Try it to see if it works.

```
$ node main.js
```

Until now we have created the connection with MongoDB Database.

Let's create the mainMenu, copy the following function after `mongoClient.connect();`


```javascript
function printMenu(){
	console.log(`
0.- Exit
1.- List by name all companies.
2.- How many companies are there?
3.- How many companies were founded in 2004?
4.- List by name all companies founded in february of 2004.
5.- List by name all companies founded in the summer of 2004 (april to june) sorted by date.
6.- What companies have offices in "Barcelona".
7.- List the 10 companies with more employees sorted ascending (show name and employees).
8.- Find the company with the name "Facebook"
9.- How many employees has Facebook?
10.- List the name of all the products of Facebook
11.- List the people that are working at Facebook right now (check relationships field)
12.- How many people are not working anymore at Facebook
13.- List all the companies where "david-ebersman" has worked.
14.- List by name the competitors of Facebook
15.- Names of the companies that has "social-networking" in tag-list (be aware that the value of field is a string check regex operators)
16.- How many companies that has "social-network" in tag-list and founded between 2002 and 2016 inclusive
17.- Names and locations of companies that have offices in London
18.- How many companies that has "social-network" in tag-list and founded between 2002 and 2016 inclusive and has offices in New York
`);
}
```
This are all the queries that you have to do.

Now let's create the main function

```javascript
...
mongoClient.connect(url, (error, db) => {
  if (error) {
    console.log('Error trying to connect to the Database');
    console.log(error);
  } else {
    console.log('Connection established correctly!! 😬');

    function mainMenu(){
      clear();
      printMenu();
      rl.question('Type an option: ', (option) => {
        switch(option){
          case "1":
            console.log('you typed 1');
            rl.question(`\nType enter to continue: `, (answer) => {mainMenu()});
            break;
          case "2":
            console.log('you typed 2');
            rl.question(`\nType enter to continue: `, (answer) => {mainMenu()});
            break;
          case "0":
            console.log(`👋👋👋👋 😞 \n`);
            db.close((error) => { process.exit(0) });
            break;
          default:
            mainMenu();
            break;
        }
      });
	}

    mainMenu();

  }
});

function printMenu(){
...
}
```

What we did?

We made an infinite loop until the option is `0`. This `rl` object will allow us to ask through the terminal and we will get a callback with the answer. Once we get the answer we make a `switch`  where we will perform the different queries.

Try it to see if it works

```
$ node main.js
```

Type an option and if you see the console message, it's a good signal.

## Let's build the first query

Inside `case "1":`


💣 **Delete lines inside the case `console.log('you typed 1');` and `rl.question('\nType enter to continue: ', (answer) => {mainMenu()});`** 💣


```javascript
                case "1":
                    db.collection('companies').find({}).project({ name: 1, _id: 0 }).toArray((error, result) => {
                        if (error) {
                            console.log(error);
                            rl.question(`\nType enter to continue: `, (answer) => { mainMenu() });
                        } else {
                            console.log(result);
                            rl.question(`\nType enter to continue: `, (answer) => { mainMenu() });
                        }
                    })
                    break;
```

## Now it's your turn

Perfom the 17 queries, following the instructions how we did before

## Extra Queries

If you feel motivated you can do 3 more extra queries.

- Find all the distinct categories, so list all unique categories [ use distinct method](https://docs.mongodb.com/manual/reference/method/db.collection.distinct/)

- How many companies mention Google in their overview.

💣 **In order to perform this query you have to create an index execute the following `db.companies.createIndex({overview: "text"})` in MongoShell** 💣


💡 **For the query check [text search operator](https://docs.mongodb.com/v3.2/reference/operator/query/text/)** 💡

- Find companies founded in 2004 and having 5 or more rounds of funding, calculate the average amount raised.

💡 **Use Framework aggregation** 💡

## Summary

In this exercise you practice with MongoDB Driver to perform different queries.

## Extra Resources

[MongoDB NodeJS Driver Documentatio](http://mongodb.github.io/node-mongodb-native/3.6/quick-start/)

[MongoShell Documentation](https://docs.mongodb.com/manual/)
