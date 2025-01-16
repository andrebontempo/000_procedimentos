# Instalação e Configuração do MariaDB no Debian 12

Este guia detalha o processo para instalar e configurar o MySQL no Debian 12, criando um usuário administrador para gerenciar o banco de dados.

## Passo 1: Atualizar os Pacotes do Sistema
Antes de começar, certifique-se de que todos os pacotes do sistema estejam atualizados:
```bash
apt install sudo
```
```bash
usermod -aG sudo andre
```
```bash
apt update && sudo apt upgrade -y
```
Agora faça a conexão via SSH, isto facilita as configurações pois usa o terminal local.
```bash
ssh andre@<servidor>
```
## Passo 2: Instalar o MySQL Server
Instale o pacote do MySQL Server usando o gerenciador de pacotes APT:
PARA MARIADB
```bash
apt install mariadb-server -y
```
OU PARA MySQL
```bash
sudo apt install mysql-server -y
```

> **Nota:** O Debian 12 utiliza os repositórios oficiais do MySQL. O pacote instalado será a versão mais estável disponível.

## Passo 3: Verificar o Status do MySQL
Certifique-se de que o serviço mariadb está ativo e configurado para iniciar automaticamente com o sistema:
```bash
systemctl status mariadb
```
Se não estiver ativo, inicie e habilite o serviço:
```bash
systemctl start mariadb
systemctl enable mariadb
```
## Passo 4: Configurar o mariaDB com `mysql_secure_installation`
Execute o script de configuração inicial para melhorar a segurança do MySQL:
```bash
mysql_secure_installation
```
### Durante o processo:
1. Configure ou altere a senha do usuário `root`.
2. Responda `Y` (Yes) para:
   - Remover usuários anônimos.
   - Restringir o acesso remoto ao usuário `root`.
   - Remover o banco de dados de teste.
   - Recarregar as tabelas de privilégios.

## Passo 5: Acessar o MySQL como Root
Acesse o console do MySQL com o usuário `root` senha `1234`:
```bash
mysql -u root -p
```
Insira a senha configurada no passo anterior.
## Passo 6: Criar um Usuário Administrador
No console do MySQL, crie um novo usuário com permissões administrativas completas:

```sql
CREATE USER 'andredb'@'%' IDENTIFIED BY 'andredb1234';
GRANT ALL PRIVILEGES ON *.* TO 'andredb'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

> **Substitua** `adm_user` pelo nome desejado para o usuário e `sua_senha_segura` por uma senha forte.

## Passo 7: Configurar o Acesso Remoto

Primeiro, se não tiver, fixe o IP do servidor

```bash
nano /etc/network/interfaces
```
```bash
auto eth0
iface eth0 inet static
    address 192.168.0.69
    netmask 255.255.255.0
    gateway 192.168.0.1
    dns-nameservers 8.8.8.8 8.8.4.4
```




Para permitir o acesso remoto ao MySQL, edite o arquivo de configuração:

Para conceder acesso remoto para um usuário:

```bash
GRANT ALL PRIVILEGES ON *.* TO 'andredb'@'%' IDENTIFIED BY 'andredb1234';
FLUSH PRIVILEGES;
```
Para mostrar permissões de um usuário
```bash
SHOW GRANTS FOR 'andredb'@'%';
```
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
No maria DB é este o arquivo:
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Encontre a linha abaixo e comente-a (adicione `#` no início):
ou
Encontre a linha abaixo e substitua para 0.0.0.0

```ini
bind-address = 127.0.0.1
```

Salve e saia do editor (Ctrl + O, Enter, Ctrl + X).

Reinicie o serviço MySQL para aplicar as alterações:

```bash
sudo systemctl restart mysql
```

Certifique-se de que as regras do firewall permitem conexões à porta padrão do MySQL (3306):

```bash
sudo ufw allow 3306
```

## Passo 8: Testar o Acesso Local
Teste o acesso com o novo usuário criado localmente:

```bash
mysql -u adm_user -p
```

## Passo 9: Testar o Acesso Remoto
De outro computador na mesma rede ou com acesso ao servidor, teste o acesso remoto ao MySQL:

```bash
mysql -u adm_user -p -h <IP_DO_SERVIDOR>
```

> **Nota:** Substitua `<IP_DO_SERVIDOR>` pelo endereço IP do servidor MySQL. 

Se o acesso funcionar, o MySQL está corretamente configurado para conexões remotas.

