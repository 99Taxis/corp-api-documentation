## 99 CORP - Documentação API

Documentação para uso dos endpoints da API para clientes corporativos da 99.

Endereço de acesso: [https://api.corp.99taxis.com/v1](https://api.corp.99taxis.com/v1)

## Swagger

https://api.corp.99taxis.com/docs

## Seções

- [Colaboradores](#colaboradores)

- [Centros de Custo](#centros-de-custo)

- [Projetos](#projetos)

- [Corridas em andamento](#corridas-em-andamento)

- [Corridas finalizadas](#corridas-finalizadas)

- [Webhooks](#webhooks)

- [FAQ](#faq)

## Colaboradores

### Busca de colaboradores

* **URL**

  `/employee`

* **Method**

  `GET`
  
*  **Parâmetros via query**


    | Atributo   | Tipo do dado   | Descrição                                  | Obrigatório | Valor padrão | Exemplo     |
    |------------|----------------|------------------------------------------- |-------------|--------------|-------------|
    | search     | alfanumérico   | Busca pelo nome do centro de custo         | não         | -            | financeiro  |
    | offset     | numérico       | Índice da página no sistema de paginação   | não         | 1            | 0           |
    | limit      | numérico       | Quantidade de registros por página         | não         | 50           | 20          |
    | page       | numérico       | Depreciado                                 | não         | -            | -           |
    | nationalId | alfanumérico   | Documento do colaborador (somente números) | não         | -            | 98765432100 |

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    [
      {
        "name": "José Santos",
        "phone": {
          "number": "11999999999",
          "country": "BRA"
        },
        "email": "jose.santos@empresa.com.br",
        "companyId": 100,
        "nationalId": "98765432100",
        "pin": "053",
        "enabled": true,
        "externalId": 0,
        "categories": [
          "regular-taxi", "turbo-taxi", "pop99"
        ],
        "id": 125
      }
    ]
    ``` 
    
-----

### Busca de colaborador por identificador

* **URL**

  `/employee/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do colaborador                 | sim             | -                | 125             |
   

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
        "name": "José Santos",
        "phone": {
          "number": "11999999999",
          "country": "BRA"
        },
        "email": "jose.santos@empresa.com.br",
        "companyId": 100,
        "nationalId": "98765432100",
        "pin": "053",
        "enabled": true,
        "externalId": 0,
        "categories": [
          "regular-taxi", "turbo-taxi", "pop99"
        ],
        "id": 125
    }
    ```
    
-----

## Cadastro de colaborador

* **URL**

  `/employee`

* **Method**

  `POST`
  
*  **Parâmetros via body**


    | Atributo                   | Tipo do dado     | Descrição                                 | Obrigatório | Valor padrão | Exemplo     |
    |------------                |----------------  |-------------------------------------------|-------------|--------------|-------------|
    | employee.name              | alfanumérico              | Nome do colaborador                      | sim         | -            | José Santos  |
    | employee.phone.number      | alfanumérico              | Número do telefone do colaborador (somente números)                | sim         | -           |  11999999999          |
    | employee.phone.country     | alfanumérico              | Código do país atrelado ao número                      | não         | BRA           | BRA          |
    | employee.email             | alfanumérico              | E-mail do colaborador               | sim         | -            | jose.santos@empresa.com.br |
    | employee.nationalId        | alfanumérico              | Documento do colaborador (CPF) (Somente números) | não&ast;         | -            | 98765432100 |
    | employee.pin               | alfanumérico              | Código de confirmação de corrida (deve conter 3 dígitos)| não&ast;         | -            | 934 |
    | employee.externalId        | numérico                  | Identificador externo do colaborador. É possível relacionar o identificador de um sistema externo. | não         | -            | 456 |
    | employee.categories        | conjunto de alfanuméricos | Categorias permitidas para uso do colaborador. Valores aceitos: regular-taxi, turbo-taxi, top99, pop99 | sim         | -            | regular-taxi, turbo-taxi |
    | sendWelcomeEmail           | verdadeiro/falso          | Se verdadeiro, colaborador cadastrado receberá um e-mail de boas vindas| não         | false            | false |

    
*  **Regras**

    Ao cadastrar um colaborador é necessário informar o nationalId ou o pin. Ambos os atributos podem ser cadastrados, mas ao menos um deles é obrigatório.
    
    Quando o colaborador realizar uma corrida da categoria "regular-taxi", ao término da mesma será necessário entrar com um código de confirmação do pagamento. Esse código será os 3 primeiros dígitos do CPF ou o código pin cadastrado (também de 3 dígitos).

*   **Exemplo de envio**

    ```json
    {
        "employee": {
            "name": "José Santos",
            "phone": {
                "number": "11999999999",
                "country": "BRA"
            },
            "email": "jose.santos@empresa.com.br",
            "nationalId": "98765432100",
            "pin": "053",
            "externalId": 55091,
            "categories": [
                "regular-taxi", "turbo-taxi", "pop99"
            ],
            "id": 125
        },
        "sendWelcomeEmail": false
    }
    ```


* **Retorno**
  
  **Status Code:** 201
  
    ```json
    {
        "name": "José Santos",
        "phone": {
          "number": "11999999999",
          "country": "BRA"
        },
        "email": "jose.santos@empresa.com.br",
        "nationalId": "98765432100",
        "pin": "053",
        "externalId": 55091,
        "categories": [
          "regular-taxi", "turbo-taxi", "pop99"
        ],
        "id": 125
    }
    ```
    
-----

## Atualizar os dados de colaborador

* **URL**

  `/employee/{id}`

* **Method**

  `PUT`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do colaborador                 | sim             | -                | 125             |
   
  
*  **Parâmetros via body**


    | Atributo                   | Tipo do dado     | Descrição                                 | Obrigatório | Valor padrão | Exemplo     |
    |------------                |----------------  |-------------------------------------------|-------------|--------------|-------------|
    | employee.name              | alfanumérico              | Nome do colaborador                      | sim         | -            | José Santos  |
    | employee.phone.number      | alfanumérico              | Número do telefone do colaborador (somente números)                | sim         | -           |  11999999999          |
    | employee.phone.country     | alfanumérico              | Código do país atrelado ao número                      | não         | BRA           | BRA          |
    | employee.email             | alfanumérico              | E-mail do colaborador               | sim         | -            | jose.santos@empresa.com.br |
    | employee.nationalId        | alfanumérico              | Documento do colaborador (CPF) (Somente números) | não         | -            | 98765432100 |
    | employee.pin               | alfanumérico              | Código de confirmação de corrida (deve conter 3 dígitos)| não         | -            | 934 |
    | employee.externalId        | numérico                  | Identificador externo do colaborador. É possível relacionar o identificador de um sistema externo. | não         | -            | 456 |
    | employee.categories        | conjunto de alfanuméricos | Categorias permitidas para uso do colaborador. Valores aceitos: regular-taxi, turbo-taxi, top99, pop99 | sim         | -            | regular-taxi, turbo-taxi |
    | sendWelcomeEmail           | verdadeiro/falso          | Se verdadeiro, colaborador cadastrado receberá um e-mail de boas vindas| não         | false            | false |

*   **Exemplo de envio**

    ```json
    {
        "employee": {
            "name": "José Santos",
            "phone": {
                "number": "11999999999",
                "country": "BRA"
            },
            "email": "jose.santos@empresa.com.br",
            "nationalId": "98765432100",
            "pin": "123",
            "externalId": 55666,
            "categories": [
                "pop99"
            ],
            "id": 125
        },
        "sendWelcomeEmail": false
    }
    ``` 


* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
        "name": "José Santos",
        "phone": {
          "number": "11999999999",
          "country": "BRA"
        },
        "email": "jose.santos@empresa.com.br",
        "nationalId": "98765432100",
        "pin": "123",
        "externalId": 55666,
        "categories": [
          "pop99"
        ],
        "id": 125
    }
    ```
    
-----

## Desativar um colaborador

* **URL**

  `/employee/{id}`

* **Method**

  `DELETE`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do colaborador               | sim             | -                | 20             |

* **Retorno**
  
  **Status Code:** 204
    
-----

## Centros de Custo

### Busca de centros de custo

* **URL**

  `/costcenter`

* **Method**

  `GET`
  
*  **Parâmetros via query**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | search       | alfanumérico     | Busca pelo nome do centro de custo           | não             | -                | financeiro     |
   | offset        | numérico         | Índice da página no sistema de paginação     | não             | 1                | 0              |
   | limit        | numérico         | Quantidade de registros por página           | não             | 50               | 20             |

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    [
      {
        "id": 1,
        "name": "financeiro"
      },
      {
        "id": 2,
        "name": "recursos humanos" 
      }
    ]
    ``` 
    
-----

### Busca de centro de custo por identificador

* **URL**

  `/costcenter/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do centro de custo           | sim             | -                | 20             |

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
      "id": 20,
      "name": "financeiro"
    }
    ```
    
-----

## Colaboradores e Centro de Custo

### Busca de centros de custo por colaborador

* **URL**

  `/employee/{id}/costcenter`

* **Method**

  `GET`
  
*  **Parâmetros via query**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do colaborador                 | sim             | -                | 125             |
   

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    [
      {
        "id": 1,
        "name": "financeiro"
      },
      {
        "id": 2,
        "name": "recursos humanos" 
      }
    ]
    ```
    
-----

### Atualizar lista de centros de custo por colaborador

* **URL**

  `/employee/{id}/costcenter`

* **Method**

  `PATCH`
  
*  **Parâmetros via url**
    
    
    | Atributo         | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
    |----------        |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
    | id               | numérico         | Identificador do colaborador               | sim             | -                | 10             |
  
*  **Parâmetros via body**


   | Atributo         | Tipo do dado             | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo         |
   |----------        |--------------            |------------------------------------------    |-------------    |--------------    |------------     |
   | costCenterIDs  | conjunto de numéricos | Identificadores de centros de custo          | sim             | -                | 100, 200, 300  |
   

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
      "costCenterIDs": [
        100, 200, 300
      ]
    }
    ```
    
-----

## Desassociar um centro de custo de um colaborador

* **URL**

  `/employee/{employeeID}/costcenter/{costcenterID}`

* **Method**

  `DELETE`
  
*  **Parâmetros via url**


   | Atributo         | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------        |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | employeeID       | numérico         | Identificador do colaborador               | sim             | -                | 10             |
   | costcenterID   | numérico         | Identificador do centro de custo           | sim             | -                | 20             |


* **Retorno**
  
  **Status Code:** 204
    
-----

## Categorias por colaborador e estimativa de valor por corrida

* **URL**

  `/employee/{id}/categories`

* **Method**

  `GET`
  
*  **Parâmetros via url**

 | Atributo         | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
 |----------        |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
 | id               | numérico         | Identificador do colaborador               | sim             | -                | 10             |
 | fromLat          | alfanumérico     | Latitude do ponto de origem               | sim             | -                | -23.5986665   |
 | fromLng          | alfanumérico     | Longitude do ponto de origem               | sim             | -                | -46.6907445    |
 | toLat            | alfanumérico     | Latitude do ponto de destino               | não             | -                | -23,6822      |
 | toLng            | alfanumérico  | Longitude do ponsto de destino               | não             | -                | -46,6896      |
 | estimatedTime    | numérico         | Tempo estimado do trajeto em minutos      | não             | -                | 15             |
 | estimatedDistance| numérico         | Distância estimada do trajeto em metros   | não             | -                | 10250         |
 
 * **Retorno**
   
   **Status Code:** 200
   
   **Estrutura de retorno**
   
   | Atributo                        | Descrição                                                                                 |
   |-----------------------------    |---------------------------------------------------------------------------------------    |
   | category.id                     | Identificador da categoria. Valores possíveis: regular-taxi, turbo-taxi, top99, pop99     |
   | category.name                   | Nome da categoria disponível                                                              |
   | category.description            | Descrição da categoria disponível                                                         |
   | category.eta                    | Tempo de estimativa de chegada do motorista                                               |
   | estimate.lowerFare              | Menor valor possível para o trajeto estimado                                              |
   | estimate.upperFare              | Maior valor possível para o trajeto estimado                                              |
   | estimate.currencyCode           | Código da moeda usada para retornar o valor da corrida                                    |
   | estimate.fareText               | Texto com valor precificado                                                               |
   | estimate.fareTextComplement     | Complemento do texto com valor precificado                                                |
   | optional.id                     | Identificador de opcional disponível para a corrida                                       |
   | optional.title                  | Nome do opcional disponível                                                               |
   | optional.description            | Descrição do opcional disponível                                                          |
   
   **Exemplo de retorno**
   
     ```json
     [
       {
         "category": {
           "id": "turbo-taxi",
           "name": "99TAXI",
           "description": "Agora: 30% OFF",
           "eta": 2
         },
         "estimate": {
           "lowerFare": 23,
           "upperFare": 32,
           "currencyCode": "R$",
           "fareText": "R$23",
           "fareTextComplement": "ou mais"
         },
         "optionals": [
             {
                 "id": "accessible-taxi",
                 "title": "Veículo adaptado para cadeirante",
                 "description": "Veículo acessível"
             },
             {
                 "id": "big-trunk",
                 "title": "Porta-malas grande",
                 "description": "Porta-malas grande"
             },
             {
                 "id": "accept-animals",
                 "title": "Permite animais",
                 "description": "Permite animais"
             },
             {
                 "id": "air-conditioner",
                 "title": "Ar-condicionado",
                 "description": "Ar-condicionado"
             }
         ]
       },
       {
            "category": {
                "id": "regular-taxi",
                "name": "Táxi Comum",
                "description": "Tarifa cheia",
                "eta": 4
            },
            "estimate": {
                "lowerFare": 34,
                "upperFare": 45,
                "currencyCode": "R$",
                "fareText": "R$34",
                "fareTextComplement": "ou mais"
            },
            "optionals": [
                {
                    "id": "accessible-taxi",
                    "title": "Veículo adaptado para cadeirante",
                    "description": "Veículo acessível"
                },
                {
                    "id": "big-trunk",
                    "title": "Porta-malas grande",
                    "description": "Porta-malas grande"
                },
                {
                    "id": "accept-animals",
                    "title": "Permite animais",
                    "description": "Permite animais"
                },
                {
                    "id": "air-conditioner",
                    "title": "Ar-condicionado",
                    "description": "Ar-condicionado"
                }
            ]
        }
     ]
     ```
     
 ----

## Cadastro de centro de custo

* **URL**

  `/costcenter`

* **Method**

  `POST`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo            |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------        |
   | name       | alfanumérico     | Nome do centro de custo                   | sim             | -                | recursos humanos  |
   
*  **Exemplo de envio**
   
   ```json
   {
     "name": "recursos humanos"
   }
   ```

* **Retorno**
  
  **Status Code:** 201
  
    ```json
    {
      "id": 44,
      "name": "recursos humanos"
    }
    ```
    
-----

## Desativar um centro de custo

* **URL**

  `/costcenter/{id}`

* **Method**

  `DELETE`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do centro de custo           | sim             | -                | 20             |

* **Retorno**
  
  **Status Code:** 204
    
-----

## Projetos

### Busca de projetos

* **URL**

  `/project`

* **Method**

  `GET`
  
*  **Parâmetros via query**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | search       | alfanumérico     | Busca pelo nome do centro de custo           | não             | -                | financeiro     |
   | page        | numérico         | Índice da página no sistema de paginação     | não             | 1                | 0              |
   | limit        | numérico         | Quantidade de registros por página           | não             | 50               | 20             |

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    [
      {
        "id": 1,
        "name": "evento brasil 2017"
      },
      {
        "id": 2,
        "name": "project financeiro" 
      }
    ]
    ``` 
    
-----

### Busca de projeto por identificador

* **URL**

  `/project/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do project                   | sim             | -                | 1             |

* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
      "id": 1,
      "name": "evento brasil 2017"
    }
    ```
    
-----

## Cadastro de projeto

* **URL**

  `/project`

* **Method**

  `POST`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo            |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------        |
   | name       | alfanumérico     | Nome do projecto                           | sim             | -                | expo 2015         |
   
*  **Exemplo de envio**
   
   ```json
   {
     "name": "recursos humanos"
   }
   ```

* **Retorno**
  
  **Status Code:** 201
  
    ```json
    {
      "id": 5,
      "name": "expo 2015"
    }
    ```
    
-----

## Atualizar os dados de um projeto

* **URL**

  `/project/{id}`

* **Method**

  `PUT`
  
*  **Parâmetros via url**


    | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
    |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
    | id           | numérico         | Identificador do project                   | sim             | -                | 1             |
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo            |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------        |
   | name       | alfanumérico     | Nome do projeto                           | sim             | -                | expo 2017         |
   
*  **Exemplo de envio**
   
   ```json
   {
     "name": "expo 2017"
   }
   ```

* **Retorno**
  
  **Status Code:** 201
  
    ```json
    {
      "id": 5,
      "name": "expo 2017"
    }
    ```
    
-----

## Desativar um projeto

* **URL**

  `/project/{id}`

* **Method**

  `DELETE`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador do projeto                   | sim             | -                | 20             |

* **Retorno**
  
  **Status Code:** 204
    
-----

## Corridas em andamento

### Busca de corridas

* **URL**

  `/ride`

* **Method**

  `GET`
  
* **Retorno**
  
  **Status Code:** 200
  
  **Estrutura de retorno**
  
  | Atributo                       | Descrição                                                  |
  |----------------------------    |--------------------------------------------------------    |
  | employeeID                     | Identificador do colaborador atrelado a corrida            |
  | from.latitude                  | Latitude do endereço de origem                             |
  | from.longitude                 | Longitude do endereço de origem                            |
  | from.street                    | Endereço de origem                                         |
  | number                         | Numeração do endereço de origem                            |
  | reference                      | Referência para o endereço de origem                       |
  | phoneNumber                    | Telefone do colaborador atrelado a corrida                 |
  | costCenterID                   | Identificador do centro de custo                           |
  | projectID                      | Identificador do projeto                                   |
  | to.latitude                    | Latitude do endereço de destino                            |
  | to.longitude                   | Longitude do endereço de destino                           |
  | to.street                      | Endereço de destino                                        |
  | to.number                      | Número do endereço de destino                              |
  | to.reference                   | Referência para o endereço de destino                      |
  | status                         | Estado da corrida. Valores possíveis: COULDNT_FIND_AVAILABLE_DRIVERS, CAR_ON_THE_WAY e CAR_ARRIVED |
  | running.rideID                 | Identificador da corrida em andamento                      |
  | running.jobID                  | Identificador da corrida finalizada                        |
  | running.driver.driverId        | Identificador do motorista                                 |
  | running.driver.fullName        | Nome completo do motorista                                 |
  | running.driver.phoneNumber     | Telefone do motorista                                      |
  | running.driver.carModel        | Modelo do carro usado pelo motorista                       |
  | running.driver.carPlate        | Placa do veículo usado na corrida                          |
  | running.driver.img             | Endereço com foto do motorista                             |
  | running.position.latitude      | Latitude do endereço onde o motorista está localizado      |
  | running.position.longitude     | Longitude do endereço onde o motorista está localizado     |
  | running.route.time          | Tempo restante para chegada do motorista para início da corrida (em segundos)                     |
  | running.route.distance      | Distância já percorrida na corrida (em metros)               |
  | running.smsStartedSent      | Notifica se o SMS com os dados do motorista foi enviado ao passageiro |
  | running.smsDriverCanceledSent   | Notifica se o SMS informando o cancelamento da corrida pelo motorista foi enviado |

  
  **Retornos possíveis para o estado da corrida**
  
  | Código                             | Descrição                                           |
  |--------------------------------    |-------------------------------------------------    |
  | WAITING_DRIVERS_ANSWERS            | Aguardando resposta dos motoristas                  | 
  | COULDNT_FIND_AVAILABLE_DRIVERS     | Nenhum motorista disponível                         |
  | DRIVERS_REJECTED                   | Nenhum motorista aceitou a corrida                  |
  | CAR_ON_THE_WAY                     | Motorista está a caminho do endereço solicitado     |
  | CAR_ARRIVED                        | Motorista chegou e a corrida está em andamento      |
  | CANCELED_BY_DRIVER                 | Corrida cancelada pelo motorista                    |
  | CANCELED_BY_PASSENGER              | Corrida cancelada pelo passageiro                   |
  | RIDE_ENDED                         | Corrida finalizada                                  |

  **Exemplo de retorno**
  
    ```json
    [{
      "employeeID": 884373,
      "from": {
        "latitude": -23.564758,
        "longitude": -46.651850,
        "street": "Av Paulista, 1000, São Paulo - SP, Brasil",
        "number": "0",
        "reference": ""
      },
      "phoneNumber": "11999999999",
      "costCenterID": 43431,
      "categoryID": "turbo-taxi",
      "to": {
        "latitude": -23.590760,
        "longitude": -46.682129,
        "street": "Av. Faria Lima, 3000, São Paulo - SP, Brasil",
        "number": "0",
        "reference": ""
      },
      "projectID": 394932,
      "status": "CAR_ARRIVED",
      "running": {
        "rideID": "12219921932",
        "jobID": "12219921932",
        "driver": {
          "driverId": 6988,
          "fullName": "Claudo Moreira Cruz",
          "phoneNumber": "11999999999",
          "carModel": "Toyota Etios Sedan",
          "carPlate": "EAN-0165",
          "img": "https://s3.amazonaws.com/99taxis-drivers/xxx.jpg",
          "position": {
            "latitude": -23.582894,
            "longitude": -46.683991
          }
        },
        "route": {
          "time": 0,
          "distance": 507
        },
        "smsStartedSent": true,
        "smsDriverCanceledSent": false
      }
    }]
    ```
    
-----

### Busca de corrida por identificador

* **URL**

  `/ride/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | alfanumérico     | Identificador da corrida                   | sim             | -                | 1             |

* **Retorno**
  
  **Status Code:** 200
  
  **Exemplo de retorno**
  
    ```json
    {
        "employeeID": 884373,
        "from": {
            "latitude": -23.564758,
            "longitude": -46.651850,
            "street": "Av Paulista, 1000, São Paulo - SP, Brasil",
            "number": "0",
            "reference": ""
        },
        "phoneNumber": "11999999999",
        "costCenterID": 43431,
        "categoryID": "turbo-taxi",
        "to": {
            "latitude": -23.590760,
            "longitude": -46.682129,
            "street": "Av. Faria Lima, 3000, São Paulo - SP, Brasil",
            "number": "0",
            "reference": ""
        },
        "projectID": 394932,
        "status": "CAR_ARRIVED",
        "running": {
            "rideID": "12219921932",
            "jobID": "12219921932",
            "driver": {
                "driverId": 6988,
                "fullName": "Claudo Moreira Cruz",
                "phoneNumber": "11999999999",
                "carModel": "Toyota Etios Sedan",
                "carPlate": "EAN-0165",
                "img": "https://s3.amazonaws.com/99taxis-drivers/xxx.jpg",
                "position": {
                    "latitude": -23.582894,
                    "longitude": -46.683991
                }
            },
            "route": {
                "time": 0,
                "distance": 507
            },
            "smsStartedSent": true,
            "smsDriverCanceledSent": false
        }
    }
    ```
    
-----

    
## Solicitar corrida

* **URL**

  `/ride`

* **Method**

  `POST`
  
*  **Parâmetros via body**


    | Atributo                   | Tipo do dado     | Descrição                                 | Obrigatório | Valor padrão | Exemplo     |
    |------------                |----------------  |-------------------------------------------|-------------|--------------|-------------|
    | employeeID              | numérico              | Identificador do colaborador                      | sim         | -            | 884373  |
    | from.latitude      | alfanumérico              | Latitude do endereço de origem                | sim         | -           |  -23.564758          |
    | from.longitude     | alfanumérico              | Longitude do endereço de origem                      | sim         | -           | -46.651850          |
    | from.street             | alfanumérico              | Endereço de origem               | sim         | -            | Av Paulista, 1000, São Paulo - SP, Brasil |
    | from.number               | alfanumérico              | Número do endereço de origem | sim         | -            | 1000 |
    | from.reference        | alfanumérico                  | Ponto de referência para endereço de origem | não         | -            | Próximo a estação de metrô |
    | to.latitude      | alfanumérico              | Latitude do endereço de destino                | sim         | -           |  -23.564758          |
    | to.longitude     | alfanumérico              | Longitude do endereço de destino                      | sim         | -           | -46.651850          |
    | to.street             | alfanumérico              | Endereço de destino               | sim         | -            | Av Paulista, 1000, São Paulo - SP, Brasil |
    | to.number               | alfanumérico              | Número do endereço de destino | sim         | -            | 1000 |
    | to.reference        | alfanumérico                  | Ponto de referência para destino de origem | não         | -            | Próximo a estação de metrô |
    | phoneNumber        | alfanumérico                  | Número de telefone do colaborador a ser exibido para o motorista | sim         | -            | 11999999999 |
    | costCenterID        | numérico                  | Identificador do centro de custo | sim         | -            | 43431 |
    | categoryID        | alfanumérico                  |  Categoria a ser usada na corrida. Valores aceitos: regular-taxi, turbo-taxi, top99, pop99  | sim         | -            | pop99 |
    | notes        | alfanumérico                  | Justificativa da corrida | não         | -            | reunião com cliente |
    | projectID    | numérico                  | Identificador do projeto | não         | -            | 394932 |
    | optionals    | conjunto de alfanuméricos                  | Opcionais da corrida. Valores aceitos: accessible-taxi, female-driver, big-trunk, air-conditioner, offers-wifi, accept-animals, bike-rack | não         | -            | - |
    

 *  **Exemplo de envio**

    ```json
    {
      "employeeID": 884373,
      "from": {
        "latitude": -23.564758,
        "longitude": -46.651850,
        "street": "Av Paulista, 1000, São Paulo - SP, Brasil",
        "number": "1000",
        "reference": ""
      },
      "phoneNumber": "11999999999",
      "costCenterID": 43431,
      "categoryID": "pop99",
      "to": {
        "latitude": -23.590760,
        "longitude": -46.682129,
        "street": "Av. Faria Lima, 3000, São Paulo - SP, Brasil",
        "number": "0",
        "reference": ""
      },
      "notes": "",
      "projectID": 394932,
      "optionals": [
        "air-conditioner",
        "female-driver"
      ]
    }
    ``` 


* **Retorno**
  
  **Status Code:** 200
  
    ```json
    {
        "rideID": "12219921932",
        "smsStartedSent": false,
        "smsDriverCanceledSent": false
    }
    ```
    
-----

## Cancelar uma corrida

* **URL**

  `/ride/{id}`

* **Method**

  `DELETE`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | alfanumérico     | Identificador da corrida                   | sim             | -                | 20             |

* **Retorno**
  
  **Status Code:** 204
    
-----

## Corridas finalizadas

### Busca de corridas finalizadas

* **URL**

  `/job`

* **Method**

  `GET`
  
*  **Parâmetros via query**


    | Atributo   | Tipo do dado   | Descrição                                  | Obrigatório | Valor padrão | Exemplo     |
    |------------   |-------------- |------------------------------------------- |-------------|--------------|-------------|
    | startDate     | data e hora   | Indica data de início para filtro de busca por corridas finalizadas         | sim         | -            | 2017-06-01T00:00:00  |
    | endDate       | data e hora   | Indica data de término para filtro de busca por corridas finalizadas   | sim         | -            | 2017-06-10T23:59:59           |
    | costCenterID      | numérico       | Indicador do centro de custo         | não         | -           | 20          |
    | projectID       | numérico       | Indicador do projeto                                 | não         | -            | 15          |
    | taxiCategory | alfanumérico   | Categoria da corrida. Valores possíveis: regular-taxi, turbo-taxi, top99 ou pop99 | não         | -            | pop99 |
    | offset     | numérico       | Índice da página no sistema de paginação   | não         | 1            | 0           |
    | limit      | numérico       | Quantidade de registros por página         | não         | 20           | 10          |
    | page       | numérico       | Depreciado         | não         | -           | -          |
  
    
*  **Regras**

    Este endpoint pode demorar até 10 minutos depois que uma corrida foi finalizada (status: RIDE_ENDED) para que retorne os dados de corridas.
    Durante este tempo, o endpoint irá retornar status 404

* **Retorno**
  
  **Status Code:** 200
  
  **Estrutura de retorno**
   
   | Atributo                       | Descrição                                                  |
   |----------------------------    |--------------------------------------------------------    |
   | ride.apiTaxiJobId              | Identificador da corrida finalizada            |
   | ride.company.id                | Identificador da empresa                            |
   | ride.company.name              | Nome da empresa                            |
   | ride.employee.id               | Identificador do colaborador                                         |
   | ride.employee.name             | Nome do colaborador                            |
   | ride.employee.phone            | Telefone do colaborador                       |
   | ride.employee.phoneCountry     | País do telefone do colaborador                 |
   | ride.employee.email            | Email do colaborador                           |
   | ride.employee.nationalId       | Documento do colaborador (somente números)                                  |
   | ride.employee.externalId       | Identificador externo do colaborador. É possível relacionar o identificador de um sistema externo. |
   | ride.costCenter.id             | Identificador do centro de custo                           |
   | ride.costCenter.name           | Nome do centro de custo                                        |
   | ride.project.id                | Identificador do projeto                              |
   | ride.project.name              | Nome do projeto                      |
   | ride.fare                      | Valor total da corrida          |
   | ride.note                      | Justificativa                                   |
   | ride.odometer                  | Distância total da corrida em metros                        |
   | ride.start.latitude            | Latitude do ponto de origem                                |
   | ride.start.longitude           | Longitude do ponto de origem                                 |
   | ride.start.date                | Data de início da corrida                                      |
   | ride.start.address             | Endereço de origem                       |
   | ride.end.latitude              | Latitude do ponto de destino                          |
   | ride.end.longitude             | Longitude do ponto de destino                             |
   | ride.end.date                  | Data de término da corrida      |
   | ride.end.address               | Endereço de destino     |
   | ride.driver.name               | Nome do motorista                    |
   | ride.driver.phone              | Telefone do motorista               |
   | ride.driver.car                | Veículo do motorista |
   | ride.driver.carYear            | Ano do veículo |
   | ride.driver.carPlate           | Placa do veículo |
   | ride.city                      | Cidade |
   | ride.callPlatform              | Depreciado |
   | ride.requesterName             | Nome da pessoa que solicitou a corrida |
   | ride.taxiCategoryName          | Categoria da corrida |
   | summary.quantity               | Quantidade de corridas  |
   | summary.totalFare              | Valor total das corridas retornadas |

   
  **Retornos possíveis para o estado da corrida**
    
    | Código                             | Descrição                                           |
    |--------------------------------    |-------------------------------------------------    |
    | RIDE_ENDED                         | Corrida finalizada                                  |
  
  **Exemplo de retorno**
  
    ```json
    {
      "rides": [{
        "apiTaxiJobId": "12219921932",
        "company": {
          "id": 9311,
          "name": "99 Tecnologia"
        },
        "employee": {
          "id": 434343,
          "name": "COLABORADOR",
          "phone": "11999999999",
          "phoneCountry": "BRA",
          "email": "colaborador@99taxis.com",
          "nationalId": "63313137709",
          "externalId": 329932
        },
        "costCenter": {
          "id": 8483843,
          "name": "99Integracao"
        },
        "project": {
          "id": 433902,
          "name": "99Projeto"
        },
        "fare": 25.4,
        "note": "justificativa",
        "odometer": 7593,
        "start": {
          "latitude": -23.590760,
          "longitude": -46.682129,
          "date": 1502921626000,
          "address": "Av. Faria Lima, 3000, São Paulo - SP, Brasil"
        },
        "end": {
          "latitude": -23.564758,
          "longitude": -46.651850,
          "date": 1502922838000,
          "address": "Av Paulista, 1000, São Paulo - SP, Brasil"
        },
        "driver": {
          "name": "Motorista",
          "phone": "11999999999",
          "car": "Fiat Siena",
          "carYear": 2015,
          "carPlate": "EJF-3931"
        },
        "city": "São Paulo",
        "callPlatform": "web",
        "requesterName": "COLABORADOR",
        "taxiCategoryName": "Taxi 30% OFF"
      }],
      "summary": {
        "quantity": 1,
        "totalFare": 25.4
      }
    }
    ```
    
-----

### Busca de corridas finalizadas por identificador

* **URL**

  `/job/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | numérico         | Identificador da corrida                   | sim             | -                | 1             |

* **Retorno**
  
  **Status Code:** 200
  
  **Exemplo de retorno**
  
    ```json
    {
      "apiTaxiJobId": "12219921932",
      "company": {
        "id": 9311,
        "name": "99 Tecnologia"
      },
      "employee": {
        "id": 434343,
        "name": "COLABORADOR",
        "phone": "11999999999",
        "phoneCountry": "BRA",
        "email": "colaborador@99taxis.com",
        "nationalId": "63313137709",
        "externalId": 329932
      },
      "costCenter": {
        "id": 8483843,
        "name": "99Integracao"
      },
      "project": {
        "id": 433902,
        "name": "99Projeto"
      },
      "fare": 25.4,
      "note": "justificativa",
      "odometer": 7593,
      "start": {
        "latitude": -23.590760,
        "longitude": -46.682129,
        "date": 1502921626000,
        "address": "Av. Faria Lima, 3000, São Paulo - SP, Brasil"
      },
      "end": {
        "latitude": -23.564758,
        "longitude": -46.651850,
        "date": 1502922838000,
        "address": "Av Paulista, 1000, São Paulo - SP, Brasil"
      },
      "driver": {
        "name": "Motorista",
        "phone": "11999999999",
        "car": "Fiat Siena",
        "carYear": 2015,
        "carPlate": "EJF-3931"
      },
      "city": "São Paulo",
      "callPlatform": "web",
      "requesterName": "COLABORADOR",
      "taxiCategoryName": "Taxi 30% OFF"
    }
    ```
        
-----

## Webhooks

Webhook permite que seu sistema receba notificações de eventos originados de corridas da 99.
Quando um desses eventos ocorrer, iremos enviar um HTTP POST com o payload do evento para a URL configurada no webhook. Webhooks podem ser utilizados para receber o status e a posição do motorista durante uma corrida em andamento.

### Segurança

Toda comunicação deve ser feita com HTTPS e a url de recebimento do evento deve estar configurada na porta 443.

Para garantir a integridade do evento, e que o mesmo está sendo enviado através dos servidores da 99, um header no padrão Basic Authentication será acrescentado em toda requisição. As credenciais podem ser configuradas através do endpoint de [Criação de Webhook](#criação-de-webhook).

### Estratégia de tentativas 
O webhook espera que sua aplicação responda com o http status code 200 (OK) com um corpo vazio para todo evento gerado, e iremos aguardar até 10 segundos para receber a resposta. Caso qualquer uma dessas regras não sejam atingidas, o evento será re-transmitido para sua aplicação de acordo com um algoritmo de compensação exponencial (exponential back-off) com multiplicador de 15 segundos, até um limite máximo de 10 tentativas. Isso significa que iremos tentar enviar o mesmo evento para o seu servidor durante um período de 2 horas.

### Criação do Webhook

* **URL**

  `/webhooks/`

* **Method**

  `PUT`
  
*  **Parâmetros via body**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | url           | String URL     | URL de recebimento dos eventos do seu servidor | sim             | -                | https://seudominio.com.br/ninenine/webhooks |
      | authentication.username           | String | Usuário do Basic Authentication | sim             | -                | https://seudominio.com.br/ninenine/webhooks |

* **Retorno**
  
  **Status Code:** 200

### Eventos

 O identificador do evento, event.id, é único por toda a plataforma, portanto sua aplicação deve ler essa informação e se adequar a duplicidades.
 


Os eventos enviados através do webhook seguem um padrão, para que sua aplicação entenda facilmente os eventos. Abaixo segue o payload da requisição através de um POST HTTP.



   | Atributo | Tipo do dado | Descrição | Exemplo |
   |----|---|----|---|---|---- |
   | event.id| String |Identificador único do evento| 123e4567-e89b-12d3-a456-426655440000
      | event.date| Long |Data da geração do evento| 123e4567-e89b-12d3-a456-426655440000






-----

## FAQ

### Aqui estão algumas dúvidas frequentes de nossos clientes

#### Estou recebendo o erro invalid.user.or.payment ao solicitar uma corrida. O que fazer?
> Este erro acontece quando há algum bloqueio ou restrição do colaborador para realizar esta corrida. Certifique-se que a corrida foi estimada através do endpoint `/employee/{id}/categories`, pois é obrigatório a estimativa antes de se fazer a corrida.

#### O valor final da corrida pode ser diferente da estimativa?
>  Sim, devido a condições de trânsito, tempo e rotas alternativas, o valor final da corrida pode ser diferente do valor estimado via API.

#### Como consigo um relatório de gastos do mês atual?
> Através do endpoint /job, faça a busca com um range de datas do mês atual. O objeto *summary* irá trazer a quantidade de corridas realizadas e a soma de todas as corridas.

#### Quando uma corrida é finalizada, meu colaborador recebe algum e-mail ou SMS?
> Sim, um e-mail de recibo de corrida é enviado para o colaborador, contendo as informações de rota, valores e os dados da empresa que fez a corrida. Alguns SMSs são enviados no decorrer da corrida, como por exemplo quando o motorista aceita a corrida. O telefone que será utilizado para enviar o SMS é do campo *phoneNumber* no endpoint /ride. Este telefone pode ser diferente do telefone cadastrado para o colaborador (/employee).

#### Existe alguma diferença entre o rideID e o jobID?
> Apesar da diferença de nomenclatura entre os atributos, o identificador representado em ambos os campos é o mesmo. Supondo uma ride que foi criada com rideID igual a "123", o jobID também terá seu valor como "123".

#### Em qual momento uma corrida pode ser cancelada?
> Uma corrida pode ser cancelada a qualquer momento desde que o passageiro não tenha embarcado no carro, representado pelo status CAR_ARRIVED. 

#### Qual a diferença entre os status CANCELED_BY_PASSENGER e CANCELED_BY_DRIVER?
> Ambos identificam que a corrida foi cancelada. Uma corrida pode ser cancelada tanto pelo motorista (CANCELED_BY_DRIVER), como pelo passageiro (CANCELED_BY_PASSENGER). Esses status são finais, portanto, caso o cliente ainda queira fazer a corrida, deve ser solicitada uma nova através do endpoint /ride
-----

## Suporte da API

Caso tenha mais dúvidas ou esteja com problemas na integração, mande um e-mail para *help-corp-api@99taxis.com*
