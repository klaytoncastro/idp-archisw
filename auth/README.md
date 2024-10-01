# Desafio: Implementacao de Arquitetura para os Processos de Autenticação e Autorização
<!--

## Contextualizacao


## rbac


Explicação do RBAC (Role-Based Access Control)

RBAC (Role-Based Access Control) é um modelo de controle de acesso que define permissões de acordo com papéis atribuídos a usuários. Ao invés de conceder permissões diretamente aos indivíduos, as permissões são concedidas aos papéis, e os usuários são atribuídos a esses papéis.
Componentes do RBAC:

    Usuários (Users): São os sujeitos que tentam realizar ações no sistema. Cada usuário pode ser associado a um ou mais papéis.

    Papéis (Roles): Definem conjuntos de permissões. Exemplo de papéis: administrador, usuário padrão, moderador, etc.

    Permissões (Permissions): Definem quais ações os papéis podem realizar em quais recursos. Exemplo: editar, excluir, criar.

    Sessões (Sessions): Em algumas implementações, os usuários podem ativar certos papéis durante uma sessão.

Exemplo de RBAC:

    Um sistema de gerenciamento de conteúdo pode ter um papel de "editor", que tem permissões para editar e publicar conteúdo, e um papel de "leitor", que pode apenas visualizar conteúdo.

    O usuário "João" pode ter o papel de "editor", enquanto "Maria" pode ter o papel de "leitor".

Vantagens do RBAC:

    Facilidade de Gerenciamento: Permissões são concedidas a papéis e não a usuários individuais, facilitando a gestão de permissões em sistemas grandes.

    Menos Erros de Configuração: Como a gestão é centralizada nos papéis, há menor chance de erros na atribuição de permissões.

    Escalabilidade: Em sistemas com muitos usuários, é mais fácil adicionar ou remover papéis do que gerenciar permissões diretamente para cada usuário.

Exemplo de Implementação RBAC com Flask e MongoDB:

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
    app.run(debug=True)





## Attribute-Based Access Control

O ABAC (Attribute-Based Access Control) é uma abordagem de controle de acesso mais flexível e dinâmica que o tradicional RBAC (Role-Based Access Control). Enquanto o RBAC controla permissões com base em papéis predefinidos (como administrador ou usuário), o ABAC baseia-se em atributos para decidir se uma ação é permitida. No ABAC, o controle de acesso é feito com base em quatro tipos principais de atributos:

### Atributos do Sujeito (Subject Attributes):
- São características do usuário que solicita acesso.
- Exemplo: Nome do usuário, cargo, idade, localização geográfica, status de segurança, horário de trabalho.

### Atributos do Objeto (Object Attributes):
- São características do recurso ao qual o acesso é solicitado.
- Exemplo: Tipo do arquivo, classificação do documento, rótulo de segurança de uma informação.

### Atributos da Ação (Action Attributes):
- São as operações que o sujeito deseja executar no objeto.
- Exemplo: Leitura, escrita, exclusão, modificação, transferência.

### Atributos do Ambiente (Environment Attributes):
- São condições contextuais que podem influenciar as decisões de acesso.
- Exemplo: Data e hora da solicitação, localização do dispositivo, política de segurança vigente, estado da rede.

O controle de acesso é decidido pela avaliação de uma política que leva em consideração todos esses atributos em tempo real. As políticas são definidas de forma que diferentes combinações de atributos resultam em diferentes permissões.
Exemplos de ABAC em Ação

    Cenário de Empresa:
        Um funcionário pode acessar documentos financeiros, mas apenas durante o horário comercial (atributo de ambiente).
        Um usuário pode editar um documento somente se ele for o criador do documento (atributo do sujeito).

    Acesso Baseado em Localização:
        Um usuário de um aplicativo móvel pode acessar determinados dados somente se estiver em um local geográfico específico, como um escritório seguro (atributo do ambiente).

    Controle de Acesso à Nuvem:
        Um sistema na nuvem pode bloquear o acesso a dados críticos de uma empresa quando a solicitação vem de fora de uma região permitida (atributo do ambiente).

Padrões de Design Aplicáveis ao ABAC

