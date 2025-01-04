#Autor: Robson Vaamonde<br>
#Procedimentos em TI: http://procedimentosemti.com.br<br>
#Bora para Prática: http://boraparapratica.com.br<br>
#Robson Vaamonde: http://vaamonde.com.br<br>
#Facebook Procedimentos em TI: https://www.facebook.com/ProcedimentosEmTi<br>
#Facebook Bora para Prática: https://www.facebook.com/BoraParaPratica<br>
#Instagram Procedimentos em TI: https://www.instagram.com/procedimentoem<br>
#YouTUBE Bora Para Prática: https://www.youtube.com/boraparapratica<br>
#Data de criação: 30/01/2023<br>
#Data de atualização: 28/07/2024<br>
#Versão: 0.10<br>
#Testado e homologado no Linux Mint 20 Ulyana, 20.1 Ulyssa, 20.2 Uma e 20.3 Una x64<br>
#Testado e homologado no Linux Mint 21 Vanessa, 21.1 Vera, 21.2 Victoria e 21.3 Virginia x64<br>
#Testado e homologado no Linux Mint 22 Wilma x64<br>

#Instalação do MongoDB Server 7.x no Linux Mint 20.1 Ulyssa, 20.2 Uma e 20.3 Una x64<br>
#Instalação do MongoDB Server 7.x Linux Mint 21 Vanessa, 21.1 Vera, 21.1 Vera, 21.2 Victoria e 21.3 Virginia x64
#Instalação do MongoDB Server 7.x no Linux Mint 22 Wilma x64.

