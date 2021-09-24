# Refatorando em Microsserviços
```
O aluno deverá desempenhar as seguintes atividades:
Você foi contratado como Arquiteto de Integração de Software pelo gerente de tecnologia (TI) da empresa Acme Producer (AP).
Ao chegar na empresa, você identifica que ela possui um setor de TI que tem um time de profissionais que está sustentando as aplicações legadas a um bom tempo.
Esse time tem bom conhecimento de programação, contudo não possui mecanismos de gestão de configuração e/ou testes automatizados. 
Os clientes da empresa AP têm reclamado constantemente sobre problemas na integração de suas aplicações com os serviços (B2B), relativos a erros em ambiente de produção, lentidão da aplicação, retorno de volume de dados muito grande pelos serviços para clientes antigos, dentre outros. O gerente de TI está fascinado com novas tecnologias e tem ouvido falar muito de microsserviços. Logo, solicita a você que faça uma análise detalhada e estabeleça um projeto para migrar a solução monolítica da empresa AP para uma solução baseado em um estilo arquitetural de microsserviços. 
Elabore uma lista de ações práticas que precisariam ser executadas para a migração da solução para o estilo arquitetural, considerando a avaliação das dimensões abaixo e justificando as ações para cada uma delas:
```
### Avaliação
#
> a - Avaliação dos profissionais do time de TI;
> 
Esta avaliação pode(deve) ser feita em conjunto com o Scrum master ou lider do projeto.

Deve ser levado em consideração a tecnologia que o time já atua, quais tecnologias eles conhecem e quais desejam aprender.
Podemos fazer um meeting rápido, para coletar as informações necessárias e ter insumos para tomar uma decisão para os passos a seguir.
Será alinhado o nível técnico e tecnologias do time com as tendências e padrões utilizados no mercado, afinal, não existe melhor tecnologia ou framework, e sim, o que se encaixa na necessidade atual.

### Processos e Metodologias
#
> b - Mudanças de processos e metodologias de desenvolvimento de serviços;
Com base nas informações coletadas anteriormente, conseguimos estabelecer melhor os processos e metodologias de desenvolvimento.

> c - Avaliação de aspectos de governança;

> d - Avaliação de ferramentas/tecnologias necessárias;

> e - Avaliação de ferramentas/tecnologias necessárias;

A documentação de serviços através de `contratos` utilizando `swagger` irá fazer parte das integrações;

Um catálogo de `serviços e domínios` será criado, contendo, a princípio (como itens obrigatórios):
- swagger do serviço;
- nome do componente;
- domínio;
- breve descrição do serviço;

o `catálogo` deverá estar linkado com a nova `esteira automatizada` de `continuous delivery/integration/deployment`.
As versionamento será feito utilizando o `git`, que é bastante difundido e utilizado por inúmeras empresas no mundo.
Ao fazer o `commit` do código, a esteira fará a seguinte análise:
- verificar se o commit possui mensagem;
- ao possuir mensagem, deve conter o `id` da página do catálogo, para este componente;
- o commit deve ser recusado, se não atender aos requisitos acima;

Caso todos os requisitos sejam atendidos, a esteira utilizando `Jenkins` irá fazer o build automático do código, aplicar uma tag com a versão, gerar um pacote, analisar a qualidade de código integrado ao `Sonarqube` e publicar o pacote em uma plataforma de nuvem, utilizando o `Spinnaker` ou `Openshift`.

### Frameworks e Ferramentas
#
> f - Implementações (frameworks) necessárias para o projeto;
- CI/CD/Deployment com `Jenkins`;
- Análise de código e qualidade com `Sonarqube`;
- Versionamento usando `Git`;
- Governança através do `Confluence`;
- Contratos de serviços com `Swagger` ou `OpenAPI`;
- Publicação em cloud com `Spinnaker`, `Openshift` ou `Urbancode`;
- Utilização mandatória de `API Gateway` para exposição externa;

