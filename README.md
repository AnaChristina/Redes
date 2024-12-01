# Redes

## Load Balance

indice 
* Oque é load balance
* Como configurar
* Tecnologias

O que é Load Balance?

Balanceamento de carga ou Load Balance é uma técnica utilizada para manter a estabilidade de um servidor quando o tráfego ou o volume de dados é muito grande. O ponto principal é otimizar o tráfego de informações e garantir o funcionamento de um sistema para o usuário, estabilizando a navegação.
O Load Balance tenta evitar que uma sobrecarga aconteça e melhora o desempenho do sistema. A ideia é distribuir as milhares de solicitações e dados dos usuários entre os diferentes servidores. Isso faz parte da gestão de infraestrutura de uma aplicação.


<center> (imagem exemplo) </center>


## Como Criar Load Balance

# Criando os serviços 

Utilizei as instâncias da AWS para criar o Load Balance, primeiramente criamos as instâncias: Serviço 1, Serviço 2 e Serviço 3. Estas instâncias serão nossos serviços que irão levantar na nuvem aws. 


configurações necessárias de segurança das instancias aws: 




Após a criação de cada uma delas, devemos instalar o NGINX, para isso segue o passo a passo:


- Conectar via SSH com a instânica:

       ssh -i /caminho/para/sua-chave.pem ec2-user@IP_PUBLICO
    ❗alterar usuario e ip para o usuario e ip publico da sua instância 



- Instalar o NGINX:

      sudo apt install nginx -y


Após a instalação do ngix, vamos criar um arquivo html para ser mostrado assim que subir o serviço. 
para criar use o seguinte comando: 

    echo "Esta é a Instância 1" | sudo tee /usr/share/nginx/html/index.html



Para verificar se está funcionando é so estartar o servidor NGINX e acessar o ip publico da sua instância: 

    sudo systemctl start nginx

Acessar endereço ip publico da sua instância:
    
    http://SEU_IP_PUBLICO_SERVICO
❗alterar usuario e ip para o usuario e ip publico da sua instância 



Repita este passo a passo em todas as instâncias ( Serviço 1, Serviço 2 e Serviço 3). 
---

# Configurando Load Balance

Agora que já instalamos o nignx e criamos os HTML, temos que criar a instância na aws que irá ser o nosso Load Balance. 

-  Criar uma instancia nova na AWS.
  
- Instalar o nginx:

      sudo apt install nginx -y

Assim que instalar o nginx, vamos iniciar ele e após isso temos que abrir o arquivo de configuração nginx.conf, utilize o comando: 

    sudo systemctl start nginx
    sudo nano /etc/nginx/nginx.conf


Agora vamos adicionar as seguintes configurações de balanceamento de carga:

        http {
            upstream backend {
                server IP_PRIVADO_SERVICO_1;
                server IP_PRIVADO_SERVICO_2;
                server IP_PRIVADO_SERVICO_3;
                .
                .
                .
                .
            } 
        }

Em ''upstream backend'' colocamos os servidores que irão ser alternados com o load balance. Salve e saia do arquivo. 

Teste as configurações no NGINX para verificar se está tudo correto, você pode usar o comando: 

    sudo nginx -t
se estiver tudo certo irá aparecer uma mensagem dizendo que está tudo OK. 

Reinicie o serviço: 

    sudo systemctl restart nginx
    
Acesse  o IP público da sua instância Load Balance: 

    http://SEU_IP_PUBLICO_LOADBALANCE

Ao acessar, deverá ser mostrado uma página HTML que criamos em uma das Instâncias SERVIÇO, se atualizar a página o HTML irá alternando entre as instâncias SERVIÇO 1, SERVIÇO 2 E SERVIÇO 3. 

Não se esqueça de verificar se as instâncias SERVIÇO 1, SERVIÇO 2 E SERVIÇO 3 estão rodando o NGINX. Caso não, é so conectar via ssh e usar comando para estartar o niginx em cada uma delas. 

    ssh -i /caminho/para/sua-chave.pem ec2-user@IP_PUBLICO
    sudo systemctl start nginx
    sudo systemctl status nginx



# Possíveis erros do Load Balance

Caso o seu Load Balance não esteja mostrando os HTML corretos ou não esteja alternando as páginas, pode ser que tenha uma configuração root que esteja apontando um caminho dentro da instância Load Balance, siga este passo a passo: 

acessar o diretório: /etc/nginx/sites-enabled/default
No arquivo default, procure por qualquer configuração de root que define um diretório local, como: root /var/www/html;
Comente ou remova essas linhas para evitar que o Nginx tente servir conteúdo local.
Salve as alterações e reinicie o Nginx e verificar status. 

Nas outras instancias, dar comando para executar o nginx também.
Acessar o navegador com o ip publico do load balance. 

Ao recarregar a página, será possível visualizar o serviço mudando entre as três instâncias. 


# TECNOLOGIAS

Para esta atividade, utilizei as seguintes tecnologias:

- AWS - Para criação das máqunas/instâncias  
- NGINX  - Como servidor
- Whimsical - Para criação de arquitetura dos serviços
- HTML - Criação de página simples para visualização do Load Balance em ação. 



