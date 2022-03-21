RANGE := 192.168.100.0/24
SUBNET := 192.168.100.0/24
NAME := labnet

net:
	@echo "> Create network $(NAME) if it does not exists"
	@if [ "$$(docker network ls | tail -n +2 | awk '{ print $$2 }' | grep -o labnet)" = "x$(NAME)" ]; then \
	  docker network create -d bridge --subnet $(SUBNET) --ip-range $(RANGE) $(NAME); \
	else \
	  echo "> Network $(NAME) exists"; \
	fi	  

