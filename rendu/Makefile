name = Inception

data = ~/data
wp_data = ~/data/wordpress
m_data = ~/data/mariadb

all	: | $(data) $(wp_data) $(m_data)
	@make up

$(data)	:
	@echo "Folder $(data) does not exist, creating..."
	mkdir -p $@

$(wp_data)	:
	@echo "Folder $(wp_data) does not exist, creating..."
	mkdir -p $@

$(m_data)	:
	@echo "Folder $(m_data) does not exist, creating..."
	mkdir -p $@

clean	:	down
	@printf "Removing unused ${name} images...\n"
	@docker system prune -a
	@sudo rm -rf $(wp_data) $(m_data)

fclean	:
	@printf "Deep clean of docker\n"
	@make stop
	@docker system prune --all --force --volumes
	@docker network prune --force
	@docker volume prune --force
	@sudo rm -rf $(wp_data) $(m_data)

re	: | $(data) $(wp_data) $(m_data)
	@printf "docker-compose up -d --build : Building ${name}'s Configuration...\n"
	@docker-compose -f srcs/docker-compose.yml --env-file srcs/.env up -d --build
	@make dbrestore
	@make list

build	:	re

up	:
	@printf "docker-compose up -d : Starting project ${name}...\n"
	@docker-compose -f srcs/docker-compose.yml --env-file srcs/.env up -d
	@make dbrestore
	@make list

down	:
	@printf "docker-compose down : Shutdown project ${name}...\n"
	@docker-compose -f srcs/docker-compose.yml --env-file srcs/.env down

list 	:
	@printf "Listing containers :\n"
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env ps

logs	:
	@printf "Checking LOGs :\n"
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env logs

pause	:
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env pause

unpause	:
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env unpause

stop	:
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env stop

start	:
	docker-compose -f srcs/docker-compose.yml --env-file srcs/.env start

#UTILS	:
#show databases;
#show grants for 'root'@'localhost';
#show grants for 'rahmed'@'%';
#SELECT User FROM mysql.user;

dbroot	:
	docker exec -it mariadb mysql -u root -p

dbuser	:
	docker exec -it mariadb mysql -u rahmed -p

dblist	:
	docker exec -it mariadb ls

dbdump	:
	docker exec mariadb mysqldump  wordpress > ./srcs/requirements/mariadb/conf/wordpress.dump

dbrestore :
	@docker exec -i mariadb  mysql wordpress < srcs/requirements/mariadb/conf/wordpress.dump

nlist	:
	docker exec -it nginx ls

wplist	:
	docker exec -it wordpress ls

.PHONY	: dbroot dbuser dblist dbdump dbrestore nlist wplist all clean fclean re build up down list logs pause unpause stop start
