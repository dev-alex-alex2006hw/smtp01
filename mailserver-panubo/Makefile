docker_tag 	= mailserver

build:
	docker build -t $(docker_tag) .

bash:
	docker run --rm -it -e MAILNAME=mail.example.com -e MYDOMAIN=example.com -e DB_MAIL_HOST=foo -e DB_SQLGREY_HOST=foo -e GENERATE_TLS=true $(docker_tag) bash

run:
	docker run -d --name mariadb -e MYSQL_ROOT_PASSWORD='00zulu' mariadb:10.0
	$(eval ID := $(shell docker run -d --name mailserver --link mariadb:mariadb -e MAILNAME=smtp.clouderg.com -e MYDOMAIN=clouderg.com -e GENERATE_TLS=true ${docker_tag}))
	$(eval IP := $(shell docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${ID}))
	@echo "Running ${ID} @ ${IP}"
	@docker logs -f ${ID}
	@docker rm -f mariadb mailserver
