## Documentação de Estudo: Métodos HTTP  

### O que são Métodos HTTP?  
Os **métodos HTTP** são comandos usados em requisições para indicar a ação desejada ao interagir com um recurso em um servidor. Eles formam a base do funcionamento das APIs e da comunicação cliente-servidor.


### Principais Métodos HTTP  

#### **1. GET**  
- **Descrição**: Utilizado para solicitar informações ou dados do servidor.  
- **Características**:  
  - Não modifica o estado do servidor.  
  - Considerado seguro (não provoca efeitos colaterais).  
  - Idempotente (o mesmo pedido sempre resulta na mesma resposta).  
- **Cabeçalhos comuns**:
  - `Authorization`: Para enviar tokens de autenticação.  
  - `Accept`: Define o formato da resposta esperada (ex.: JSON, XML).  
- **Exemplo de uso**:
  ```http
  GET /api/produtos HTTP/1.1
  Host: exemplo.com
  Authorization: Bearer token_de_acesso
  Accept: application/json
  ```
- **Resposta esperada** (200 OK):  
  ```json
  [
    { "id": 1, "nome": "Produto A", "preco": 10.0 },
    { "id": 2, "nome": "Produto B", "preco": 20.0 }
  ]
  ```

---

#### **2. POST**  
- **Descrição**: Usado para enviar dados ao servidor e criar novos recursos.  
- **Características**:  
  - Altera o estado do servidor (não é seguro).  
  - Não é idempotente (requisições repetidas podem criar duplicatas).  
- **Cabeçalhos comuns**:
  - `Content-Type`: Indica o formato dos dados enviados (ex.: `application/json`).  
  - `Authorization`: Para autenticação.  
- **Exemplo de uso**:
  ```http
  POST /api/produtos HTTP/1.1
  Host: exemplo.com
  Content-Type: application/json
  Authorization: Bearer token_de_acesso
  
  {
    "nome": "Produto C",
    "preco": 30.0
  }
  ```
- **Resposta esperada** (201 Created):  
  ```json
  { "id": 3, "nome": "Produto C", "preco": 30.0 }
  ```

---

#### **3. PUT**  
- **Descrição**: Atualiza um recurso existente ou substitui totalmente seus dados.  
- **Características**:  
  - Idempotente.  
  - Requer envio de todos os dados do recurso.  
- **Exemplo de uso**:
  ```http
  PUT /api/produtos/3 HTTP/1.1
  Host: exemplo.com
  Content-Type: application/json
  
  {
    "nome": "Produto C Atualizado",
    "preco": 35.0
  }
  ```

---

#### **4. PATCH**  
- **Descrição**: Atualiza parcialmente um recurso existente.  
- **Características**:  
  - Não idempotente (depende da implementação).  
- **Exemplo de uso**:
  ```http
  PATCH /api/produtos/3 HTTP/1.1
  Host: exemplo.com
  Content-Type: application/json
  
  {
    "preco": 40.0
  }
  ```

---

#### **5. DELETE**  
- **Descrição**: Remove um recurso do servidor.  
- **Características**:  
  - Idempotente.  
- **Exemplo de uso**:
  ```http
  DELETE /api/produtos/3 HTTP/1.1
  Host: exemplo.com
  Authorization: Bearer token_de_acesso
  ```

---

### Tabela Resumo  

| Método   | Finalidade            | Idempotente | Modifica o servidor | Status comum  |
|----------|------------------------|-------------|----------------------|---------------|
| GET      | Buscar dados           | Sim         | Não                  | 200 OK        |
| POST     | Criar recurso          | Não         | Sim                  | 201 Created   |
| PUT      | Substituir recurso     | Sim         | Sim                  | 200 OK, 204   |
| PATCH    | Atualizar parcialmente | Não         | Sim                  | 200 OK, 204   |
| DELETE   | Remover recurso        | Sim         | Sim                  | 204 No Content|

---

### Práticas Recomendadas  
1. Use **GET** apenas para leitura; nunca envie dados sensíveis pelo query string.  
2. Utilize **POST** quando criar novos recursos, evitando duplicação com identificadores únicos.  
3. Prefira **PUT** para atualizações completas e **PATCH** para alterações parciais.  
4. Sempre retorne os códigos de status corretos para cada método.  
5. Proteja requisições com autenticação (ex.: token JWT).  