O ABAC pode ser implementado de maneira eficaz utilizando alguns padrões de design arquitetural. Esses padrões ajudam a garantir que o sistema seja escalável, flexível e de fácil manutenção.
1. Factory Pattern para Criação de Atributos e Políticas

    O Factory Pattern pode ser usado para criar objetos relacionados aos atributos e políticas de controle de acesso de forma flexível. Por exemplo, diferentes atributos (como localização ou cargo) podem ser criados dinamicamente conforme a necessidade do sistema.

    Exemplo:

class AttributeFactory:
    def create_attribute(attribute_type):
        if attribute_type == 'location':
            return LocationAttribute()
        elif attribute_type == 'role':
            return RoleAttribute()
    Isso facilita a inclusão de novos atributos à medida que a lógica de controle de acesso evolui.

2. Policy Pattern para Avaliação de Políticas

    O Policy Pattern é crucial para implementar a lógica de avaliação de regras no ABAC. Nele, as políticas de acesso são abstraídas como objetos que podem ser combinados ou avaliados de acordo com os atributos.

    Exemplo:
        Uma política pode ser representada como uma classe que contém regras que devem ser satisfeitas com base nos atributos. As políticas podem ser combinadas ou agregadas de forma flexível para aplicar lógicas de permissão mais complexas.

class Policy:
    def is_allowed(self, subject, action, object, environment):
        # Avalia os atributos do sujeito, ação, objeto e ambiente.
        return (subject.role == 'admin' and environment.is_secure) or subject.department == 'finance'
Chain of Responsibility Pattern para Avaliação de Múltiplas Políticas

    O Chain of Responsibility Pattern pode ser usado quando múltiplas políticas precisam ser avaliadas em sequência. Cada política na cadeia pode aceitar ou rejeitar a solicitação com base nos atributos.

    Exemplo:
        Imagine que diferentes departamentos têm regras específicas de acesso. As políticas para esses departamentos podem ser representadas como uma cadeia onde cada política é verificada individualmente.

        class PolicyHandler:
    def __init__(self, next_handler=None):
        self.next_handler = next_handler

    def handle(self, request):
        if self.can_handle(request):
            return self.process(request)
        elif self.next_handler:
            return self.next_handler.handle(request)

class DepartmentPolicyHandler(PolicyHandler):
    def can_handle(self, request):
        return request.subject.department == 'finance'

    def process(self, request):
        # Avalia a política do departamento de finanças.
4. Strategy Pattern para Definir Dinamicamente as Políticas de Acesso

    O Strategy Pattern é útil quando diferentes políticas podem ser aplicadas a diferentes cenários, como o uso de diferentes algoritmos de controle de acesso. No ABAC, isso é especialmente útil, já que você pode definir estratégias baseadas em vários atributos (ex: um método para regras simples de RBAC e outro para regras dinâmicas de ABAC).

    Exemplo:
        Em um contexto onde as políticas de acesso variam de acordo com o tipo de ação, o Strategy Pattern pode ser usado para aplicar diferentes conjuntos de regras, como no exemplo abaixo.


class AccessStrategy:
    def evaluate(self, subject, action, object):
        pass

class RBACStrategy(AccessStrategy):
    def evaluate(self, subject, action, object):
        # Avalia usando RBAC

class ABACStrategy(AccessStrategy):
    def evaluate(self, subject, action, object):
        # Avalia usando ABAC com base nos atributos
5. Decorator Pattern para Adicionar Condicionalmente Regras de Acesso

    O Decorator Pattern permite adicionar ou modificar dinamicamente as regras de acesso de uma política sem alterar sua estrutura básica. Isso é útil no ABAC quando você deseja adicionar condições especiais para determinadas políticas, como incluir a avaliação de atributos de ambiente de forma opcional.

    Exemplo:

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

    def is_within_time_restriction(self):
        # Verifica se o horário está dentro das regras permitidas.
Benefícios do ABAC

    Flexibilidade: O ABAC permite criar políticas de controle de acesso muito mais detalhadas e específicas em comparação com o RBAC. É possível criar regras baseadas em qualquer combinação de atributos.

    Contexto Dinâmico: As políticas podem mudar com base em atributos dinâmicos, como horário ou localização, possibilitando controle adaptativo de acesso.

    Escalabilidade: Em sistemas complexos com muitos usuários e recursos, o ABAC oferece uma escalabilidade maior em termos de controle de acesso, ao evitar a explosão de papéis que frequentemente ocorre no RBAC.

