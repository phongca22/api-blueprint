# Design API first

* Reference: [API Blueprint](https://apiblueprint.org)
* Extension: `.apib`

``` apib
FORMAT: 1A
HOST: http://polls.apiblueprint.org/

# Phong ca

Polls is a simple API allowing consumers to view polls and vote in them.

## Questions Collection [/questions{?status,rank}]

+ Parameters
    + status (string)
    + rank (number)

### List All Questions [GET]

+ Response 200 (application/json)

        [
            {
                "question": "Favourite programming language?",
                "published_at": "2015-08-05T08:40:51.620Z",
                "choices": [
                    {
                        "choice": "Swift",
                        "votes": 2048
                    }, {
                        "choice": "Python",
                        "votes": 1024
                    }, {
                        "choice": "Objective-C",
                        "votes": 512
                    }, {
                        "choice": "Ruby",
                        "votes": 256
                    }
                ]
            }
        ]

### Create a New Question [POST]

You may create your own question using this action. It takes a JSON
object containing a question and a collection of answers in the
form of choices.

+ Request (application/json)

        {
            "question": "Favourite programming language?",
            "choices": [
                "Swift",
                "Python",
                "Objective-C",
                "Ruby"
            ]
        }

+ Response 201 (application/json)

    + Headers

            Location: /questions/2

    + Body

            {
                "question": "Favourite programming language?",
                "published_at": "2015-08-05T08:40:51.620Z",
                "choices": [
                    {
                        "choice": "Swift",
                        "votes": 0
                    }, {
                        "choice": "Python",
                        "votes": 0
                    }, {
                        "choice": "Objective-C",
                        "votes": 0
                    }, {
                        "choice": "Ruby",
                        "votes": 0
                    }
                ]
            }
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


