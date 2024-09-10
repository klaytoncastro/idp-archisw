# Design Patterns

A turma será dividida em 4 grupos. Cada grupo receberá um desafio específico que abrange a arquitetura completa de um sistema, com foco na escolha adequada de banco de dados (SQL ou NoSQL), design patterns e modelagem UML. Cada grupo deve propor a solução mais apropriada, justificando suas escolhas. Utilize o material de referência sobre [Design Patterns](https://github.com/klaytoncastro/idp-archisw/blob/main/patterns/DesignPatterns.pdf) para implementar os padrões. 

## Grupo 1: Sistema de Cache Dinâmico para Site de Alta Demanda

**Integrantes**:  Távora, Bee, Petrus, Vitor

**Desafio:** Desenvolver uma solução completa para um sistema de cache dinâmico que permita o acesso rápido a informações em um site de alta demanda (como e-commerce ou rede social). O sistema deve ser escalável e eficiente no gerenciamento de atualizações de cache em tempo real.

### Requisitos:
- Escolher o banco de dados adequado para suportar o cache e alta demanda de acessos.
- Implementar o Design Pattern **Proxy** para gerenciar o acesso ao cache e otimizar as requisições.
- Utilizar o Pattern **Observer** para monitorar as atualizações nos dados e invalidar o cache quando necessário.
- Propor um diagrama de classes UML que ilustre as interações entre o cache, a base de dados e os clientes.
- Criar um diagrama de sequência que mostre o fluxo de dados entre a aplicação, o cache e a base de dados.
- Arquitetura escalável com foco em **alta disponibilidade** e **baixa latência**.

---

## Grupo 2: Gerenciamento e Distribuição em Tempo Real de Conteúdo Digital

**Integrantes**: Mateus Batista, Lucas Rabelo, João Henrique

**Desafio:** Projetar uma solução para o gerenciamento e distribuição em tempo real de conteúdo diversificado (textos, vídeos, imagens) para uma plataforma digital de notícias. O sistema deve suportar grandes volumes de atualizações e permitir consultas rápidas.

### Requisitos:
- Escolher o banco de dados adequado para armazenar e acessar diversos tipos de conteúdo de forma eficiente.
- Utilizar o Design Pattern **Factory Method** para instanciar diferentes tipos de conteúdo de maneira dinâmica.
- Implementar o Pattern **Strategy** para permitir algoritmos flexíveis de categorização e busca.
- Propor um diagrama de classes UML que ilustre a criação e o gerenciamento dos diversos tipos de conteúdo.
- Desenvolver um diagrama de sequência que mostre como o conteúdo é criado, armazenado e distribuído em tempo real.
- Arquitetura com foco em **escalabilidade horizontal** e suporte a grandes volumes de dados.

---

## Grupo 3: Coleta e Análise de Séries Temporais de Dados IoT

**Integrantes**: Matheus Antônio, Leonardo Freitas, Maria Fernanda

**Desafio:** Desenvolver uma arquitetura completa para coletar, armazenar e analisar grandes volumes de dados de séries temporais provenientes de dispositivos IoT. O sistema deve permitir análises detalhadas e previsões baseadas nesses dados.

### Requisitos:
- Escolher o banco de dados adequado para armazenar grandes volumes de dados de séries temporais e permitir análises detalhadas.
- Utilizar o Design Pattern **Repository** para centralizar a lógica de persistência dos dados.
- Aplicar o Pattern **Observer** para monitorar novos dados de dispositivos IoT e acionar o processamento em tempo real.
- Propor um diagrama de classes UML que modele a interação entre os dispositivos, o sistema de armazenamento e as análises.
- Criar um diagrama de sequência que ilustre o fluxo de coleta, armazenamento e análise dos dados de séries temporais.
- Arquitetura com foco em **alta disponibilidade**, **baixa latência** e **processamento em tempo real**.

---

## Grupo 4: Sistema de Otimização de Rotas e Logística

**Integrantes**: Rafael Cândido, Luca Verdade, Lucas Fidalgo, Vinicius (4)

**Desafio:** Construir um sistema completo para modelar e resolver questões de logística e otimização de rotas de entrega. O sistema deve lidar com variáveis como distâncias, custos e capacidades dos veículos para oferecer soluções eficientes.

### Requisitos:
- Escolher o banco de dados adequado para modelar redes complexas de rotas e armazenar os dados de maneira eficiente.
- Utilizar o Design Pattern **Command** para estruturar operações de planejamento de rotas, como adição, remoção e ajustes.
- Aplicar o Pattern **Singleton** para gerenciar o algoritmo de otimização de forma centralizada e eficiente.
- Propor um diagrama de classes UML que modele a lógica de rotas, custos e capacidades.
- Criar um diagrama de casos de uso para demonstrar as interações do usuário com o sistema de planejamento de rotas.
- Arquitetura com foco em **otimização de algoritmos**, **processamento eficiente** e suporte a grandes volumes de dados.

---

### Entregáveis:
- Arquitetura completa do sistema (incluindo a escolha do banco de dados SQL/NoSQL).
- Diagrama de Classes UML.
- Diagrama de Sequência ou Casos de Uso (dependendo do desafio).
- Justificativa detalhada das escolhas de design patterns e arquitetura de banco de dados.
- Data de Apresentação: **19/09/2024**