## Conclusão
O MySQL está instalado e configurado no Debian 12, com um usuário administrador pronto para uso e suporte a acesso remoto (se configurado). Certifique-se de usar uma senha segura e gerenciar os acessos ao servidor adequadamente para garantir a segurança dos dados.
```

Para acessar um servidor MariaDB remotamente via SSH, você precisa configurar tanto o servidor MariaDB quanto o cliente local. Siga as etapas abaixo:

---

### 1. **Configurar o Servidor MariaDB**

#### a) **Editar o Arquivo de Configuração do MariaDB**
1. Acesse o servidor onde o MariaDB está instalado.
2. Edite o arquivo de configuração principal do MariaDB:
   ```bash
   sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
   ```
   Ou, dependendo da sua instalação:
   ```bash
   sudo nano /etc/my.cnf
   ```

3. Verifique e altere a seguinte linha para que o MariaDB escute em todos os endereços IP (não apenas `localhost`):
   ```ini
   bind-address = 0.0.0.0
   ```
   > ⚠️ Certifique-se de aplicar regras de firewall e restrições de acesso para segurança.

#### b) **Conceder Permissões de Acesso ao Usuário**
1. Acesse o MariaDB como usuário root:
   ```bash
   sudo mysql -u root -p
   ```

2. Conceda permissões de acesso remoto ao usuário:
   ```sql
   GRANT ALL PRIVILEGES ON *.* TO 'usuario'@'%' IDENTIFIED BY 'senha' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```
   > Substitua `'usuario'` e `'senha'` pelos valores apropriados.

#### c) **Abrir Porta no Firewall**
1. Permita a porta 3306 (padrão do MariaDB):
   ```bash
   sudo ufw allow 3306/tcp
   ```

---

### 2. **Configurar Acesso Via SSH**

#### a) **Configurar o Servidor SSH**
1. Verifique se o SSH está ativo no servidor:
   ```bash
   sudo systemctl status ssh
   ```
   Se não estiver ativo, inicie e ative o SSH:
   ```bash
   sudo systemctl start ssh
   sudo systemctl enable ssh
   ```

2. Certifique-se de que o cliente tenha acesso ao servidor via SSH:
   ```bash
   ssh usuario@ip-do-servidor
   ```
   Substitua `usuario` e `ip-do-servidor` pelos valores corretos.

#### b) **Criar um Túnel SSH**
No cliente local, crie um túnel SSH para redirecionar o tráfego da porta local para o MariaDB no servidor remoto:
```bash
ssh -L 3307:127.0.0.1:3306 usuario@ip-do-servidor
```
Neste caso:
- **3307**: Porta local usada para conectar.
- **127.0.0.1:3306**: Endereço e porta do MariaDB no servidor.
- **usuario**: Usuário SSH do servidor.
- **ip-do-servidor**: Endereço IP do servidor.

---

### 3. **Testar a Conexão Localmente**

#### a) **Usando o Cliente MySQL**
Conecte ao MariaDB usando o túnel SSH criado:
```bash
mysql -u usuario -p -h 127.0.0.1 -P 3307
```
- Substitua `usuario` pelo nome do usuário MariaDB configurado.

#### b) **Usando um Cliente GUI**
Você pode usar ferramentas como **DBeaver**, **MySQL Workbench** ou **HeidiSQL**:
1. Configure uma nova conexão com as seguintes informações:
   - **Host**: 127.0.0.1
   - **Port**: 3307 (ou outra porta local usada no túnel)
   - **Username**: Usuário do MariaDB.
   - **Password**: Senha do MariaDB.

---

### 4. **Testes Adicionais**

#### a) **Ping na Porta do MariaDB**
No servidor local, teste se o MariaDB está acessível:
```bash
telnet 127.0.0.1 3306
```

#### b) **Teste de Firewall**
Garanta que a porta está acessível externamente:
```bash
nc -zv ip-do-servidor 3306
```

#### c) **Logs do MariaDB**
Se houver problemas, verifique os logs:
```bash
sudo tail -f /var/log/mysql/error.log
```

---

### 5. **Boas Práticas de Segurança**
- Use autenticação SSH por chave para evitar senhas no túnel SSH.
- Configure um firewall para permitir conexões apenas de IPs autorizados.
- Utilize SSL para proteger conexões diretas ao MariaDB (opcional).

Esses passos devem garantir acesso remoto ao servidor MariaDB via SSH com testes eficazes de conectividade.


