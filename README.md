# Airline Customer Support System

## Steps to Follow

1. Create a new GitHub repository

2. Start a new GitHub Codespace whithin the GitHub repo

3. Install N8N

4. Create Virtual Environment

5. Activate Virtual Environment & Install LLM-Guard

6. Start Postgres Container (docker is containrisation tool, you have libraries)
     docker -- version
    `docker run -it -d -p 5432:5432 -e POSTGRES_PASSWORD=mypassword --name=postgrescont postgres:latest`

    Make the port 5432 Public.

7. Move into the Postgres Container and Create database and table

    `docker exec -it postgrescont psql -U postgres`

    `SELECT datname FROM pg_database WHERE datistemplate = false;`

    `CREATE DATABASE airlinedb;`

    `\c airlinedb;`

    ```sql
    CREATE TABLE IF NOT EXISTS flights (
        id BIGINT PRIMARY KEY,
        flight_no TEXT NOT NULL,
        airline_code TEXT NOT NULL,
        airline_name TEXT NOT NULL,
        origin TEXT NOT NULL,
        destination TEXT NOT NULL,
        departure_scheduled TIMESTAMP NOT NULL,
        arrival_scheduled TIMESTAMP NOT NULL,
        departure_date DATE GENERATED ALWAYS AS (departure_scheduled::date) STORED,
        departure_time TIME GENERATED ALWAYS AS (departure_scheduled::time) STORED,
        arrival_date   DATE GENERATED ALWAYS AS (arrival_scheduled::date) STORED,
        arrival_time   TIME GENERATED ALWAYS AS (arrival_scheduled::time) STORED,
        status TEXT NOT NULL CHECK (status IN ('On Time','Delayed','Cancelled')),
        delay_minutes INT DEFAULT 0,
        delay_reason TEXT DEFAULT '',
        terminal TEXT,
        gate TEXT,
        aircraft_type TEXT,
        seats_total INT,
        seats_booked INT,
        fare_inr INT
    );
    ```
\dt 
SELECT * FROM flights LIMIT 10; 
quit
\q

8. Insert data into Postgres DB using Python script
   vscode terminal - python add_data_to_db.py 

                   SELECT * FROM flights LIMIT 10; 

10. Start N8N within your Virtual Environment

    npm install -g n8n@1.117.3(using guardrail  so we are using new install command)
   python --version
   source .env 
Create virtual environment 
python -m venv venv 
● Activate the virtual environment 
source venv/bin/activate 
● Install the required packages using below command: 
pip install llm-guard pandas psycopg2

upload 3 file in VS code ( guardrails.py 
It contains the guardrails function to test for toxicity, secrets, violence, and 
Prompt Injection. 
○ Flights_Schedule.csv 
It consists of rows that will be inserted into the PostgreSQL database. 
○ add_data_to_db.py 
This is the Python Script used to insert the CSV data into the database. )

Upload all 4 files on the GitHub Codespace, then commit, and push to remote repository 
git add . 
git commit -m "Files added" 
git push


After 30 minutes of inactivity the Codespace stops automatically. You may need to 
start it again by reloading the window, changing the port visibility to public, and 
running below commands again to restart N8N & PostgreSQL container: 
source .env 
source venv/bin/activate 
docker start postgrescont 
n8n start

