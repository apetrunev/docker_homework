RANGE := 192.168.100.0/24
SUBNET := 192.168.100.0/24
NET_NAME := labnet

DB_NAME := django
DB_USER := django
DB_PASSWORD := django

net:
	@echo "> Create network $(NET_NAME) if it does not exists"
	@if [ "x$$(docker network ls | tail -n +2 | awk '{ print $$2 }' | grep -o labnet)" != "x$(NET_NAME)" ]; then \
	  docker network create -d bridge --subnet $(SUBNET) --ip-range $(RANGE) $(NET_NAME); \
	else \
	  echo "> Network $(NET_NAME) exists"; \
	fi	  

database: net
	@if [ "x$$(docker ps --filter name='^database$$' | grep -o database)" != "xdatabase" ]; then \
	  docker run -d \
	    --name database \
	    -e POSTGRES_DB=$(DB_NAME) \
	    -e POSTGRES_USER=$(DB_USER) \
	    -e POSTGRES_PASSWORD=$(DB_PASSWORD) \
	    --network $(NET_NAME) \
	    postgres:14-alpine; \
	else \
	  echo "> Container $@ already exists"; \
	fi

backend: net database
	@echo "> Building the backend"
	docker build -t backend .
	@if [ "x$$(docker ps --filter name='^backend$$' | grep -o backend)" != "xbackend" ]; then \
	  sleep 3 && docker run -d \
	    -p 8000:8000 \
	    --name backend \
	    --network $(NET_NAME) \
	    backend:latest; \
	else \
	  echo "> Container $@ already exists"; \
	fi