### Desenvolvimento
#
> g - Implementação de princípios de design de serviços;

> h - Particionamento do modelo de domínio;

> i -  Particionamento do banco de dados para a solução;

##### Princípios de design

Deve ser utilizada as mais boas práticas atuais de desenvolvimento, através de `clean code` com padrões como `Domain Driven Development` (afinal vamos utilizar separações por dominio, faz total sentido), `S.O.L.I.D` e ir implementando mais `patterns` após o time adquirir mais maturidade com os primeiros `patterns` propostos.

### Especificação dos microsserviços

> Utilizando o editor do SWAGGER (https://editor.swagger.io/), crie a especificação dos microsserviços a serem implementados na solução em um estilo arquitetural de microsserviços;

###### Swagger
O contrato também pode ser encontrado [aqui.](https://github.com/renatoaps/igti-atividade-ari/blob/main/microservice.yaml)
<details>
  <summary>microsservices.yaml</summary>
  
```
  swagger: "2.0"
info:
  version: "1.0.0"
  title: "Refatorando em Microsservices"
  termsOfService: "http://swagger.io/terms/"
  contact:
    email: "apiteam@swagger.io"
  license:
    name: "Apache 2.0"
    url: "http://www.apache.org/licenses/LICENSE-2.0.html"
host: "acme-producer.api.io"
basePath: "/v1"
tags:
- name: "Clientes"
  description: "API cliente domain"
- name: "Instalacoes"
  description: "API instalacao domain"
- name: "Faturas"
  description: "API fatura domain"
schemes:
- "https"
- "http"
paths:
  /clientes:
    post:
      tags:
      - "Clientes"
      summary: "Criar cliente na base"
      operationId: "inserirCliente"
      produces:
      - "application/json"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/Cliente"
      responses:
        "201":
          description: "cliente criado com sucesso"
      security:
        - apiKey: []
    get:
      tags:
      - "Clientes"
      summary: "Recupera info de todos os clientes"
      description: ""
      operationId: "getAllClienteData"
      produces:
      - "application/json"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosClientesCadastrados"
        "400":
          description: "Invalid cpf supplied"
        "404":
          description: "User not found"
      security:
        - apiKey: []
  /clientes/{cpf}:
    get:
      tags:
      - "Clientes"
      summary: "Recupera info por CPF"
      description: ""
      operationId: "getClienteById"
      produces:
      - "application/json"
      parameters:
      - name: "cpf"
        in: "path"
        required: true
        type: "string"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosCliente"
        "400":
          description: "Invalid cpf supplied"
        "404":
          description: "User not found"
      security:
        - apiKey: []

  /instalacoes:
    post:
      tags:
      - "Instalacoes"
      summary: "Criar instalacao na base"
      operationId: "inserirInstalacao"
      produces:
      - "application/json"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/Instalacao"
      responses:
        "201":
          description: "instalacao criada com sucesso"
      security:
        - apiKey: []
    get:
      tags:
      - "Instalacoes"
      summary: "Recupera todas as instalacoes"
      description: ""
      operationId: "getAllInstalacao"
      produces:
      - "application/json"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosInstalacaoCadastrados"
        "400":
          description: "Some error"
        "404":
          description: "Instalacao nao encontrada"
      security:
        - apiKey: []
  /instalacoes/{id}:
    get:
      tags:
      - "Instalacoes"
      summary: "Recupera info por ID"
      description: ""
      operationId: "getInstalacaoById"
      produces:
      - "application/json"
      parameters:
      - name: "id"
        in: "path"
        required: true
        type: "string"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosInstalacao"
        "400":
          description: "Invalid id supplied"
        "404":
          description: "Instalacao nao encontrada"
      security:
        - apiKey: []
        
  /instalacoes/cliente/{cpf}:
    get:
      tags:
      - "Instalacoes"
      summary: "Recupera info por CPF"
      description: ""
      operationId: "getInstalacaoByCpf"
      produces:
      - "application/json"
      parameters:
      - name: "cpf"
        in: "path"
        required: true
        type: "string"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosInstalacao"
        "400":
          description: "Invalid id supplied"
        "404":
          description: "Instalacao nao encontrada"
      security:
        - apiKey: []
        
  /faturas:
    post:
      tags:
      - "Faturas"
      summary: "Criar Faturas na base"
      operationId: "inserirFatura"
      produces:
      - "application/json"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/Fatura"
      responses:
        "201":
          description: "Faturas criada com sucesso"
      security:
        - apiKey: []
    get:
      tags:
      - "Faturas"
      summary: "Recupera todas as Faturas"
      description: ""
      operationId: "getAllFatura"
      produces:
      - "application/json"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosFaturasCadastradas"
        "400":
          description: "Some error"
        "404":
          description: "Instalacao nao encontrada"
      security:
        - apiKey: []
  /faturas/{id}:
    get:
      tags:
      - "Faturas"
      summary: "Recupera info por ID"
      description: ""
      operationId: "getFaturaById"
      produces:
      - "application/json"
      parameters:
      - name: "id"
        in: "path"
        required: true
        type: "string"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosFatura"
        "400":
          description: "Invalid id supplied"
        "404":
          description: "Fatura nao encontrada"
      security:
        - apiKey: []
  /faturas/cliente/{cpf}:
    get:
      tags:
      - "Faturas"
      summary: "Recupera info por CPF"
      description: ""
      operationId: "getFaturasByCpf"
      produces:
      - "application/json"
      parameters:
      - name: "cpf"
        in: "path"
        required: true
        type: "string"
      responses:
        "200":
          description: "successful operation"
          schema:
            $ref: "#/definitions/DadosFatura"
        "400":
          description: "Invalid id supplied"
        "404":
          description: "Instalacao nao encontrada"
      security:
        - apiKey: []
securityDefinitions:
  apiKey:
    type: apiKey
    in: header
    name: X-API-Key
    
definitions:
  DadosClientesCadastrados:
    type: "object"
    properties:
      dados:
        type: "array"
        items:
          $ref: "#/definitions/DadosCliente"
  DadosCliente:
    type: "object"
    properties:
      dadosCliente:
        $ref: "#/definitions/Cliente"
      dadosInstalacao:
        $ref: "#/definitions/DadosInstalacao"
  Cliente:
    type: "object"
    properties:
      id:
        type: "integer"
        format: "int64"
      nome:
        type: "string"
      dataNascimento:
        type: "string"
        format: "date"
      cpf:
        type: "string"
      endereco:
        $ref: "#/definitions/Endereco"
    xml:
      name: "Cliente"
  DadosInstalacaoCadastrados:
    type: "object"
    properties:
      dados:
        type: "array"
        items:
          $ref: "#/definitions/DadosInstalacao"
  DadosInstalacao:
    type: "object"
    properties:
      id:
        type: "string"
      endereco:
        $ref: "#/definitions/Endereco"
  Instalacao:
    type: "object"
    properties:
      endereco:
          $ref: "#/definitions/Endereco"
          
  DadosFaturasCadastradas:
    type: "object"
    properties:
      dados:
        type: "array"
        items:
          $ref: "#/definitions/DadosFatura"
  DadosFatura:
    type: "object"
    properties: 
      id:
        type: "integer"
      dadosFatura:
        $ref: "#/definitions/Fatura"
        
  Fatura:
    type: "object"
    properties:
      codigoInstalacao:
        type: "string"
      cpf:
        type: "string"
      dataVencimento:
        type: "string"
        format: "date"
      dataLeitura:
        type: "string"
        format: "date"
      valorLeitura:
        type: "number"
      valorConta:
        type: "number"
          
  Endereco:
    type: "object"
    properties:
      logradouro:
        type: "string"
      cep:
        type: "string"
      complemento:
        type: "string"
 ```
</details>
