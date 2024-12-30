Setting Up Metabase with Docker

This guide provides step-by-step instructions to set up Metabase using Docker. Follow the appropriate sections based on your requirements, such as running Metabase locally, setting it up for production, or configuring advanced options.

Running Metabase on Docker

Prerequisites

Ensure Docker is installed and running on your system.

Open Source Quick Start
	1.	Pull the latest Metabase Docker image:

docker pull metabase/metabase:latest


	2.	Start the Metabase container:

docker run -d -p 3000:3000 --name metabase metabase/metabase


	3.	Access Metabase:
Open http://localhost:3000 in your browser.
	4.	Optional - View logs during initialization:

docker logs -f metabase


	5.	Run Metabase on a custom port:
Replace 12345 with your desired port:

docker run -d -p 12345:3000 --name metabase metabase/metabase

Pro or Enterprise Quick Start
	1.	Pull the latest Enterprise Docker image:

docker pull metabase/metabase-enterprise:latest


	2.	Start the Enterprise Metabase container:

docker run -d -p 3000:3000 --name metabase metabase/metabase-enterprise


	3.	Access Metabase:
Open http://localhost:3000 in your browser.
	4.	Optional - View logs during initialization:

docker logs -f metabase


	5.	Run Metabase on a custom port:

docker run -d -p 12345:3000 --name metabase metabase/metabase-enterprise

Production Installation

Metabase uses an embedded H2 database by default, which is not suitable for production. To ensure data persistence, use a production-grade database like PostgreSQL.

Steps for Production Setup
	1.	Set up a PostgreSQL database:
Example command to create a database:

createdb metabaseappdb


	2.	Run Metabase with PostgreSQL:
Replace placeholders with your database credentials:

docker run -d -p 3000:3000 \
  -e "MB_DB_TYPE=postgres" \
  -e "MB_DB_DBNAME=metabaseappdb" \
  -e "MB_DB_PORT=5432" \
  -e "MB_DB_USER=username" \
  -e "MB_DB_PASS=password" \
  -e "MB_DB_HOST=my-database-host" \
  --name metabase metabase/metabase

Example Docker Compose File

Here's an example docker-compose.yml for running Metabase with PostgreSQL:

version: "3.9"
services:
  metabase:
    image: metabase/metabase:latest
    container_name: metabase
    ports:
      - 3000:3000
    environment:
      MB_DB_TYPE: postgres
      MB_DB_DBNAME: metabaseappdb
      MB_DB_PORT: 5432
      MB_DB_USER: metabase
      MB_DB_PASS: mysecretpassword
      MB_DB_HOST: postgres
    networks:
      - metanet1
  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_USER: metabase
      POSTGRES_DB: metabaseappdb
      POSTGRES_PASSWORD: mysecretpassword
    networks:
      - metanet1
networks:
  metanet1:
    driver: bridge

Run this with:

docker-compose up -d

Persisting Data
	1.	Mount a local directory:

docker run -d -p 3000:3000 \
  -v ~/metabase-data:/metabase-data \
  -e "MB_DB_FILE=/metabase-data/metabase.db" \
  --name metabase metabase/metabase


	2.	Backup Metabase data:
Copy the embedded database file from a container:

docker cp CONTAINER_ID:/metabase.db ./

Advanced Configuration

Customizing the Java Timezone

Specify a timezone for consistent reporting:

docker run -d -p 3000:3000 \
  -e "JAVA_TIMEZONE=US/Pacific" \
  --name metabase metabase/metabase

Using Docker Secrets

Use secrets for sensitive parameters. Example docker-compose.yml:

services:
  metabase:
    environment:
      MB_DB_USER_FILE: /run/secrets/db_user
      MB_DB_PASS_FILE: /run/secrets/db_password
    secrets:
      - db_user
      - db_password
secrets:
  db_user:
    file: ./db_user.txt
  db_password:
    file: ./db_password.txt

Troubleshooting
	•	View logs:

docker logs -f metabase


	•	Restart stopped container:

docker start CONTAINER_ID


	•	Recreate a custom image:

docker commit CONTAINER_ID custom-metabase
docker run -d -p 3000:3000 custom-metabase



For additional troubleshooting, refer to the official Metabase documentation. 