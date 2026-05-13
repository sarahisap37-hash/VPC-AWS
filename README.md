Laboratório 2: Criar a VPC e executar um servidor web
Visão geral e objetivos do laboratório
Neste laboratório, você usará a Amazon Virtual Private Cloud (VPC) para criar sua própria VPC e adicionar outros componentes para produzir uma rede personalizada. Você também criará um grupo de segurança. Em seguida, você configurará e personalizará uma instância do EC2 para executar um servidor web e iniciará a instância do EC2 para execução em uma sub-rede na VPC.

A Amazon Virtual Private Cloud (Amazon VPC) permite iniciar recursos da Amazon Web Services (AWS) em uma rede virtual definida por você. Essa rede virtual se assemelha a uma rede tradicional operada no seu data center, com os benefícios de usar a infraestrutura escalável da AWS. Você pode criar uma VPC que abranja várias Zonas de Disponibilidade.

Depois de concluir o laboratório, você será capaz de:

Criar uma VPC.

Criar sub-redes.

Configurar um grupo de segurança.

Executar uma instância do EC2 em uma VPC.

 

Duração
O laboratório levará aproximadamente 30 minutos para ser concluído.

 

Restrições de serviço da AWS
Neste ambiente de laboratório, o acesso aos serviços e às ações de serviços da AWS pode ser restrito aos necessários para concluir as instruções do laboratório. Você poderá encontrar erros se tentar acessar outros serviços ou executar ações além das descritas neste laboratório.

 

Cenário
Neste laboratório, você criará a seguinte infraestrutura:

Arquitetura

Acessar o Console de Gerenciamento da AWS
No topo destas instruções, escolha  Iniciar laboratório.

A sessão de laboratório será iniciada.

Um cronômetro ficará visível no topo da página e mostrará o tempo restante da sessão.

 Dica: para atualizar a duração da sessão a qualquer momento, selecione  Iniciar laboratório novamente antes que o cronômetro seja zerado.

Antes de continuar, aguarde até o ícone circular à direita do link da AWS  no canto superior esquerdo ficar verde. 

 

Para se conectar ao Console de Gerenciamento da AWS, escolha o link da AWS no canto superior esquerdo.

Uma nova guia do navegador será aberta, e você acessará o console.

 Dica: se uma nova guia não for aberta, você verá um banner ou um ícone no topo do navegador com uma mensagem informando que o programa está impedindo que o site abra janelas pop-up. Selecione o banner ou ícone e escolha Permitir pop-ups.

 

Organize a guia do Console de Gerenciamento da AWS para que ela seja exibida com essas instruções. O ideal seria você poder visualizar as duas guias do navegador ao mesmo tempo para facilitar o acompanhamento das etapas do laboratório.

 

Como obter crédito para seu trabalho
Ao final deste laboratório, você receberá instruções de como enviar o laboratório para receber uma pontuação com base em seu progresso.

 Dica: o script que verifica seu trabalho só pode conceder pontos se você nomear recursos e definir as configurações conforme especificado. Em particular, os valores nessas instruções que aparecem This Format devem ser inseridos exatamente como documentado (diferenciando letras maiúsculas de minúsculas).

 

Tarefa 1: Criar a VPC
Nesta tarefa, você usará a opção VPC e muito mais no console da VPC para criar vários recursos, incluindo uma VPC, um gateway de internet, uma sub-rede pública e uma sub-rede privada em uma única Zona de Disponibilidade, duas tabelas de rotas e um gateway NAT.

 

Na caixa de pesquisa à direita de  Serviços, procure e selecione VPC para abrir o console da VPC.

   

Comece a criar uma VPC.

No canto superior direito da tela, confirme se a região é Norte da Virgínia (us-east-1). 

Selecione o link Painel da VPC na parte superior esquerda do console.

Depois, escolha Criar VPC. 

Observação: se não houver um botão com esse nome, escolha o botão “Iniciar assistente da VPC”.

  

Configure os detalhes da VPC no painel Configurações da VPC à esquerda:

Escolha VPC e muito mais.

Em Geração automática da etiqueta de nome, mantenha a opção Gerar automaticamente selecionada, mas altere o valor do projeto para lab.

Mantenha IPv4 CIDR block (Bloco CIDR IPv4) definido como 10.0.0.0/16.