Desafios do ABAC

    Complexidade: A implementação de políticas ABAC pode se tornar muito complexa, especialmente em grandes sistemas com muitos atributos e regras. Isso aumenta o esforço para manutenção e debugging.

    Desempenho: Como o ABAC avalia vários atributos e políticas em tempo real, pode haver um impacto no desempenho, especialmente se os atributos precisarem ser recuperados de bases de dados ou sistemas externos.

Conclusão

O ABAC é uma solução poderosa e flexível para controle de acesso em sistemas que precisam de uma abordagem adaptativa e baseada em múltiplos fatores. A aplicação de padrões de design, como Factory, Policy, Strategy, e Decorator, torna sua implementação mais modular, escalável e de fácil manutenção.

Se aplicado corretamente, o ABAC pode fornecer um controle de acesso muito mais preciso e contextual, sendo ideal para ambientes onde as condições de acesso podem mudar dinamicamente.

bcrypt é um algoritmo de hashing que foi desenvolvido especificamente para proteger senhas. Ao contrário de algoritmos de hashing comuns, como MD5 ou SHA-1, que são projetados para serem rápidos, o bcrypt foi desenvolvido para ser deliberadamente lento, tornando-o mais seguro contra ataques de força bruta e de "rainbow tables" (tabelas de pré-calculo de hashes). Além disso, ele inclui um fator de complexidade configurável que pode ser ajustado para tornar o processo de hashing mais lento à medida que o poder computacional aumenta.
Características do bcrypt

    Função de hash criptográfica:
        bcrypt é uma função de hash baseada no Blowfish cipher, mas simplificada para hash de senhas. O seu design inclui uma implementação adaptativa que permite aumentar o tempo necessário para computar um hash, retardando ataques com força bruta.

    Salt embutido:
        O bcrypt gera um salt aleatório e o inclui como parte do hash. O salt é um valor único que é adicionado à senha antes de aplicar o algoritmo de hash, o que impede que duas senhas idênticas gerem o mesmo hash.

    Fator de Custo:
        O bcrypt tem um fator de custo configurável, que define o número de rounds de hashing aplicados. Quanto maior o custo, mais tempo leva para gerar o hash, aumentando a segurança, pois torna o ataque de força bruta mais custoso.
        Exemplo de fator de custo: Se o custo for 12, isso significa que o algoritmo irá realizar 212212 rounds de hashing. Para cada aumento no custo, o tempo de computação dobra.

    Resistência a Ataques de Força Bruta:
        Ao ser projetado para ser mais lento do que outros algoritmos de hash, o bcrypt torna ataques de força bruta muito mais caros em termos de tempo e poder computacional.

Estrutura do bcrypt

O hash gerado pelo bcrypt tem um formato específico e consiste em três partes principais:

    Prefixo: Indica o algoritmo usado ($2b$ ou $2a$, sendo 2b a versão atualizada do algoritmo).
    Custo: O fator de custo, especificando quantos rounds de hashing foram aplicados (por exemplo, 12).
    Salt e Hash: O salt aleatório utilizado e o hash resultante.

Exemplo de hash gerado pelo bcrypt:

$2b$12$P1ayq0y4bvnhkfJnULyW8e.Q3xHslTpBmQyXQeWvQj/9dZwR9fENa


Aqui:

    $2b$: Versão do algoritmo.
    12: Fator de custo.
    P1ayq0y4bvnhkfJnULyW8e: Salt gerado.
    .Q3xHslTpBmQyXQeWvQj/9dZwR9fENa: Hash resultante da senha e do salt.

Implementação do bcrypt

Abaixo está um exemplo de como usar o bcrypt em Python para gerar e verificar senhas:
Instalação:

pip install bcrypt


import bcrypt

# Gera um hash para a senha
def hash_password(password):
    # Gera o salt automaticamente e aplica o hashing
    salt = bcrypt.gensalt(rounds=12)  # 12 é o fator de custo (ajustável)
    hashed_password = bcrypt.hashpw(password.encode('utf-8'), salt)
    return hashed_password

# Verifica se a senha está correta
def check_password(stored_hash, password):
    # Verifica se a senha fornecida corresponde ao hash armazenado
    return bcrypt.checkpw(password.encode('utf-8'), stored_hash)

# Exemplo de uso:
plain_password = 'minhasenha123'
hashed = hash_password(plain_password)

# Armazenar 'hashed' no banco de dados

# Ao tentar fazer login:
if check_password(hashed, 'minhasenha123'):
    print("A senha está correta!")
