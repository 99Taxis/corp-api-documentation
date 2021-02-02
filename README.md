
## 99 CORP - Documentação API

Documentação para uso dos endpoints da API para clientes corporativos da 99.

Endereço de acesso: [https://api-corp.99app.com/v1](https://api-corp.99app.com/v1)

## Swagger

É possível realizar testes online utilizando a interface do Swagger. Para isso, acesse o endereço abaixo:

https://api-corp.99app.com/docs

## Autenticação

Todas as requisições precisam informar o token de acesso para o processo de autenticação. Informe a chave com nome `x-api-key` juntamente com o valor do token de acesso no cabeçalho HTTP de cada requisição. 

Considerando o cenário de exemplo onde a chave de acesso possui o valor `key-abc-123`, segue chamada cURL para a listagem de colaboradores:

```
curl -X GET https://api-corp.99app.com/v1/employee -H 'x-api-key: key-abc-123'
```

## Seções

- [Empresas](#empresas)

- [Colaboradores](#colaboradores)

- [Centros de Custo](#centros-de-custo)

- [Projetos](#projetos)

- [Corridas](#corridas)

- [Recibos](#recibos)

- [Webhooks](#webhooks)

- [Testes em Sandbox](#testes-em-sandbox)

- [Gerenciar múltiplas empresas](#gerenciar-múltiplas-empresas)

- [FAQ](#faq)

## Empresas

### Busca de empresas autenticadas

* **URL**

  `/companies`

* **Method**

  `GET`
  
* **Retorno**
  
  **Status Code:** 200
  
    ```json
    [
      {
       "id": "1",
        "name": "Empresa principal"
      },
      {
        "id": "2",
        "name": "Empresa secundária"
      }
    ]
    ``` 
    
-----

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
        "companyId": 1073,
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        },
        "nationalId": "98765432100",
        "supervisorlId": 167,
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
  **Observação**
  O campo _companyId_ está **depreciado**. Deve-se usar os campos _company.id_ e _company.name_.

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
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        },
        "nationalId": "98765432100",
        "supervisorId": 167,
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
 | toLat            | alfanumérico     | Latitude do ponto de destino               | sim             | -                | -23,6822      |
 | toLng            | alfanumérico  | Longitude do ponsto de destino               | sim             | -                | -46,6896      |
 
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
    | employee.supervisorId      | numérico                  | Id do supervisor (employeeId)do colaborador. | não         | -            | 256 |
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
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        },
        "email": "jose.santos@empresa.com.br",
        "nationalId": "98765432100",
        "supervisorId": 256,
        "pin": "053",
        "externalId": 55091,
        "categories": [
          "regular-taxi", "turbo-taxi", "pop99"
        ],
        "id": 125
    }
    ```
    
-----

## Busca de centro de custo do colaborador

- **URL**

  `/employee/{employeeId}/costcenter`

- **Método**

  `GET`

- **Parâmetros via query**

  | Atributo | Tipo do dado | Descrição                    | Obrigatório | Valor padrão | Exemplo |
    | -------- | ------------ | ---------------------------- | ----------- | ------------ | ------- |
  | employeeId | numérico     | Identificador do colaborador | sim         | -            | 20      |

* **Retorno**

  **Status Code:** 200

  ```json
  [
    {
      "id": 77045,
      "name": "IntegrationAPI"
    }
  ]
  ```

---

## Atualizar centro de custos do colaborador

- **URL**

  `/employee/{employeeId}/costcenter`

- **Método**

  `PATCH`

- **Parâmetros via url**

  | Atributo   | Tipo do dado | Descrição                    | Obrigatório | Valor padrão | Exemplo |
    | ---------- | ------------ | ---------------------------- | ----------- | ------------ | ------- |
  | employeeId | numérico     | Identificador do colaborador | sim         | -            | 125     |

* **Parâmetros via body**

  | Atributo      | Tipo do dado          | Descrição                           | Obrigatório | Valor padrão | Exemplo  |
    | ------------- | --------------------- | ----------------------------------- | ----------- | ------------ | -------- |
  | costCenterIDs | conjunto de numéricos | Identificadores de centros de custo | sim         | -            | 100, 200 |

* **Exemplo de envio**

  ```json
  {
    "costCenterIDs": [100, 200]
  }
  ```

- **Retorno**

  **Status Code:** 200

  ```json
  [100, 200]
  ```

---

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
    | employee.supervisorId      | numérico                  | Id do supervisor (employeeId) do colaborador. | não         | -            | 256 |
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
            "supervisorId": 256,
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
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        },
        "email": "jose.santos@empresa.com.br",
        "nationalId": "98765432100",
        "supervisorId": 256,
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

## Remover supervisor de um colaborador

* **URL**

  `/employee/{id}/supervisor`

* **Method**

  `DELETE`
  
*  **Parâmetros via url**


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
        "name": "financeiro",
        "enabled": true,
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        }
      },
      {
        "id": 2,
        "name": "recursos humanos",
        "enabled": true,
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        } 
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
      "name": "financeiro",
      "enabled": true,
      "company": {
        "key": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
        "name": "99"
      }
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
        "enabled": true,
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        }
      },
      {
        "id": 2,
        "name": "recursos humanos",
        "enabled": true,
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        }
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
      "name": "recursos humanos",
      "enabled": true,
      "company": {
        "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
        "name": "99"
      }
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
        "name": "evento brasil 2017",
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        }
      },
      {
        "id": 2,
        "name": "project financeiro",
        "company": {
          "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
          "name": "99"
        }
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
      "name": "evento brasil 2017",
      "company": {
        "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
        "name": "99"
      }
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
      "name": "expo 2015",
      "company": {
        "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
        "name": "99"
      }
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
      "name": "expo 2017",
      "company": {
        "id": "47a3083b-5d03-4e05-ad9d-9fd6fddd613e",
        "name": "99"
      }
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

## Corridas

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
  | status                         | Estado da corrida. Os valores possíveis estão listados na tabela abaixo. |
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
  | WAITING_FOR_PASSENGER              | Motorista chegou e está aguardando passageiro       | 
  | CAR_ARRIVED                        | A corrida está em andamento                         |
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
    | from.reference        | alfanumérico                  | Ponto de referência para endereço de origem | não         | -            | Próximo a estação de metrô |
    | to.latitude      | alfanumérico              | Latitude do endereço de destino                | sim         | -           |  -23.564758          |
    | to.longitude     | alfanumérico              | Longitude do endereço de destino                      | sim         | -           | -46.651850          |
    | to.street             | alfanumérico              | Endereço de destino               | sim         | -            | Av Paulista, 1000, São Paulo - SP, Brasil |
    | to.reference        | alfanumérico                  | Ponto de referência para destino de origem | não         | -            | Próximo a estação de metrô |
    | phoneNumber        | alfanumérico                  | Número de telefone do colaborador a ser exibido para o motorista | sim         | -            | 11999999999 |
    | costCenterID        | numérico                  | Identificador do centro de custo | sim         | -            | 43431 |
    | categoryID        | alfanumérico                  |  Categoria a ser usada na corrida. Valores aceitos: regular-taxi, turbo-taxi, top99, pop99  | sim         | -            | pop99 |
    | notes        | alfanumérico                  | Justificativa da corrida | não         | -            | reunião com cliente |
    | projectID    | numérico                  | Identificador do projeto | não         | -            | 394932 |
    | optionals    | conjunto de alfanuméricos                  | Opcionais da corrida. Valores aceitos: accessible-taxi | não         | -            | - |
    

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

## Recibos

### Busca de recibos

* **URL**

  `/job`

* **Method**

  `GET`
  
*  **Parâmetros via query**


    | Atributo   | Tipo do dado   | Descrição                                  | Obrigatório | Valor padrão | Exemplo     |
    |------------   |-------------- |------------------------------------------- |-------------|--------------|-------------|
    | startDate     | data   | Indica data de início para filtro de busca por corridas finalizadas         | sim         | -            | 2017-06-01  |
    | endDate       | data   | Indica data de término para filtro de busca por corridas finalizadas   | sim         | -            | 2017-06-02           |
    | costCenterID      | numérico       | Indicador do centro de custo         | não         | -           | 20          |
    | projectID       | numérico       | Indicador do projeto                                 | não         | -            | 15          |
    | taxiCategory | alfanumérico   | Categoria da corrida. Valores possíveis: regular-taxi, turbo-taxi, top99 ou pop99 | não         | -            | pop99 |
    | offset     | numérico       | Índice da página no sistema de paginação   | não         | 1            | 0           |
    | limit      | numérico       | Quantidade de registros por página         | não         | 20           | 10          |
    | page       | numérico       | Depreciado         | não         | -           | -          |
  

* **Retorno**
  
  **Status Code:** 200
  
  **Estrutura de retorno**
   
   | Atributo                       | Descrição                                                  |
   |----------------------------    |--------------------------------------------------------    |
   | ride.apiTaxiJobId              | Identificador da corrida            |
   | ride.receiptId                 | Identificador do recibo             |
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
   | ride.duration                  | Duração da corrida em minutos 
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
        "receiptId": "8382932932",
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

### Busca do recibo pelo identificador da corrida

* **URL**

  `/job/{id}`

* **Method**

  `GET`
  
*  **Parâmetros via url**


   | Atributo     | Tipo do dado     | Descrição                                    | Obrigatório     | Valor padrão     | Exemplo        |
   |----------    |--------------    |------------------------------------------    |-------------    |--------------    |------------    |
   | id           | alfanumérico         | Identificador da corrida                   | sim             | -                | 1             |

*  **Regras**

    Após o término da corrida, existe o tempo de sincronização e geração do recibo. Devido a isso, pode demorar até 10 minutos para que o recibo da corrida esteja disponível. O identificador da corrida é o mesmo usado para obter os dados do recibo.

    Enquanto o recibo não estiver disponível, o endpoint irá retornar o status code 204.

* **Retorno**
  
  **Status Code:** 200

  Descrição: Recibo gerado e disponível.
  
  **Exemplo de retorno**
  
    ```json
    {
      "apiTaxiJobId": "12219921932",
      "receiptId": "8382932932",
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

    **Status Code:** 204

    Descrição: O recibo não existe ou não está disponível. Em caso de corrida finalizada, deve-se aguardar o tempo de sincronização e geração do recibo.
        
-----

## Webhooks

Webhook permite que seu sistema receba notificações de eventos originados de corridas da 99.
Quando um desses eventos ocorrer, iremos enviar um HTTP POST com o payload do evento para a URL configurada no webhook. Webhooks podem ser utilizados para receber o status e a posição do motorista durante uma corrida em andamento. É possível receber eventos de mudança de status de corrida, assim como posição atual do motorista durante uma corrida em andamento.

### Segurança

Toda comunicação deve ser feita com HTTPS e a url de recebimento do evento deve estar configurada na porta 443.

Para garantir a integridade do evento, e que o mesmo está sendo enviado através dos servidores da 99, um header no padrão Basic Authentication será acrescentado em toda requisição. As credenciais podem ser configuradas através do endpoint de Criação de Webhook abaixo. Seu servidor deve validar o header afim de garantir a segurança da informação.

### Estratégia de tentativas 
O webhook espera que sua aplicação responda com o http status code de sucesso 2xx (aceita-se 200, 201, 202, 203, 204, 205 ou 206) com um corpo vazio para todo evento gerado, e iremos aguardar até 10 segundos para receber a resposta. Caso qualquer uma dessas regras não sejam atingidas, o evento será re-transmitido para sua aplicação de acordo com um algoritmo de compensação exponencial (exponential back-off) com multiplicador de 15 segundos, até um limite máximo de 10 tentativas. Isso significa que iremos tentar enviar o mesmo evento para o seu servidor durante um período de 2 horas (por exemplo: 15, 30, 60, 120, 240 segundos e assim sucessivamente).

De acordo com as regras acima, um evento pode ser transmitido mais de uma única vez para os seus servidores, portanto você deve decidir em ignorar caso receba eventos repetidos. O atributo `event.id` garante a chave única do evento, e você pode utilizá-lo para validar eventuais duplicidades.

### Status de corridas

Os seguintes status de corrida serão notificados quando ocorrerem.
Os status marcados como **final** significam que não sofrerão alterações futuras.

| Status     | Descrição | Status final? |
|----------    |--------------    |---- |
| finding           | Buscando motorista | não |
| no-drivers-available | Nenhum motorista foi encontrado para sua solicitação| sim |
| canceled-by-passenger | Corrida cancelada pelo passageiro | sim |
| canceled-by-driver | Corrida cancelada pelo motorista | sim |
| on-the-way | Motorista encontrado e a caminho | não |
| arrived | Motorista chegou e está aguardando o passageiro | não |
| in-progress | Corrida iniciada com passageiro a bordo | não |
| finished | Corrida encerrada | sim |

### Obter configuração do Webhook

* **URL**

  `/webhooks/`

* **Method**

  `GET`

* **Retorno**
  
  **Status Code:** 200

  Descrição: Configuração do webhook cadastrada

  ```json
  {
    "url": "https://app.99taxis.com/v1/webhook",
    "authentication": {
        "username": "username",
        "password": "password123&&"
    },
    "subscriptions": [
        "ride-status",
        "ride-driver-location"
    ]
  }
  ```

### Criação do Webhook

* **URL**

  `/webhooks/`

* **Method**

  `PUT`
  
*  **Parâmetros via body**


   | Atributo                 | Tipo do dado     | Descrição                                            | Obrigatório     | Valor padrão     | Exemplo        |
   |----------                |--------------    |------------------------------------------            |-------------    |--------------    |------------    |
   | url                      | alfanumérico     | URL https de recebimento dos eventos do seu servidor | sim             | não possui       | https://seudominio.com.br/ninenine/webhooks |
   | authentication.username  | alfanumérico | Credencial do usuário de Basic Authentication            | sim             | não possui       | username |
   | authentication.password  | alfanumérico | Credencial de senha de Basic Authentication              | sim             | não possui       | password |
   | subscriptions            | conjunto de alfanuméricos | Lista com subscrições para recebimento de webhooks. Valores aceitos: ride-status, ride-driver-location | sim | não possui | ["ride-status", "ride-driver-location"] |

* **Regras**

- A senha informada para o sistema de segurança Basic Authentication deve conter as seguintes premissas:
  
  - Conter ao menos 8 caracteres
  - Conter ao menos 1 dígito numérico
  - Conter ao menos 1 caracter special

* **Exemplo de envio**

  ```json
  {
    "url": "https://app.99taxis.com/v1/webhook",
    "authentication": {
        "username": "username",
        "password": "password123&&"
    },
    "subscriptions": [
        "ride-status",
        "ride-driver-location"
    ]
  }
  ```

* **Retorno**
  
  **Status Code:** 200

  Descrição: Configuração de webhook atualizada.

  **Status Code:** 422

  Descrição: Ocorreram um ou mais erros de validação. Neste status, a seguinte estrutura json será retornada:

  ```json
  {
    "errors": [
        {
            "code": "required-url",
            "field": "url",
            "message": "Url must be informed"
        },
        {
            "code": "invalid-url",
            "field": "url",
            "message": "Url is not valid"
        },
        {
            "code": "required-authentication",
            "field": "authentication",
            "message": "Authentication must be informed"
        }
    ]
  }
  ```

  Segue mapeamento de erros completos que podem ser retornados caso algum dado inválido seja informado ao se definir o webhook.

  | Code                     | Field                   | Message                                                               | Descrição  |
  |----------                |-------                  |---------                                                              | ---------  |
  | required-url             | url                     | Url must be informed                                                  | Url não foi informada |
  | invalid-url              | url                     | Url is not valid                                                      | Url informada não é válida |
  | invalid-url              | url                     | Url must have secure https scheme                                     | Url precisa ser segura e usar https |
  | required-authenticationl | authentication          | Authentication must be informed                                       | Dados de autenticação não foram informados |
  | required-username        | authentication.username | Authentication username must be informed                              | Usuário para autenticação não foi informado |
  | required-password        | authentication.password | Authentication password must be informed                              | Senha para autenticação não foi informado |
  | invalid-password         | authentication.password | Authentication password should contain at least eight digits          | Senha de autenticação deve conter ao menos 8 caracteres |
  | invalid-password         | authentication.password | Authentication password should contain at least one digit             | Senha de autenticação deve conter ao menos 1 número |
  | invalid-password         | authentication.password | Authentication password should contain at least one special character | Senha de autenticação deve conter ao menos 1 caracter especial |
  | required-subscriptions   | subscriptions           | At least one subscription must be informed                            | Subscrição de eventos não foi definida |
  | invalid-subscriptions    | subscriptions           | Allowed subscriptions: ride-status, ride-driver-location              | Subscrição de evento selecionada não é válida |

### Desativação do uso do Webhook

* **URL**

  `/webhooks/`

* **Method**

  `DELETE`

* **Retorno**
  
  **Status Code:** 204

  Descrição: Desativado uso do webhook.

### Eventos

O identificador do evento `event.id` é único por toda a plataforma, portanto sua aplicação deve ler essa informação e se adequar a duplicidades. 

Os eventos enviados através do webhook seguem um padrão, para que sua aplicação entenda facilmente os eventos. Determinados dados não serem exibidos, dependendo do estado atual da corrida. Conforme a corrida mudar de status (iniciar, entrar em andamento ou finalizar), novos dados serão enviados, seguindo a estrutura única de entidade. Abaixo segue o payload da requisição através de um POST HTTP.

**Estrutura de retorno**
   
   | Atributo                         | Tipo do dado | Descrição                                                  |
   |----------------------------      | --------- | --------------------------------------------------------    |
   | event.id                         | identificador único universal (UUID) | Identificador único do evento            |
   | event.date                       | numérico | Data e hora do evento em formato unix timestamp  |
   | event.notification               | alfanumérico | Tipo da notificação. Valores possíveis: ride-status, ride-driver-location                             |
   | data.ride.id                     | alfanumérico | Identificador da corrida                                        |
   | data.ride.status                 | alfanumérico | Estado atual da corrida                            |
   | data.ride.category               | alfanumérico | Categoria da corrida. Valores possíveis: 99POP, 99TOP, 99TAXI ou REGULAR-TAXI                |
   | data.ride.corporate.employee.id  | numérico | Identificador do colaborador                                 | 
   | data.ride.corporate.note         | alfanumérico | Justificativa definida pelo colaborador ao solicitar a corrida                        |
   | data.ride.origin.latitude        | coordenada geográfica | Latitude do endereço de origem                              |
   | data.ride.origin.longitude       | coordenada geográfica | Longitude do endereço de origem            |
   | data.ride.destination.latitude   | coordenada geográfica | Latitude do endereço de destino                        |
   | data.ride.destination.longitude  | coordenada geográfica | Longitude do endereço de destino                                |
   | data.ride.driver.name            | alfanumérico | Nome do motorista                                 |
   | data.ride.driver.phone           | alfanumérico | Telefone do motorista                                     |
   | data.ride.vehicle.model          | alfanumérico | Modelo do veículo                          |
   | data.ride.vehicle.plate          | alfanumérico | Placa do veículo                             |
   | location.latitude                | coordenada geográfica | Latitude com última localização da corrida     |
   | location.longitude               | coordenada geográfica | Longitude com última localização da corrida     |

**Exemplo de notificação**

```json
{
  "event": {
    "id": "5c99bf07-a852-42ed-9bff-30ff13ed888a",
    "date": 1519918755701,
    "type": "ride-status"
  },
  "data": {
    "ride": {
      "id": "123456789",
      "status": "on-the-way",
      "category": "99POP",
      "corporate": {
        "employee": {
          "id": 123456
        },
        "note": "reunião com cliente externo"
      },
      "origin": {
        "latitude": -23.563157,
        "longitude": -46.654316
      },
      "destination": {
        "latitude": -23.599713,
        "longitude": -46.687777
      },
      "driver": {
        "name": "Claudio Moreira Cruz",
        "phone": "11223344559"
      },
      "vehicle": {
        "model": "Volkswagen GOL",
        "plate": "ABC-1234"
      },
      "location": {
        "latitude": -23.565678,
        "longitude": -46.680754
      }
    }
  }
}
```

**Headers**

Todos as notificações serão enviadas com as seguintes chaves no cabeçalho HTTP:

| Header                         | Descrição |
|----------------------------    | --------- |
| X-Environment                  | Indica se a notificação de evento foi enviada pela API no ambiente de **sandbox** ou **production** |


-----

## Testes em Sandbox

* **Endereço**

  `https://sandbox-api-corp.99app.com/v1`

O ambiente de sandbox tem como objetivo prover uma estrutura para realizar os testes de gerenciamento de recursos como colaboradores (employee), centros de custo (cost center) e projetos (project). Adicionalmente, é possível criar corridas e atualizar seu status sem a necessidade de um aplicativo aberto com um motorista  disponível. Por fim, o mecanismo de webhook, ou seja, notificações sobre mudança de status da corrida ou localização do motorista, também está disponível neste ambiente de testes, permitindo validar todos os passos da integração com a api da 99.

Todos os dados de corrida neste ambiente são simulados e com isso alguns valores podem não fazer sentido como relação entre distância e duração de uma corrida. Os dados de motorista, veículo, entre outros, sempre serão os mesmos. O objetivo é ter um conteúdo válido e para permitir validar a integração entre sistemas.

As corridas criadas neste ambiente de teste ficarão disponíveis por 72 horas (3 dias no total). Após esse período, os dados serão expirados, sendo necessário criar outra corrida.

* **Cenário de testes de corrida**

1) Criar uma corrida

Para criar uma corrida basta utilizar o mesmo endpoint de criação de corridas disponível no recurso `ride`.

`curl -X POST https://sandbox-api-corp.99app.com/v1/ride -H "Content-Type: application/json" -H "x-api-key: token" -d '{
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
}'`

2) Acompanhar status de uma corrida

Após criar a corrida, para obter os dados da mesma, basta utilizar o mesmo endpoint de acompanhamento de corridas disponível no recurso `ride`.

`curl -X GET https://sandbox-api-corp.99app.com/v1/ride/1234567890 -H "x-api-key: token"`

3) Atualizar o status de uma corrida

É possível atualizar o status da corrida, permitindo simular diversos cenários como cancelamento por parte do passageiro ou motorista, não localização de motoristas, corridas em andamento ou finalizadas. Para isso, é necessário realizar uma requisição com o método HTTP `PATCH`, fornecendo o identificador da corrida na url e o status desejado no corpo da mensagem.

Estrutura do endpoint:

* **URL**

  `/ride/{id}`

* **Method**

  `PATCH`
  
*  **Parâmetros via body**


   | Atributo                 | Tipo do dado     | Descrição                                            | Obrigatório     | Valor padrão     | Exemplo        |
   |----------                |--------------    |------------------------------------------            |-------------    |--------------    |------------    |
   | status                      | alfanumérico     | Status da corrida | sim 

*  **Valores de status válidos**

| Status                             | Descrição                                           |
|--------------------------------    |-------------------------------------------------    |
| WAITING_DRIVERS_ANSWERS            | Aguardando resposta dos motoristas                  | 
| COULDNT_FIND_AVAILABLE_DRIVERS     | Nenhum motorista disponível                         |
| DRIVERS_REJECTED                   | Nenhum motorista aceitou a corrida                  |
| CAR_ON_THE_WAY                     | Motorista está a caminho do endereço solicitado     |
| CAR_ARRIVED                        | Motorista chegou e a corrida está em andamento      |
| CANCELED_BY_DRIVER                 | Corrida cancelada pelo motorista                    |
| CANCELED_BY_PASSENGER              | Corrida cancelada pelo passageiro                   |
| RIDE_ENDED                         | Corrida finalizada                                  |

* **Retorno**
  
  **Status Code:** 204

  Descrição: Corrida teve seu status atualizado com sucesso.

  **Status Code:** 400

  Descrição: Estrutura de envio de requisição inválida.

  **Status Code:** 404

  Descrição: Corrida não localizada ou inválida.

  **Status Code:** 500

  Descrição: Algum erro inesperado ocorreu.

* **Exemplo de requisição**

`curl --request PATCH https://sandbox-api-corp.99app.com/v1/ride/1234567890 -H "Content-Type: application/json" -H "x-api-key: token" -d '{"status": "RIDE_ENDED"}'`

4) Receber notificações

Para receber notificações de mudança de status de corrida ou posição do motorista quando uma corrida estiver em andamento, é preciso definir as configurações de webhook através do recurso `webhooks`. Feito isso, sempre que o status da corrida for alterado usando o procedimento anterior (ou seja, uma requisição HTTP PATCH para o recurso `ride`), uma notificação será enviada para o endereço cadastrado com o status atual da corrida.

Caso não receba as notificações de webhook, verifique as configurações cadastradas de webhook como URL e meio de autenticação (nome usuário e senha).

## Gerenciar múltiplas empresas

É possível associar múltiplas empresas a um mesmo token de acesso. Esta funcionalidade permite gerenciar múltiplas empresas que podem ser filiais ou parceiras usando um único meio de autenticação. Para ativar esta funcionalidade, é preciso entrar em contato com nossa equipe através do e-mail *help-corp-api@99taxis.com*

### Listar empresas associadas ao token de acesso

* **URL**

  `/companies/`

* **Method**

  `GET`

* **Retorno**
  
  **Status Code:** 200

  Descrição: Lista de empresas associadas ao token de acesso

  ```json
  [
    {
        "id": "6d74008f-d2d5-4f79-95f5-4e049bdbac99",
        "name": "99 matriz"
    },
    {
        "id": "5169224d-30c5-4bdf-ba2c-019aa51c3829",
        "name": "99 filial"
    }
  ]
  ```

* **Exemplo de chamada**

Considera-se 123 como a chave de autenticação da empresa (token único de acesso)

```curl
curl -X GET "/v1/companies" -H "x-api-key: 123"
```

### Identificar empresa

O endpoint de listagem de empresas deve ser usado para obter o identificador de cada empresa. Este identificador será usado para escolher qual empresa de destino por operação, tendo como exemplo de uso o cadastro de colaborador/centro de custo/projeto, solicitação de corridas ou extração de relatórios de recibos.

É preciso fornecer o identificador da empresa como parâmetro de uma chave no cabeçalho HTTP com o nome *x-company-id*. Exemplo:

```
"x-company-id: 6d74008f-d2d5-4f79-95f5-4e049bdbac99"
```

Exemplos de uso considerando a chave de acesso 123 e o ambiente de sandbox:

- Listar centros de custo da empresa 99 Matriz

```curl
curl -X GET https://sandbox-api-corp.99app.com/v1/costcenter/ 'x-api-key: 123' -H 'x-company-id: 6d74008f-d2d5-4f79-95f5-4e049bdbac99'
```

- Listar centros de custo da empresa 99 Filial

```curl
curl -X GET https://sandbox-api-corp.99app.com/v1/costcenter/ 'x-api-key: 123' -H 'x-company-id: 5169224d-30c5-4bdf-ba2c-019aa51c3829'
```

**Se o identificador (chave x-company-id) não for fornecido, será considerado a empresa primária associada ao token de acesso (na listagem de empresas associadas, será considerado o primeiro registro).**

## FAQ

### Aqui estão algumas dúvidas frequentes de nossos clientes

#### Como realizar o cálculo de estimativa de corridas?
> A estimativa de corridas está atrelada ao recurso de colaboradores (`employee`). Veja mais detalhes na seção Categorias por colaborador e estimativa de valor por corrida.

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
