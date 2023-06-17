# Langchain

## How are we using Langchain?
We built a flask server that makes use of langchain framework to interact with openai. Thus, instead of directly sending HTTP requests
to openai API, we now send requests to langchain server which forwards these requests (with langchain data) to openai API. 

HTTP Request: Amotions-Web -> langchain server API -> openai API
<br />
HTTP Response: openai API -> langchain server API -> Amotions-Web

## How was langchain server created?
I will break this into three parts, in each I will talk about a file in the repo.

1. ingestion.py
In this file, I simply upload data (e.g books pdfs), then we split the book semantically into small chunks, and then we upload these data chunks to the vector database Pinecone. Pinecone stores these chunks in a smart way by converting text chunks into vectors (vector is a list of numbers) using mathematical equations that we don't care about, but it does the job. textchunks -> vectors. After this file has finished running, the data will be stored in Pinecone vector store which is hosted on the cloud. This means we need to run ingestion once every time we upload new data. Note: Duplicate data will be ignored, so don't worry about running ingestion file mutiple times.

2. backend/core.py
In this file, we chat with the data stored on Pinecone by sending query questions. The way this works is that when you send a query question to the Pinecone vector store, Pinecone searches for vectores in the store that are semantically relevant to the question and return them to you with the question as a single query. Now, you have the question + context from the data you uploaded to Pinecone. You now can send this quesry to openai, which will use the context and its general data to answer your question.

3. application.py
In this file, we build the api that calls the function in core.py whenever a POST HTTP requests is sent to our server with the param query="Question". After core.py function has returend the answer to the question we send and HTTP response with the answer as json object.

## How was langchain hosted?
1. Created a virtual environment to build the server
2. Created a git repo and pushed it to github
3. Created the requirements.txt file by running pip freeze -> requirements.txt - This makes sure that the production environment will have all the packages you have locally on your machine, so make sure to run this command every time you install packages using the pip command 
4. Created YAML file (main.yml) to instruct the production environment, what comands need to be run to build the code.
5. Created AWS codePipline and codebuild for CI/CD
6. Created AWS Elastic Beanstalk to host the falsk server

## Pull and Make Changes:
1. Clone the repository 
2. Create you own branch
3. Create a virtual environment (I used pipenv, command: pipenv shell). You can use whatever you like
4. Install the requirements dependencies/packages using this command: pip install -r requirements.txt
5. Make changes and test locally before pushing to remote
6. run command: python application.py
7. Server should be running on your localhost, use the url given in the terminal to test with Postman
8. If everything is working correctly, then you can move to the next step
9. If you have installed any packages using the command pip install package_name, make sure you are in the virtual environment, then run the command pip freeze -> requirements.txt. This command will update the requirements.txt for production environment
10. You are ready to push now, commit your changes and push them to your branch. NOTE: Follow the instruction in amotions-web repo for how to merge and release new feature to production

## Note: If you have any question or need help, please ask in slack.
