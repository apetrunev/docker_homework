RANGE := 192.168.100.0/24
SUBNET := 192.168.100.0/24
NET_NAME := labnet

DB_NAME := django
DB_USER := django
DB_PASSWORD := django

net:
	@echo "> Create network $(NET_NAME) if it does not exists"
	@if [ "$$(docker network ls | tail -n +2 | awk '{ print $$2 }' | grep -o labnet)" = "x$(NET_NAME)" ]; then \
	  docker network create -d bridge --subnet $(SUBNET) --ip-range $(RANGE) $(NET_NAME); \
	else \
	  echo "> Network $(NET_NAME) exists"; \
	fi	  

db:
	docker run -d \
		--name database \
		-e POSTGRES_DB=$(DB_NAME) \
		-e POSTGRES_USER=$(DB_USER) \
		-e POSTGRES_PASSWORD=$(DB_PASSWORD) \
		--network $(NET_NAME) \
		postgres:14-alpine
