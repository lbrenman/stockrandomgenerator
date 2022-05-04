# API Builder Stock Quote Random

API Builder project *stockrandomgenerator* exposes an API `GET /api/stockquote?symbol=txn` that returns a random stock quote value in a given pattern so that the value does NOT change every time you call it.

```
curl -H "accept: application/json" -H "apikey: yn....GAV" -X GET "http://localhost:8080/api/stockquote?symbol=txn"
```

The response looks like:

```
{
  "symbol": "txn",
  "lastPrice": 2287
}
```

The pattern is predicated on a data object retrieved from a MongoDB in [**MongoDB Atlas**](https://cloud.mongodb.com/) called *mybank* and a collection called *stockrandconfig*. Only the first document returned in that collection is analyzed so make sure that there is only one document. If there are more than one then unexpected results may happen.

The object is as follows:

```
{
  "_id": {
    "$oid": "62715607f06c84d0cfa635d2"
  },
  "dataMax": {
    "$numberInt": "4000"
  },
  "dataMin": {
    "$numberInt": "10"
  },
  "dataVal": {
    "$numberInt": "1862"
  },
  "firstRun": true,
  "intervalMax": {
    "$numberInt": "10"
  },
  "intervalMin": {
    "$numberInt": "2"
  },
  "intervalVal": {
    "$numberInt": "8"
  },
  "numRuns": {
    "$numberInt": "6"
  }
}
```

The pattern is to return a random integer value between *dataMin* and *dataMax* and to return that value for every call until a number of calls have been made and then return a different random value and so on.

The number of calls before the random value is changed is also random and in between *intervalMin* and *intervalMax*.

To start the pattern, set:

* **firstRun** to true
* **dataMin** to the minimum random value you want returned
* **dataMax** to the maximum random value you want returned
* **intervalMin** to the minimum random number of times you want the same value returned before it changes
* **intervalMax** to the maximum random number of times you want the same value returned before it changes

The other values are computed and can be left alone or set to 0 (or any value).

Screen shots of initializing the sequence is shown below:

![](https://i.imgur.com/AUcK3C7.png)

![](https://i.imgur.com/E4IFC4r.png)


This project requires the following environment variables to be set (e.g. in `/conf/.env` or similar):

```
PORT=8080
API_KEY=<An API Key that you provide and use in API Calls>
MONGO_USERNAME=<MongoDB DB user's username>
MONGO_PASSWORD=<MongoDB DB user's password>
MONGO_DB_NAME=mybank
```

The flow is as follows:

![](https://i.imgur.com/eKYCdol.png)
