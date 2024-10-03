# Implementacao de Arquitetura de Software para os Processos de Autenticação e Autorização

## 1. Introdução

Os processos de autenticação e autorização são fundamentais para a segurança em sistemas de informação, sendo utilizados em quase todas as aplicações web,  móveis e sistemas modernos. Neste projeto, você aplicará o conhecimento de arquitetura de software para implementar o controle de acesso, um recurso para proteger os recursos de uma aplicação, de plataformas de gerenciamento de usuários, de permissões em recursos de grandes corporações, aplicativos bancários e sistemas de gerenciamento de conteúdo. Você desenvolverá habilidades procuradas no mercado, aplicando esses conceitos por meio de tecnologias populares. Ao concluir o projeto, você será capaz de implementar autenticação e autorização em um sistema, preferencialmente utilizando Python Flask no backend, MongoDB para persistência de dados e JWT para autenticação e autorização de usuários durante o uso da aplicação. Além disso, aplicará os conceitos de RBAC (Role-Based Access Control) e ABAC (Attribute-Based Access Control), juntamente com a proteção de senhas utilizando bcrypt.

## Tecnologias Utilizadas
- **Python Flask**: Framework para o backend RESTful.
- **MongoDB**: Base de dados NoSQL para armazenamento de usuários e permissões.
- **JWT (JSON Web Tokens)**: Para autenticação e autorização sem estado.
- **bcrypt**: Para hashing seguro de senhas.
- **Docker**: Para containerizar e gerenciar o ambiente da aplicação.

## 2. RBAC (Role-Based Access Control)

**RBAC** (Role-Based Access Control) é um modelo de controle de acesso que define permissões de acordo com papéis atribuídos a usuários. Ao invés de conceder permissões diretamente aos indivíduos, as permissões são concedidas aos papéis, e os usuários são atribuídos a esses papéis.

### Componentes do RBAC:

- **Usuários (Users)**: São os sujeitos que tentam realizar ações no sistema. Cada usuário pode ser associado a um ou mais papéis.
- **Papéis (Roles)**: Definem conjuntos de permissões. Exemplo de papéis: administrador, usuário padrão, moderador, etc.
- **Permissões (Permissions)**: Definem quais ações os papéis podem realizar em quais recursos. Exemplo: editar, excluir, criar.
- **Sessões (Sessions)**: Em algumas implementações, os usuários podem ativar certos papéis durante uma sessão.

### Exemplo de RBAC:

- Um sistema de gerenciamento de conteúdo pode ter um papel de "editor", que tem permissões para editar e publicar conteúdo, e um papel de "leitor", que pode apenas visualizar conteúdo.
- O usuário "João" pode ter o papel de "editor", enquanto "Maria" pode ter o papel de "leitor".

### Vantagens do RBAC:

- **Facilidade de Gerenciamento**: Permissões são concedidas a papéis e não a usuários individuais, facilitando a gestão de permissões em sistemas grandes.
- **Menos Erros de Configuração**: Como a gestão é centralizada nos papéis, há menor chance de erros na atribuição de permissões.
- **Escalabilidade**: Em sistemas com muitos usuários, é mais fácil adicionar ou remover papéis do que gerenciar permissões diretamente para cada usuário.

### Exemplo de Implementação RBAC com Flask e MongoDB:

```python
from flask import Flask, jsonify, request
from functools import wraps

app = Flask(__name__)

# Simulando um banco de dados
users_db = {
    "joao": {"role": "editor"},
    "maria": {"role": "reader"}
}

roles_permissions = {
    "editor": ["create", "edit", "delete"],
    "reader": ["read"]
}

# Decorador para verificar permissões
def role_required(required_role):
    def decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            username = request.args.get('username')
            user = users_db.get(username)
            if not user or user['role'] != required_role:
                return jsonify({"message": "Unauthorized"}), 403
            return f(*args, **kwargs)
        return wrapper
    return decorator

@app.route('/edit', methods=['POST'])
@role_required('editor')
def edit_content():
    return jsonify({"message": "Content edited"})

@app.route('/read', methods=['GET'])
@role_required('reader')
def read_content():
    return jsonify({"message": "Content read"})

if __name__ == '__main__':
    app.run(host = 0.0.0.0, debug=True)
```

### Alguns Design Aplicáveis ao RBAC

O **Factory Pattern** pode ser usado para criar papéis (roles) dinamicamente no sistema. Isso é útil quando há diferentes tipos de papéis que podem ser gerados com base em parâmetros fornecidos:

```python
# Factory Pattern para criar diferentes papéis no sistema
class Role:
    def __init__(self, name, permissions):
        self.name = name
        self.permissions = permissions

class RoleFactory:
    def create_role(self, role_type):
        if role_type == 'admin':
            return Role('admin', ['create', 'edit', 'delete', 'view'])
        elif role_type == 'user':
            return Role('user', ['view'])
        elif role_type == 'editor':
            return Role('editor', ['edit', 'view'])

# Exemplo de uso
factory = RoleFactory()
admin_role = factory.create_role('admin')
editor_role = factory.create_role('editor')

print(admin_role.name, admin_role.permissions)  # Saída: admin ['create', 'edit', 'delete', 'view']
print(editor_role.name, editor_role.permissions)  # Saída: editor ['edit', 'view']
```

O **Policy Pattern** pode ser usado para encapsular a lógica de permissão de ações dentro de uma classe, permitindo que seja facilmente modificada ou substituída:

```python
class Policy:
    def is_allowed(self, user, action):
        raise NotImplementedError

class AdminPolicy(Policy):
    def is_allowed(self, user, action):
        return action in ['create', 'edit', 'delete', 'view']

class UserPolicy(Policy):
    def is_allowed(self, user, action):
        return action == 'view'

# Exemplo de uso:
admin_policy = AdminPolicy()
user_policy = UserPolicy()

print(admin_policy.is_allowed('admin', 'delete'))  # Saída: True
print(user_policy.is_allowed('user', 'delete'))  # Saída: False
```

O **Chain of Responsibility** pode ser usado para fazer a avaliação de múltiplas políticas em sequência, permitindo que cada uma trate um aspecto da permissão. Se uma política não pode tratar a solicitação, ela passa para a próxima na cadeia:

```python
class PolicyHandler:
    def __init__(self, next_handler=None):
        self.next_handler = next_handler

    def handle(self, user, action):
        if self.can_handle(user, action):
            return self.process(user, action)
        elif self.next_handler:
            return self.next_handler.handle(user, action)
        return False

    def can_handle(self, user, action):
        raise NotImplementedError

    def process(self, user, action):
        raise NotImplementedError

class AdminPolicyHandler(PolicyHandler):
    def can_handle(self, user, action):
        return user['role'] == 'admin'

    def process(self, user, action):
        return action in ['create', 'edit', 'delete', 'view']

class UserPolicyHandler(PolicyHandler):
    def can_handle(self, user, action):
        return user['role'] == 'user'

    def process(self, user, action):
        return action == 'view'

# Criando a cadeia de responsabilidade
admin_handler = AdminPolicyHandler()
user_handler = UserPolicyHandler(admin_handler)

# Exemplo de uso
user = {'role': 'user'}
action = 'view'
print(user_handler.handle(user, action))  # Saída: True

admin = {'role': 'admin'}
action = 'delete'
print(user_handler.handle(admin, action))  # Saída: True
```

O **Strategy Pattern** pode ser usado para definir diferentes estratégias de controle de acesso (RBAC, ABAC, etc.) e selecionar dinamicamente a melhor política para o contexto:

```python

class AccessStrategy:
    def evaluate(self, user, action):
        pass

class RBACStrategy(AccessStrategy):
    def evaluate(self, user, action):
        role = user['role']
        if role == 'admin':
            return action in ['create', 'edit', 'delete', 'view']
        elif role == 'user':
            return action == 'view'
        return False

class ABACStrategy(AccessStrategy):
    def evaluate(self, user, action):
        # Atribuir lógica baseada em atributos dinâmicos aqui
        if user['location'] == 'office' and action == 'view':
            return True
        return False

# Exemplo de uso
rbac_strategy = RBACStrategy()
abac_strategy = ABACStrategy()

user = {'role': 'admin'}
print(rbac_strategy.evaluate(user, 'edit'))  # Saída: True

user = {'location': 'office'}
print(abac_strategy.evaluate(user, 'view'))  # Saída: True
```

O **Decorator Pattern** pode ser usado para adicionar restrições ou requisitos adicionais às permissões sem alterar a lógica principal:

