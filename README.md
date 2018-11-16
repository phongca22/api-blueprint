# Design API first

* Reference: [API Blueprint](https://apiblueprint.org)
* Extension: `.apib`

``` apib
FORMAT: 1A

# Advanced Action API
A resource action is – in fact – a state transition. This API example
demonstrates an action - state transition - to another resource.


# Tasks [/tasks/tasks{?status,priority}]

+ Parameters
    + status (string)
    + priority (number)

## List All Tasks [GET]

+ Response 200 (application/json)

        [
            {
                "id": 123,
                "name": "Exercise in gym",
                "done": false,
                "type": "task"
            },
            {
                "id": 124,
                "name": "Shop for groceries",
                "done": true,
                "type": "task"
            }
        ]

## Retrieve Task [GET /task/{id}]
This is a state transition to another resource.

+ Parameters
    + id (string)

+ Response 200 (application/json)

        {
            "id": 123,
            "name": "Go to gym",
            "done": false,
            "type": "task"
        }

## Delete Task [DELETE /task/{id}]

+ Parameters
    + id (string)

+ Response 204

```

# Create Mock Server

Reference: [Drakov](https://github.com/Aconex/drakov)

## Install
``` bash
npm install drakov
```

## Locally
``` bash
drakov -f demo.apib
```

## Docker

### Create Dockerfile

``` 
FROM node:9.11.2-alpine

RUN apk update \
  && apk add --no-cache git make gcc g++ python \
  && npm config set loglevel error \
  && npm install drakov@1.0.4 \
  && apk del git make gcc g++ python \
  && rm -rf /var/cache/apk/*

ENV PATH ${PATH}:/node_modules/.bin

ADD test.apib /test.apib

CMD ["-f","/test.apib"]

ENTRYPOINT ["drakov","-p","3000", "--public"]
```

### Build image

Image name: mock-server
``` bash
docker build -t mock-server .
```

### Run
``` bash
docker run --name demo-serer mock-server -p 3000:3000
```

# API renderer

Reference: [Aglio](https://github.com/danielgtaylor/aglio)

## Install
``` bash
npm install aglio
```

## Convert to HTML
``` bash
aglio -i demo.apib -o demo.html
```


