## AI DRIVEN SQL GENERATING CHATBOT APPLICATION:
* Generative AI Powered Application is the integration of Spring AI Framework with Spring Boot Application.
* Integration of Artificial Intelligence and Machine Learning into Spring-based application.
* Spring AI project aims to simplify the development of applications that incorporate artificial intelligence functionality without unnecessary complexity.
* This technique was introduced to help the developers in connecting with LLM for decision-making, solving task etc.

## OBJECTIVE:
Task is to create AI SQL GENERATING CHATBOT Application having Java Spring boot application as backend which connects with the AI technologies.
Application that accepts natural language queries and generates valid MySQL SELECT queries based on the world database schema.
The chatbot will utilize a backend built using Spring Boot and integrate an AI model to accurately interpret user input and translate it into precise SQL commands.

## FEATURES NEEDED TO BE INCLUDED:
* Read the schema.sql file containing the database's DDL (Data Definition Language) and use it as the reference for all SQL queries generated.
* Define an appropriate system instruction that aligns with the workshop objective to guide the AI model in generating correct SQL queries.
* Fine-tune their chatbot by testing with varied inputs, analysing results, and iteratively optimizing responses.
* (Optional): Configure the application to execute generated SQL queries against a database and return the results as JSON.

### **`Input:`** User asks to generate MySQL SELECT queries.
### **`Spring boot application:`** Customized Prompts are created and configured with System instructions & LLM parameter Settings.
### **`Output:`** If the user asks for MySQL SELECT queries, Open AI model will respond with queries. Then Spring boot application establishes JDBCConnection to execute the query and provide executed data. Otherwise, show appropriate error message.

## STEPS implemented in AI Driven Spring Application:
1. Spring boot application is created with Gradle as build automation tool. Necessary dependencies are added.
2. Properties file:
    * Spring application connects with Open AI's predefined model gpt-4o.
    * Added the necessary configuration properties to connect with Open AI model (API key, max-attempts, model name, temperature, frequency-penalty, presence-penalty).
      API Key is added as environmental variable.
    * Configured the necessary LLM Fine Tune options to customize the model response.
3. Controller and Service classes are created. Taken necessary actions to implement the API endpoints inside these classes.
4. Adding System Information, Sending request to, Receiving response from Open AI chat model and Executing the select query is configured inside the Spring boot application using the following techniques:
    - **`ChatClient:`** Predefined Interface present inside org.springframework.ai.chat.client framework. It has pre-built methods to connect with Open AI Model is used in Service layer.
    - **`JDBCTemplate:`** Added the necessary dependency in build.gradle file to use this class in Service layer. It has pre-built methods to execute the MySQL query from Open AI model and return the response. 
    - Interact with AI functionalities using APIs like AI Model API and Advisors API. 
    1. API endpoint to Generate Query -> **`generateSQL()`**:
        - This endpoint interacts with Open AI model.
        - User question is passed as **`String`** through GET method.
        - Provided Template contains different sections like Role, Instruction, DDL and Question.
            * Role -> Role of the model is defined
            * Instruction -> If Request is valid, then respond with SELECT Query; 
                         Otherwise, throws "Table or Column does not exist" if table/column is invalid, 
                         "Operation isn't supported" for Operations like Insert, Update, Delete, Alter or 
                         "DDL doesn't support answering that question" for other irrelevant questions.  
            * DDL -> World Database Schema. Replace the prompt with data from Resource file, schema.sql.
            * Question -> User Question. Replace the prompt with user question.
        - ChatClient bean initialises new prompt session with call() method. So, response from Open AI model is returning as blocking one.
        - User and Application has to wait until the response is generated fully.
        - Generated MySQL select query/Error message is delivered as **`String`**.
    2. API endpoint to Generate and Execute Query -> **`executeSQL()`**:
        - This endpoint interacts with Open AI model and then execute the select query from Open AI model.
        - User question is passed as **`String`** through GET method.
        - Calls the generateSQL() method to get the MySQL Select Query.
        - Then executes the MySQL Select Query using method queryForList() in JDBCTemplate class.
        - Response is delivered as **`JSON using Record`** which contains SQLQuery(from Open AI model) and SQLResult(after query execution using JDBC Template).
        - Necessary Error message is handled through GlobalExceptionHandler class.

## Sample Request and Response through Postman:
![APIValidResponse.png](./images/1.APIValidResponse.png)
![APIErrorResponse.png](./images/2.APIErrorResponse.png)