Para Número de zonas de disponibilidade, selecione 1.

Para Número de sub-redes públicas, mantenha 1.

Para Número de sub-redes privadas, mantenha 1.

Expanda a seção Personalizar blocos CIDR de sub-redes.

Altere Public subnet CIDR block in us-east-1a (Bloco CIDR de sub-rede pública em us-east-1a) para 10.0.0.0/24.
Altere Private subnet CIDR block in us-east-1a (Bloco CIDR de sub-rede privada em us-east-1a) para 10.0.1.0/24.
Defina Gateways NAT como In 1 AZ (em 1 AZ).

Defina Endpoints da VPC como Nenhum.

Mantenha as opções Nomes de host DNS e Resolução de DNS ativadas.

 

No painel Visualização à direita, confirme as configurações definidas.

VPC: lab-vpc

Sub-redes:

us-east-1a

Nome da sub-rede pública: lab-subnet-public1-us-east-1a
Nome da sub-rede privada: lab-subnet-private1-us-east-1a
Tabelas de rotas

lab-rtb-public
lab-rtb-private1-us-east-1a
Conexões de rede

lab-igw
lab-nat-public1-us-east-1a 
 

Na parte inferior da tela, escolha Criar VPC.

Os recursos da VPC foram como os recursos são criados. O gateway NAT levará alguns minutos para ser ativado. 

Aguarde até que todos os recursos tenham sido criados antes de passar para a próxima etapa.

 

Assim que tudo estiver concluído, escolha Visualizar VPC.

O assistente provisionou uma VPC com uma sub-rede pública e uma sub-rede privada em uma Zona de Disponibilidade, juntamente com tabelas de rotas para cada sub-rede. Ele também criou um gateway de internet e um gateway NAT. 

Para visualizar as configurações desses recursos, navegue pelos links do console da VPC que exibem os detalhes dos recursos. Por exemplo, selecione Sub-redes para visualizar os detalhes da sub-rede e Tabelas de rotas para visualizar os detalhes da tabela de rota. O diagrama abaixo resume os recursos da VPC recém-criados e como estão configurados.

Tarefa 1

 

Um gateway de internet é um recurso da VPC que permite a comunicação entre instâncias do EC2 na VPC e a internet. 

A sub-rede pública lab-subnet-public1-us-east-1a tem o CIDR de 10.0.0.0/24, o que significa que contém todos os endereços IP que começam com 10.0.0.x. A tabela de rota associada a essa sub-rede pública roteia o tráfego de rede 0.0.0.0/0 para o gateway de internet, o que a torna uma sub-rede pública.

Um gateway NAT é um recurso da VPC usado para fornecer conectividade com a internet para instâncias do EC2 executadas em sub-redes privadas na VPC, sem a necessidade de uma conexão direta com o gateway de internet.

A sub-rede privada lab-subnet-private1-us-east-1a tem um CIDR de 10.0.1.0/24, o que significa que contém todos os endereços IP que começam com 10.0.1.x.

 

Tarefa 2: Criar sub-redes adicionais
Nesta tarefa, você criará duas sub-redes adicionais para a VPC em uma segunda Zona de Disponibilidade. Ter sub-redes em várias Zonas de Disponibilidade em uma VPC é útil para implantar soluções que fornecem alta disponibilidade. 

Após criar uma VPC, como feito anteriormente, você pode configurá-la ainda mais, por exemplo, adicionando mais sub-redes. Cada sub-rede criada reside inteiramente em uma Zona de Disponibilidade. 

 

No painel de navegação à esquerda, escolha Sub-redes.

Primeiro, você criará uma segunda sub-rede pública.

 

Selecione Criar sub-rede e configure:

ID da VPC:  lab-vpc (selecione no menu).
Nome da sub-rede: lab-subnet-public2
Zona de Disponibilidade:: selecione a segunda Zona de Disponibilidade (por exemplo, us-east-1b)
IPv4 CIDR block (Bloco CIDR IPv4): 10.0.2.0/24
A sub-rede terá todos os endereços IP que começam com 10.0.2.x.

 

Escolha Criar sub-rede

A segunda sub-rede pública foi criada. Agora, você criará uma segunda sub-rede privada.

 

Selecione Criar sub-rede e configure:

ID da VPC: lab-vpc
Nome da sub-rede: lab-subnet-private2
Zona de Disponibilidade:: selecione a segunda Zona de Disponibilidade (por exemplo, us-east-1b)
IPv4 CIDR block (Bloco CIDR IPv4): 10.0.3.0/24
A sub-rede terá todos os endereços IP que começam com 10.0.3.x.

 

Escolha Criar sub-rede

A segunda sub-rede privada foi criada. 

Agora, você configurará a nova sub-rede privada para rotear o tráfego destinado à internet para o gateway NAT de modo que os recursos na segunda sub-rede privada possam se conectar à internet e se manter privados ao mesmo tempo. Para fazer isso, configure uma tabela de rota.

Uma tabela de rota contém um conjunto de regras, denominadas rotas, que são usadas para determinar para onde o tráfego de rede é direcionado. Toda sub-rede em uma VPC deve ser associada a uma tabela de rota; a tabela de rota controla o roteamento para a sub-rede.

 

No painel de navegação à esquerda, selecione Tabelas de rotas.

 

Selecione  a tabela de rota lab-rtb-private1-us-east-1a.

 

No painel inferior, escolha a guia Rotas.

Observe que Destination 0.0.0.0/0 (Destino 0.0.0.0/0) está definido como Target nat-xxxxxxxx (Meta nat-xxxxxxxx). Isso significa que o tráfego destinado à internet (0.0.0.0/0) será enviado ao gateway NAT. Em seguida, o gateway NAT encaminhará o tráfego para a internet.

Essa tabela de rota está sendo usada para rotear o tráfego de sub-redes privadas. 

 

Selecione a guia Associações de sub-rede.

Você criou essa tabela de rota na tarefa 1 quando optou por criar uma VPC e vários recursos nela. Essa ação também criou a sub-rede lab-subnet-private-1 e a associou à tabela de rota. 

Agora que criou outra sub-rede privada, lab-subnet-private-2, você também associará essa tabela de rota a essa sub-rede.

 

No painel Associações explícitas de sub-rede, escolha Editar associações de sub-rede.

 

Mantenha lab-subnet-private1-us-east-1a selecionada, mas também selecione  lab-subnet-private2.

 

Escolha Salvar associações.

Agora, você configurará a tabela de rota usada pelas sub-redes públicas.

 

Selecione a tabela de rota  lab-rtb-public e desmarque as demais sub-redes.

 

No painel inferior, escolha a guia Rotas.

Observe que Destination 0.0.0.0/0 está definido como Target igw-xxxxxxxx, que é um gateway de internet. Isso significa que o tráfego destinado à internet será enviado diretamente para a internet por meio desse gateway de internet.

Agora, você associará essa tabela de rota à segunda sub-rede pública criada.

 

Selecione a guia Associações de sub-rede.

 

Na área Associações explícitas de sub-rede, escolha Editar associações de sub-rede.

 

Mantenha lab-subnet-public1-us-east-1a selecionada, mas também selecione  lab-subnet-public2.

 

Escolha Salvar associações.

A VPC agora tem sub-redes públicas e privadas configuradas em duas Zonas de Disponibilidade. As tabelas de rotas criadas na tarefa 1 também foram atualizadas para rotear o tráfego de rede para as duas novas sub-redes.

Tarefa 2

 

Tarefa 3: Criar um grupo de segurança da VPC
Nesta tarefa, você criará um grupo de segurança da VPC, que atua como um firewall virtual. Ao executar uma instância, você pode associar um ou mais grupos de segurança a ela. Você pode adicionar regras a cada grupo de segurança que permitam tráfego de entrada ou de saída nas instâncias associadas.

 

No painel de navegação à esquerda, escolha Grupos de segurança.

 

Selecione Criar grupo de segurança e configure:

Nome do grupo de segurança: Web Security Group

Descrição: Enable HTTP access

VPC: selecione o X para remover a VPC selecionada e escolha lab-vpc na lista suspensa.

 

No painel Regras de entrada, selecione Adicionar regra.

 

Defina as configurações a seguir:

Tipo: HTTP

Origem: Anywhere-IPv4

Descrição: Permit web requests

 

Role até a parte inferior da página e selecione Criar grupo de segurança.

