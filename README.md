# Langchain

## Why Langchain?


## How are we using Langchain?
We built a flask server that makes use of langchain framework to interact with openai. Thus, instead of directly sending HTTP requests
to openai API, we now send requests to langchain server which forwards these requests (with langchain data) to openai API. 

HTTP Request: Amotions-Web -> langchain server API -> openai API
HTTP Response: openai API -> langchain server API -> Amotions-Web

## How was langchain server created and hosted?
I created 


## Pull and Make Changes:

1. Clone