```python

class Policy:
    def is_allowed(self, user, action):
        raise NotImplementedError

class BasicPolicy(Policy):
    def is_allowed(self, user, action):
        return action in ['view', 'edit']

class PolicyDecorator(Policy):
    def __init__(self, decorated_policy):
        self.decorated_policy = decorated_policy

    def is_allowed(self, user, action):
        return self.decorated_policy.is_allowed(user, action)

class TimeRestrictedPolicy(PolicyDecorator):
    def is_allowed(self, user, action):
        # Exemplo: Só permite acesso durante o horário comercial
        current_hour = 14  # Exemplo de hora atual
        if 9 <= current_hour <= 17:
            return self.decorated_policy.is_allowed(user, action)
        return False

# Exemplo de uso:
basic_policy = BasicPolicy()
time_restricted_policy = TimeRestrictedPolicy(basic_policy)

print(time_restricted_policy.is_allowed('user', 'view'))  # Saída: True ou False dependendo da hora
```

## 3. ABAC (Attribute-Based Access Control) 

ABAC é uma abordagem de controle de acesso mais flexível e dinâmica que o tradicional RBAC. Enquanto o RBAC controla permissões com base em papéis predefinidos (como administrador ou usuário), o ABAC baseia-se em atributos para decidir se uma ação é permitida. O controle de acesso é feito com base em quatro tipos principais de atributos:

### Atributos do Sujeito (Subject Attributes):

São características do usuário que solicita acesso. 
- Exemplo: Nome do usuário, cargo, idade, localização geográfica, status de segurança, horário de trabalho.

### Atributos do Objeto (Object Attributes):

São características do recurso ao qual o acesso é solicitado.
- Exemplo: Tipo do arquivo, classificação do documento, rótulo de segurança de uma informação.

### Atributos da Ação (Action Attributes):

São as operações que o sujeito deseja executar no objeto.
- Exemplo: Leitura, escrita, exclusão, modificação, transferência.

### Atributos do Ambiente (Environment Attributes):

São condições contextuais que podem influenciar as decisões de acesso.
- Exemplo: Data e hora da solicitação, localização do dispositivo, política de segurança vigente, estado da rede.

### Alguns Design Aplicáveis ao ABAC: 

```python
#Factory Pattern para Criação de Atributos e Políticas:
class AttributeFactory:
    def create_attribute(attribute_type):
        if attribute_type == 'location':
            return LocationAttribute()
        elif attribute_type == 'role':
            return RoleAttribute()

#Policy Pattern para Avaliação de Políticas:
class Policy:
    def is_allowed(self, subject, action, object, environment):
        return (subject.role == 'admin' and environment.is_secure) or subject.department == 'finance'

#Chain of Responsibility para Avaliação de Múltiplas Políticas:
class PolicyHandler:
    def __init__(self, next_handler=None):
        self.next_handler = next_handler

    def handle(self, request):
        if self.can_handle(request):
            return self.process(request)
        elif self.next_handler:
            return self.next_handler.handle(request)

# Strategy Pattern para Definir Dinamicamente as Políticas de Acesso:
class AccessStrategy:
    def evaluate(self, subject, action, object):
        pass

class RBACStrategy(AccessStrategy):
    def evaluate(self, subject, action, object):
        # Avalia usando RBAC

class ABACStrategy(AccessStrategy):
    def evaluate(self, subject, action, object):
        # Avalia usando ABAC com base nos atributos

#Decorator Pattern para Adicionar Condicionalmente Regras de Acesso:
class Policy:
    def is_allowed(self, subject, action, object):
        pass

class TimeRestrictedPolicyDecorator(Policy):
    def __init__(self, policy):
        self.policy = policy

    def is_allowed(self, subject, action, object):
        if self.is_within_time_restriction():
            return self.policy.is_allowed(subject, action, object)
        return False
```

### 4. Autenticação e Autorização de Usuários 

Vamos usar **bcrypt** para garantir que as senhas dos usuários sejam armazenadas de maneira segura.

#### Exemplo de Código:

```python
import bcrypt

# Gera um hash para a senha
def hash_password(password):
    salt = bcrypt.gensalt()
    hashed_password = bcrypt.hashpw(password.encode('utf-8'), salt)
    return hashed_password

# Verifica se a senha está correta
def check_password(stored_hash, password):
    return bcrypt.checkpw(password.encode('utf-8'), stored_hash)

# Exemplo de uso:
password = 'senha_segura123'
hashed = hash_password(password)

# Armazenar 'hashed' no banco de dados
print(check_password(hashed, 'senha_segura123'))  # True se a senha for correta
```

### Benefícios do ABAC

