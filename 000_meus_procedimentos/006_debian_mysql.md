# Segue o procedimento atualizado, incluindo o teste para o acesso remoto antes da conclusão:  

```markdown
# Instalação e Configuração do MySQL no Debian 12

Este guia detalha o processo para instalar e configurar o MySQL no Debian 12, criando um usuário administrador para gerenciar o banco de dados.

## Passo 1: Atualizar os Pacotes do Sistema
Antes de começar, certifique-se de que todos os pacotes do sistema estejam atualizados:

```bash
sudo apt update && sudo apt upgrade -y
```

## Passo 2: Instalar o MySQL Server
Instale o pacote do MySQL Server usando o gerenciador de pacotes APT:

```bash
sudo apt install mysql-server -y
```

> **Nota:** O Debian 12 utiliza os repositórios oficiais do MySQL. O pacote instalado será a versão mais estável disponível.

## Passo 3: Verificar o Status do MySQL
Certifique-se de que o serviço MySQL está ativo e configurado para iniciar automaticamente com o sistema:

```bash
sudo systemctl status mysql
```

Se não estiver ativo, inicie e habilite o serviço:

```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Passo 4: Configurar o MySQL com `mysql_secure_installation`
Execute o script de configuração inicial para melhorar a segurança do MySQL:

```bash
sudo mysql_secure_installation
```

### Durante o processo:
1. Configure ou altere a senha do usuário `root`.
2. Responda `Y` (Yes) para:
   - Remover usuários anônimos.
   - Restringir o acesso remoto ao usuário `root`.
   - Remover o banco de dados de teste.
   - Recarregar as tabelas de privilégios.

## Passo 5: Acessar o MySQL como Root
Acesse o console do MySQL com o usuário `root`:

```bash
sudo mysql -u root -p
```

Insira a senha configurada no passo anterior.

## Passo 6: Criar um Usuário Administrador
No console do MySQL, crie um novo usuário com permissões administrativas completas:

```sql
CREATE USER 'adm_user'@'%' IDENTIFIED BY 'sua_senha_segura';
GRANT ALL PRIVILEGES ON *.* TO 'adm_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

> **Substitua** `adm_user` pelo nome desejado para o usuário e `sua_senha_segura` por uma senha forte.

## Passo 7: Configurar o Acesso Remoto
Para permitir o acesso remoto ao MySQL, edite o arquivo de configuração:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Encontre a linha abaixo e comente-a (adicione `#` no início):

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
