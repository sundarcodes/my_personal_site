{
"title": "Tuition Management App",
"date": "2020-08-11T12:41:05-05:00",
"image": "/img/circleci-workflow.webp",
"link": "",
"image": "/img/tutora_arch.png",
"description": "A Mobile App to connect tutors with students and relieve of tutors from the administrative hassles",
"tags": ["React Native","AWS", "Lambda", "Mongo", "GraphQL", "SNS", "TypeScript", "Mobx", "Mongo REALM"],
"fact": "",
"featured":true,
"category": "Mobile App"
}

This is about developing a mobile app for small scale/home based tutors. The app is primarily targetted to reduce the administation hassles of managing/running a online/physical classes. The backend is cloud native and frontend is built using React Native.

### Tech Stack

#### Frontend

- React Native as UI framework
- Mobx for state management
- GraphQL/REST clients for CRUD

#### Backend

- AWS Serverless for business logic
- AWS API Gateway for routing
- AWS Cognito for authentication
- AWS S3 for storage
- AWS Event Bridge for integrations with third party apps

#### DataStore

- Mongo DB Atlas for storing application data
- Mongo Realm as the persistence layer interface exposing GraphQL server

#### Integrations

- Twilio for SMS and Email

#### Programming Languages

- TypeScript
- JavaScript
