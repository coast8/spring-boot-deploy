
##  Exportando o projeto no eclipse
	Project/RUN AS/Maven Install


## JAR compilado e pronto para executar. Mas, para rodá-lo, precisamos dar permissão de execução a este arquivo:
	chmod +x harbor-backend.jar


## Executando projeto no braço
	java -jar arquivo.jar


## Agora, vamos configurar um serviço Linux apontando para esse JAR utilizando systemd. Para isso, criamos um arquivo dentro de “/etc/systemd” com o nome do nosso projeto e com extensão “.service”:
	vim /etc/systemd/system/harbor-backend.service


## Com o seguinte conteúdo:
	[Unit]
	Description=Harbor Backend Spring Boot
	After=syslog.target
	After=network.target[Service]
	User=harbor
	Type=simple

	[Service]
	ExecStart=/usr/bin/java -jar <path-projeto>/target/harbor-backend.jar
	Restart=always
	StandardOutput=syslog
	StandardError=syslog
	SyslogIdentifier=harbor-backend

	[Install]
	WantedBy=multi-user.target


## Depois de criado o arquivo, reinicie os serviços do Linux:
	systemctl daemon-reload


## E agora, podemos usar este comando para ver o status do nosso serviço java:
	sudo systemctl status harbor-backend


## Podemos ainda iniciar (start), parar (stop) e reiniciar (restart) caso necessário. Para atualizar o projeto backend, basta mexer dentro do projeto, pois o serviço criado está apontando diretamente para lá. Assim, vá a raiz do projeto backend e faça os comando em sequência:
	sudo systemctl stop harbor-backend
	git pull <url-harbor-backend>
	mvn clean install
	sudo systemctl start harbor-backend



## Fontes:
	https://blog.caelum.com.br/publicando-springboot-e-frontend-em-producao/
