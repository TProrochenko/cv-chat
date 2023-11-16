**What?** A chat with a GPT-based assistant that answers questions about my work experience:
https://tymur-prorochenko.chat/

**Why?** The idea of asking and getting an answer instead of searching for it
through a bunch of text in a CV seems cool. In addition, I never developed 
websites and do not know HTML / CSS / JS.

**How?** I heavily relied on GPT to write all the frontend and then played around 
with it until I liked the result. Everything is hosted and set up on AWS.

**Frontend:** Structure, style, and scripting are in one file `index.html`, 
located in a public S3 bucket, which is set up with Route 53 and CloudFront 
to be accessible from my domain. Simple JS code is responsible for receiving 
user input, storing chat history, and displaying user and assistant messages. 

**Backend:**
When a user sends a message, a POST request with all conversation history is 
directed to Lambda `lambda.py` via API Gateway. Lambda function reads a system 
prompt for the model on how to respond from `instructions.txt` in private S3 
bucket. These instructions also contain a text version of my CV. Everything is 
combined and sent to OpenAI via chat completion API. Openai response 
is parsed and the assistant answer is returned to be displayed to the user.
A lambda layer with zipped libraries from `requirements.txt` was set up 
manually.

**Deployment**
Every merge to master triggers a CodeBuild pipeline based on `buildspec.yml` 
that updates `index.html` on S3, `lambda.py` in Lambda and invalidates 
CloudFront cache for a faster page update.
