{
"title": "Search Engine for Discord",
"date": "2020-08-11T12:41:05-05:00",
"image": "/img/circleci-workflow.webp",
"link": "https://www.matispore.com/",
"image": "",
"description": "A web app to search within public Discord servers",
"tags": ["React", "NodeJS", "Redis", "BullMQ", "Scalable", "TypeScript"],
"fact": "",
"featured": true,
"category": "Web App",
"draft": false
}

This is about building an aggregated search engine for Discord servers. Users could see and search on what people are talking publicly on popular discord servers. The current version of app is targeted to be used by people interested in fin tech and crypto news.

### Tech Stack

#### Frontend

- React, Mithriljs
- Mobx for state management
- REST clients for CRUD

#### Backend

- Feathersjs for REST APIs
- NodeJS app for data collection/scraping
- Bullmq for queueing

#### DataStore

- Redis as in-memory data store
- Firestore for persistent data storage

#### Programming Languages

- TypeScript
- JavaScript