- Flexibilidade: O ABAC permite criar políticas de controle de acesso mais detalhadas e específicas em comparação com o RBAC.
- Contexto Dinâmico: As políticas podem mudar com base em atributos dinâmicos, como horário ou localização.
- Escalabilidade: O ABAC oferece maior escalabilidade ao evitar a explosão de papéis que ocorre no RBAC.

### Desafios do ABAC

- Complexidade: A implementação de políticas ABAC pode se tornar muito complexa, especialmente em grandes sistemas.
- Desempenho: Avaliar vários atributos e políticas em tempo real pode impactar o desempenho, especialmente se os atributos precisarem ser recuperados de bases de dados externas. Em aplicações modernas, o Redis é uma boa opção nesse cenário. 

### Controle de Acesso com RBAC

No RBAC, vimos usuários recebem papéis que definem suas permissões. Cada papel terá permissões para realizar determinadas ações no sistema:

```python
@app.route('/edit', methods=['POST'])
@role_required('editor')
def edit_content():
    return jsonify({"message": "Content edited"})

@app.route('/read', methods=['GET'])
@role_required('reader')
def read_content():
    return jsonify({"message": "Content read"})
```

### Controle de Acesso com ABAC

Com ABAC (Attribute-Based Access Control), vimos que a lógica é controlar as permissões de acordo com atributos dinâmicos dos usuários, objetos e ambiente:

```python
def has_permission(user, action, resource):
    if user['role'] == 'admin':
        return True
    elif user['role'] == 'user' and action == 'read':
        return True
    return False

@app.route('/resource', methods=['GET', 'POST'])
def resource_access():
    username = request.args.get('username')
    action = request.method.lower()
    user = users_db.get(username)
    
    if not has_permission(user, action, 'resource'):
        return jsonify({"message": "Access Denied"}), 403
    
    return jsonify({"message": f"{action.capitalize()} access granted"})
```

## 5. Armazenamento de Dados com MongoDB

Para incrementar este desafio, utilizaremos MongoDB para armazenar os usuários e suas permissões:

```python
from pymongo import MongoClient

client = MongoClient('localhost', 27017)
db = client['auth_system']
users_collection = db['users']

# Inserindo usuário
user = {"username": "joao", "password": hash_password("minhasenha"), "role": "editor"}
users_collection.insert_one(user)

# Consultando usuário
user = users_collection.find_one({"username": "joao"})
print(user)
```

## 6. JWT para Autenticação e Autorização

Vamos utilizar JWT para autenticar os usuários e permitir o acesso a rotas protegidas.

```python
import jwt
import datetime

# Função para gerar JWT
def generate_jwt(user):
    token = jwt.encode({
        'user': user['username'],
        'exp': datetime.datetime.utcnow() + datetime.timedelta(minutes=30)
    }, 'secret_key', algorithm='HS256')
    return token

# Função para verificar JWT
def decode_jwt(token):
    try:
        return jwt.decode(token, 'secret_key', algorithms=['HS256'])
    except jwt.ExpiredSignatureError:
        return None

@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    
    user = users_db.get(username)
    if user and check_password(user['password'], password):
        token = generate_jwt(user)
        return jsonify({"token": token})
    
    return jsonify({"message": "Invalid credentials"}), 401
```

## 7. Atividade em Grupo 

Embora o RBAC seja um modelo de controle de acesso muito popular, o ABAC oferece uma abordagem mais flexível, ideal para cenários onde o controle dinâmico de permissões é necessário. Contudo, o ABAC pode ser mais complexo de implementar e pode ter impactos no desempenho, especialmente em grandes sistemas. Com base no que vimos, nosso próximo desafio será implementar um sistema de autenticação e autorização, aplicando JWT, RBAC ou ABAC, usando os conceitos discutidos e  Design Patterns:

- Utilize Factory Pattern para criar dinamicamente papéis e permissões no RBAC ou atributos no ABAC.
- Implemente o Strategy Pattern para permitir que o sistema escolha dinamicamente qual abordagem (RBAC ou ABAC) usar para gerenciar permissões.
- Utilize Decorator Pattern para adicionar restrições como tempo e localização às políticas de acesso.





### Como rodar o projeto? 

Vamos configurar o ambiente para rodar em containers Docker. Usaremos Docker Compose para gerenciar os serviços. Clone este repositorio e suba os conteineres do Flask e MongoDB, conforme instruções do professor.