[![MongoDB Server](http://img.youtube.com/vi/JUWuyTHvLVY/0.jpg)](https://www.youtube.com/watch?v=JUWuyTHvLVY "MongoDB Server")

Link da vídeo aula: https://www.youtube.com/watch?v=JUWuyTHvLVY

Site Oficial do MongoDB: https://www.mongodb.com/<br>
Site Oficial do MongoDB Compass: https://www.mongodb.com/products/compass

O QUE É E PARA QUE SERVER O MONGODB: MongoDB é um software de banco de dados orientado a documentos<br>
livre, de código aberto e multiplataforma, escrito na linguagem C++. Classificado como um programa<br>
de banco de dados NoSQL, o MongoDB usa documentos semelhantes a JSON com esquemas.

#00_ Verificando as Informações do Sistema Operacional Linux Mint<br>
```bash
#atalho para acessar o Terminal
Terminal: Ctrl + Alt + T

#verificando as versões e codinome do sistema operacional
#OBSERVAÇÃO IMPORTANTE: Linux Mint 20.x é derivado do Ubuntu Desktop 20.04.x Focal Fossa
#OBSERVAÇÃO IMPORTANTE: Linux Mint 21.x é derivado do Ubuntu Desktop 22.04.x Jammy Jellyfish
#OBSERVAÇÃO IMPORTANTE: Linux Mint 22.x é derivado do Ubuntu Desktop 24.04.x Noble Numbat
sudo cat /etc/os-release
sudo cat /etc/lsb-release

#verificando informações de localidade
sudo localectl

#modo gráfico para verificar as informações de sistema operacional e hardware
Menu
	Informações do Sistema
```

#01_ Atualização do Sistema Operacional Linux Mint<br>
```bash
#atualizando o sistema operacional via MintUpdate (Recomendado)
_ Atualização do sistema utilizando o MintUpdate;
_ Atualização do sistema utilizando o Apt;

#atualizando o sistema operacional via Terminal
#atalho para acessar o Terminal
Terminal: Ctrl + Alt + T

#recomendo utilizando o comando: apt - o comando: apt-get e considerado obsoleto
sudo apt update
sudo apt upgrade
sudo apt full-upgrade
sudo apt dist-upgrade
sudo apt autoremove
sudo apt autoclean
sudo
```

#02_ Instalando as Dependências do MongoDB Server no Linux Mint<br>
```bash
#INSTALANDO AS DEPENDÊNCIAS DO MONGODB SERVER NO LINUX MINT 20.x
sudo apt install git vim build-essential software-properties-common gnupg apt-transport-https ca-certificates

#INSTALANDO AS DEPENDÊNCIAS DO MONGODB SERVER NO LINUX MINT 21.x
#opção do comando dpkg: -i (install)
sudo apt install git vim build-essential software-properties-common gnupg apt-transport-https ca-certificates

#OBSERVAÇÃO IMPORTANTE: o tempo todo a Biblioteca LibSSL sofre alteração de versão, antes de baixar a versão
#acesse o site: http://nz2.archive.ubuntu.com/ubuntu/pool/main/o/openssl/ e veja qual a versão atual, altere
#o script e faço o download.

#download da biblioteca LibSSL1.1.x
wget http://nz2.archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.22_amd64.deb

#instalação da biblioteca LibSSL em modo manual
#opção do comando dpkg: -i (install) (link atualizado no dia: 06/03/2024)
sudo dpkg -i libssl1.1_1.1.1*.deb
```

#03_ Baixando e instalando a Chave GPG do MongoDB Server no Linux Mint<br>
```bash
#opção do comando curl: -f (fail), -s (silent), -S (show-error), -L (location)
#opção do redirecionador |: Conecta a saída padrão com a entrada padrão de outro comando
#opção do comando gpg: -o (output)
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-7.0.gpg
```

#04_ Criando o repositório do MongoDB Server no Linux Mint<br>
```bash
#ADICIONANDO O REPOSITÓRIO DO MONGODB SERVER NO LINUX MINT 20.x
#opção do redirecionador |: Conecta a saída padrão com a entrada padrão de outro comando
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

#ADICIONANDO O REPOSITÓRIO DO MONGODB SERVER NO LINUX MINT 21.x
#opção do redirecionador |: Conecta a saída padrão com a entrada padrão de outro comando
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

#05_ Atualizando as Lista do Apt com o novo Repositório do MongoDB no Linux Mint<br>
```bash
#atualizando o sources list com o novo repositório
sudo apt update
```

#06_ Instalando o MongoDB Server e Client no Linux Mint<br>
```bash
#instalação do MongoDB
sudo apt install mongodb-org
```

#07_ Habilitando o Serviço do MongoDB Server no Linux Mint<br>
```bash
#iniciando o serviço do MongoDB
sudo systemctl daemon-reload
sudo systemctl enable mongod
sudo systemctl start mongod
```

#08_ Verificando o Serviço e Versão do MongoDB Server no Linux Mint<br>
```bash
#verificando o Status de Serviço do MongoDB
sudo systemctl status mongod
sudo systemctl restart mongod
sudo systemctl stop mongod
sudo systemctl start mongod

#verificando a versão MongoDB
mongod --version
mongosh --version
```

#09_ Verificando a Porta de Conexão do MongoDB Server no Linux Mint<br>
```bash
#verificando a porta padrão do MongoDB
#opção do comando lsof: -n (network number), -P (port number), -i (list IP Address), -s (alone directs)
sudo lsof -nP -iTCP:'27017' -sTCP:LISTEN
```

#10_ Localização dos Arquivos de Configuração do MongoDB Server no Linux Mint<br>
```bash
/etc/mongod.conf   <-- arquivo de configuração do MongoDB Server
/var/log/mongodb   <-- diretório dos arquivos de Log do MongoDB Sever
/var/lib/mongodb   <-- diretório dos arquivos de Banco de Dados do MongoDB Server
```

#11_ Adicionado o Usuário Local no Grupo Padrão do MongoDB Server no Linux Mint<br>
```bash
#opções do comando usermod: -a (append), -G (groups), $USER (environment variable)
sudo usermod -a -G mongodb $USER
newgrp mongodb
id

#recomendado reinicializar a máquina para aplicar as permissões
sudo reboot
```

#12_ Testando a Conexão Local com o MongoDB Server o Linux Mint<br>
```bash
#mongodb shell padrão
mongosh
```

#13_ Comandos Básicos do MongoDB Server no Linux Mint<br>
```bash
#exibir os bancos de dados existentes no MongoDB
show dbs

#alterar o database informe no MongoDB
use admin

#listar o database informe atual no MongoDB
db

#exibir os collections do database informe atual no MongoDB
show collections

#sair do MongoDB
quit
```

#14_ Criando o usuário de administração do MongoDB Server no Linux Mint<br>
```bash
mongosh

#alterar o database informe no MongoDB
use admin

#criando o usuário admin e suas roles do MongoDB
db.createUser(
{
	user: "admin",
	pwd: "pti@2018",
	roles: [ "userAdminAnyDatabase", "dbAdminAnyDatabase", "readWriteAnyDatabase" ]
}
)

#visualizando os usuários do MongoDB
db.getUsers()

#saindo do MongoDB
exit
```

#15_ Configurando o MongoDB Server para suportar autenticação e acesso Remoto no Linux Mint<br>
```bash
#editando o arquivo de configuração do MongoDB
sudo vim /etc/mongod.conf
INSERT
	
	#habilitando o suporte remoto do MongoDB Server
	#alterar a linha: bindIp: 127.0.0.1 para: bindIp: 0.0.0.0
	net:
		port: 27017
		bindIp: 0.0.0.0
	
	#habilitando o recurso de autenticação do MongoDB Server
	#descomentar a linha: #security, adicionar o valor: authorization: enabled
	security:
		authorization: enabled

#salvar e sair do arquivo
ESC SHIFT :x <ENTER>

#reiniciar o serviço do MongoDB
sudo systemctl restart mongod
sudo systemctl status mongod
```

#16_ Acessando o MongoDB com e sem autenticação no Linux Mint<br>
```bash
mongosh

#exibir os bancos de dados existentes no MongoDB
show dbs

#saindo do MongoDB Server
quit

#opção do comando mongosh: admin (database) -u (username), -p (password)
mongosh admin -u admin -p

#exibir os bancos de dados existentes no MongoDB
show dbs

#saindo do MongoDB Server
quit
```

#17_ Download e Instalação do MongoDB Compass no Linux Mint<br>
```bash
#Link atualizado em: 29/07/2024
Link Oficial do MongoDB Compass: https://downloads.mongodb.com/compass/mongodb-compass_1.43.4_amd64.deb

01) Na pasta de Download, clicar duas vezes no Instalador do MongoDB Compass;
02) Seguir os procedimentos na tela.

#criando uma conexão com o MongoDB
Menu
	Pesquisa Indexada: MongoDB Compass
		New connection +
			Advanced Connection Options
				Authentication
					Authentication Method: Username/Password
						Username: admin
						Password: pti@2018
						Authentication Database: admin
						Authentication Mechanism: Default
		<Save>
```

#18_ Integrando o MongoDB Server com o Visual Studio Code VSCode no Linux Mint<br>
```bash
#instalação da Extensão do MongoDB no VSCode
VSCode
	Extensões
		Pesquisar
			MongoDB for VS Code
				Instalar

#configuração da conexão do MongoDB no VSCode 
VSCode
	MongoDB
		CONNECTIONS
			Add Connection
				Advanced Connection String: <Open From>
					New Connection
						General
							Connection Type: Standalone
							Hostname: localhost
							Port: 27017
							Authentication: Username/Password
								Username: admin
								Password: pti@2018
								Authentication Database: admin
					<Connect>
				<Close>
```