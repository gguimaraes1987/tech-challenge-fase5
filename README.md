## Setup e uso da aplicação

- Requisitos:
    - Docker
    - Gradle

- Clonar o projeto e rodar o script store.sh -> Esse script garante a ordem necessária de inicialização.
- Collection do [arquivo JSON da coleção](./store_collection.json).
    - Utilizar a collection para facilitar as requisições para a aplicação, a collection pode ser importada no Insomnia ou no Postman.

# Ordem de Inicialização do Projeto

Ao iniciar o ambiente de desenvolvimento local que utiliza contêineres Docker para cada serviço do projeto, é crucial seguir uma ordem específica de inicialização dos serviços para garantir que a infraestrutura esteja configurada corretamente. No contexto deste projeto, é necessário iniciar o projeto "Items" primeiro, pois ele é responsável por criar a rede de conexão Docker que será utilizada pelos demais serviços.

## Por que iniciar o projeto "Items" primeiro?

O projeto "Items" desempenha um papel fundamental na infraestrutura do sistema, pois é responsável por criar a rede de conexão Docker que será compartilhada pelos outros serviços, como o carrinho de compras ("Cart"), autenticação ("Auth"), e pagamento ("Payment"). Ao iniciar o projeto "Items" primeiro, garantimos que a rede Docker esteja pronta e configurada para aceitar conexões dos demais serviços.

## Procedimento Recomendado

Segue abaixo a ordem recomendada para iniciar os serviços do projeto:

1. **Items**: Inicie o serviço "Items" para criar a rede de conexão Docker.
2. **Cart**, **Auth**, **Payment**: Após o serviço "Items".

# Requisições com Header "userId" Obrigatório

Neste projeto, é obrigatório incluir o header "userId" em determinadas requisições, especificamente nas operações relacionadas à adição de itens no carrinho ("Cart") e no processo de pagamento ("Payment"). O header "userId" é essencial para identificar o usuário associado à ação realizada, garantindo segurança e consistência nas operações.

## Header "userId" nas Requisições

Ao realizar as seguintes operações, certifique-se de incluir o header "userId" com o ID do usuário correspondente:

### Adição de Item no Carrinho ("Cart")

Ao enviar uma requisição para adicionar um item ao carrinho, certifique-se de incluir o header "userId" com o ID do usuário.

# Autenticação de Serviços

## Visão Geral

Este documento descreve o comportamento do serviço de autenticação (Auth) em relação à criação automática de usuários para outros sistemas. Quando o serviço de autenticação é iniciado, verifica-se se os usuários necessários para os sistemas de itens (Items), carrinho (Cart) e pagamento (Payment) existem no banco de dados. Se algum desses usuários estiver ausente, o serviço de autenticação os criará automaticamente.

## Processo de Verificação e Criação Automática de Usuários

O serviço de autenticação executa o seguinte processo durante a inicialização:

1. Verificação da Existência de Usuários:
   - O serviço de autenticação verifica se os usuários para os sistemas de itens, carrinho e pagamento estão presentes no banco de dados.

2. Criação Automática de Usuários Ausentes:
   - Se algum dos usuários necessários estiver ausente, o serviço de autenticação os criará automaticamente com as credenciais padrão.

## Credenciais Padrão dos Usuários

Os usuários criados automaticamente terão as seguintes credenciais padrão:

- **Usuário de Itens (Items)**
  - Email: items@service.com
  - Senha: itemsService

- **Usuário de Carrinho (Cart)**
  - Email: cart@service.com
  - Senha: cartService

- **Usuário de Pagamento (Payment)**
  - Email: payment@service.com
  - Senha: paymentService

Os usuários ficam configurados na seguinte estrutura, dentro do application.yml:

    authentication-service:
      user:
        username: payment@service.com
        password: paymentService
## Nota

Este processo de criação automática de usuários ocorre apenas durante a inicialização do serviço de autenticação e é projetado para garantir a integridade e o funcionamento adequado dos sistemas inter-relacionados.
