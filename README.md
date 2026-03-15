# airline-customer-support-system-
customer
line one
# new version availabele instead of npm install n8n -g
npm install -g n8n@1.117.3
source .env 
python -m venv venv 
source venv/bin/activate 
pip install llm-guard pandas psycopg2 
git add . 
git commit -m "Files added" 
git push 
docker run -it -d -p 5432:5432 -e POSTGRES_PASSWORD=mypassword --name=postgrescont postgres:latest 
# Once the container starts, move into the container: 
docker exec -it postgrescont psql -U postgres 
# Check the available databases list: 
SELECT datname FROM pg_database WHERE datistemplate = false; 
# Create a Postgres database - airlinedb: 
CREATE DATABASE airlinedb; 