else:
    print("Senha incorreta!")
Vantagens do bcrypt

    Resistência a Rainbow Tables: O uso do salt previne ataques baseados em tabelas de pré-calculo de hashes. Mesmo que duas senhas idênticas sejam hashadas, os hashes resultantes serão diferentes por causa do salt único.

    Função de Custo Configurável: Ao ajustar o fator de custo, é possível aumentar a dificuldade de calcular o hash, tornando ataques de força bruta menos viáveis.

    Resistência a ataques paralelos: O bcrypt foi projetado para dificultar a paralelização de cálculos de hash, dificultando ainda mais o uso de hardware especializado (como GPUs) para realizar ataques.

Quando Usar bcrypt?

    bcrypt é amplamente utilizado para proteger senhas de usuários em sistemas de autenticação. Qualquer sistema que necessite armazenar senhas de forma segura deve optar por bcrypt ou outros algoritmos de hash específicos para senhas, como Argon2 ou PBKDF2.
    Quando se deseja implementar proteção de longo prazo para senhas e prevenir a possibilidade de comprometimento em caso de vazamento de banco de dados.

Comparação com Outros Algoritmos

    MD5 e SHA-1: Não são recomendados para armazenar senhas. Eles são rápidos e projetados para gerar hashes rapidamente, o que facilita ataques de força bruta. Além disso, MD5 e SHA-1 são vulneráveis a colisões (duas entradas diferentes resultam no mesmo hash).

    PBKDF2: Um algoritmo de derivação de chave que, assim como o bcrypt, permite especificar o número de rounds. Ele é amplamente utilizado, mas pode ser menos eficiente do que bcrypt ao lidar com ataques de hardware especializado, como GPUs.

    Argon2: Um algoritmo mais moderno que ganhou o concurso de senha segura do Password Hashing Competition (PHC). Ele oferece resistência a ataques de força bruta e é configurável tanto em termos de memória quanto de tempo de execução. É considerado mais seguro que bcrypt, mas pode não estar disponível em todas as plataformas.

Conclusão

O bcrypt é um dos melhores algoritmos para hash de senhas, sendo amplamente utilizado devido à sua segurança e flexibilidade. A combinação de fator de custo, salt embutido e resistência a ataques paralelos o torna uma excelente escolha para proteger senhas em sistemas de autenticação.
   
Utilize os slides de referencia.-->


## Descrição do Projeto

Neste desafio, você implementará autenticação e autorização em um sistema simples utilizando **Python Flask** no backend, **MongoDB** para persistência de dados, e **JWT** para autenticação e autorização de usuários. O foco será na aplicação dos conceitos de **RBAC (Role-Based Access Control)** e **ABAC (Attribute-Based Access Control)**, além da proteção de senhas usando **bcrypt**.

## Tecnologias Utilizadas
- **Python Flask**: Framework para o backend RESTful.
- **MongoDB**: Base de dados NoSQL para armazenamento de usuários e permissões.
- **JWT (JSON Web Tokens)**: Para autenticação e autorização sem estado.
- **bcrypt**: Para hashing seguro de senhas.
- **Docker**: Para containerizar e gerenciar o ambiente da aplicação.

## Estrutura do Desafio

### 1. Autenticação de Usuários com bcrypt

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

### 2. Controle de Acesso com RBAC

No RBAC (Role-Based Access Control), usuários recebem papéis que definem suas permissões. Cada papel terá permissões para realizar determinadas ações no sistema.
Exemplo de Código RBAC:

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

### 3. Controle de Acesso com ABAC

Com ABAC (Attribute-Based Access Control), vamos implementar uma lógica onde as permissões são controladas por atributos dinâmicos dos usuários, objetos e ambiente.
Exemplo de Código ABAC:

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

### 4. Armazenamento de Dados com MongoDB

Neste desafio, utilizaremos MongoDB para armazenar os usuários e suas permissões.
Exemplo de Código para Inserir e Consultar Usuários:

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

### 5. JWT para Autenticação e Autorização

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

### 6. Como rodar o projeto? 

Vamos configurar o ambiente para rodar em containers Docker. Usaremos Docker Compose para gerenciar os serviços. Voce precisa do WSL e Docker para rodar em um ambiente Windows. 

Clone este repositorio e suba os conteineres do Flask e MongoDB. 

<!-- INSERIR TUTORIAL-->