Você usará esse grupo de segurança na próxima tarefa ao executar uma instância Amazon EC2.

 

Tarefa 4: Executar uma instância de servidor Web
Nesta tarefa, você executará uma instância do Amazon EC2 na nova VPC. Você configurará a instância para atuar como um servidor web.

 

Na caixa de pesquisa à direita de  Serviços, procure e selecione EC2 para abrir o console do EC2.

 

No menu Executar instância, selecione Executar instância.

 

Nomeie a instância:

Dê o nome Web Server 1

Quando a instância é nomeada, a AWS cria uma tag e a associa à instância. Uma tag é um par de chave-valor. A chave desse par é *Name*, e o valor é o nome que você inseriu para a instância do EC2.

 

Selecione uma AMI da qual criar a instância:

Na lista de AMIs Quick Start disponíveis, mantenha a configuração padrão Amazon Linux selecionada. 

Também mantenha a configuração padrão Amazon Linux 2023 AMI.

O tipo de imagem de máquina da Amazon (AMI) escolhido determina o sistema operacional que será executado na instância do EC2 que você iniciar.

 

Escolha um tipo de instância:

No painel Tipo de instância, mantenha o padrão t2.micro selecionado.

O Tipo de instância define os recursos de hardware atribuídos à instância.

 

Selecione o par de chaves para associar à instância:

No menu Nome do par de chaves, selecione vockey.

O par de chaves vockey selecionado permitirá que você se conecte a essa instância via SSH após a inicialização. Embora você não precise fazer isso neste laboratório, ainda é necessário identificar um par de chaves existente, criar um ou optar por prosseguir sem um par de chaves ao iniciar uma instância.

 

Defina as configurações de rede:

Ao lado de Configurações de rede, selecione Editar e configure: 

Rede: lab-vpc 
Sub-rede: lab-subnet-public2 (não privada.)
Atribuir IP público automaticamente: Ativar
Depois, você configurará a instância para usar o grupo de segurança da web criado anteriormente.

Em “Firewall (grupos de segurança)”, escolha  Selecionar grupo de segurança existente.

Em Grupos de segurança comuns, selecione  Web Security Group (Grupo de segurança da web).

O grupo de segurança permitirá acesso HTTP à instância.

 

Na seção Configurar armazenamento, mantenha as configurações padrão.

Observação: as configurações padrão especificam que o volume raiz da instância, que hospedará o sistema operacional convidado do Amazon Linux especificado anteriormente, será executado em um disco rígido SSD (gp3) de uso geral com tamanho de 8 GiB. Apesar de ser possível adicionar mais volumes de armazenamento como alternativa, isso não será necessário neste laboratório.

 

Configure um script para ser executado na instância quando ela for iniciada: 

Expanda o painel Detalhes avançados.

Role até a parte inferior da página e copie e cole o código mostrado abaixo na caixa Dados do usuário:

#!/bin/bash
# Install Apache Web Server and PHP
dnf install -y httpd wget php mariadb105-server
# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip
unzip lab-app.zip -d /var/www/html/
# Turn on web server
chkconfig httpd on
service httpd start
Esse script será executado com permissões de usuário-raiz no sistema operacional convidado da instância. Ele será executado automaticamente quando a instância for iniciada pela primeira vez. O script instala um servidor web, um banco de dados e bibliotecas PHP e, em seguida, baixa e instala um aplicativo web PHP no servidor web.

 

Na parte inferior do painel Resumo à direita da tela, selecione Executar instância.

Uma mensagem de sucesso será exibida.

 

Selecione Visualizar todas as instâncias.

 

Aguarde até que o Web Server 1 indique 2/2 verificações aprovadas na coluna Verificação de status.

 Isso pode levar alguns minutos. Selecione o ícone de atualização  no topo da página a cada 30 segundos ou mais para conferir rapidamente o status mais recente da instância.

Agora, você se conectará ao servidor web em execução na instância do EC2.

 

Selecione  Web Server 1.

 

Copie o valor de Public IPv4 DNS (DNS IPv4 público) mostrado na guia Detalhes na parte inferior da página.

 

Abra uma nova guia do navegador da web, cole o valor de DNS público e pressione Enter.

Você deve ver uma página da web com o logotipo da AWS e os valores de metadados da instância.

