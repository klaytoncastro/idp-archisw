# Arquitetura de Software


## Apresentação da Disciplina

Bem-vindo à disciplina de Arquitetura de Software para os cursos de Engenharia de Software e Ciência da Computação. A arquitetura de software é uma área fundamental para o desenvolvimento de aplicações, sendo responsável por decisões que influenciam diretamente a manutenção, evolução e desempenho de sistemas. 

Esta disciplina tem como objetivo apresentar conceitos e práticas que norteiam a arquitetura de sistemas de software escaláveis, flexíveis e robustos e abordará os seguintes tópicos:

### Introdução à Arquitetura de Software:
- Definições e importância da arquitetura no ciclo de vida de desenvolvimento de software.
- Papéis e responsabilidades do arquiteto de software em equipes ágeis e tradicionais.

### Documentação de Arquitetura:
- Como documentar decisões arquiteturais.
- Documentação arquitetural utilizando Diagramas UML.
- Ferramentas de design, documentação e modelagem de software.

### Design Patterns:
- Padrões clássicos (ex: Singleton, Factory, Observer, Strategy, Chain of Responsibility) e sua relevância prática.
- Padrões emergentes e sua aplicação em arquiteturas modernas (ex: CQRS, Event Sourcing, Saga Pattern) e sua relevância prátic.
- Aplicação de padrões para lidar com problemas práticos de escalabilidade, consistência e integração de sistemas distribuídos.

### Estilos Arquiteturais:
- Estilos arquiteturais comuns (Monolítico, MVC, Microservices, SOA, Event-Driven).
- Impacto dos estilos arquiteturais na escalabilidade, manutenibilidade e evolução de sistemas complexos.

### Visões de Arquitetura:
- Visão Lógica: Organização e estrutura de componentes de software.
- Visão Física: Infraestrutura e como o software é implantado em diferentes ambientes (cloud, on-premises, híbrido).
- Visão de Desenvolvimento: Organização do código-fonte e integração com sistemas de controle de versão.
- Visão de Processos: Modelagem de processos concorrentes e intercomunicação entre sistemas.
- Modelo 4+1: Visões para documentação arquitetural e sua relevância no mercado.

## Objetivos de Aprendizado

Ao final da disciplina, você será capaz de:

- Compreender os diferentes estilos arquiteturais e como eles impactam o desenvolvimento e manutenção de sistemas.
- Projetar sistemas utilizando as melhores práticas de arquitetura.
- Identificar e aplicar padrões de design apropriados para solucionar problemas arquiteturais.
- Documentar uma arquitetura de software usando diagramas UML e outras técnicas.
- Avaliar arquiteturas quanto à sua escalabilidade, desempenho e segurança.

<!--

### Tecnologias Emergentes e Ferramentas de Suporte:
- Introdução à Infraestrutura como Código e sua aplicação na arquitetura de software: (Pipelines CI/CD), Orquestração de Containers (Docker, Kubernetes), Serverless Computing. 
- Ferramentas para modelagem, automação e testes arquiteturais (ex: Ansible, Terraform, Jenkins, Artifactory).
- Ferramentas de observabilidade (ex: Prometheus, Grafana) no monitoramento de arquiteturas complexas.


1. Introdução à Arquitetura de Software

Conexão com o mercado: como o papel do arquiteto de software evoluiu em startups, empresas de tecnologia e grandes corporações.
2. Estilos Arquiteturais
Arquitetura monolítica vs. microservices: Quando utilizar cada uma e suas implicações.
Arquitetura orientada a serviços (SOA) e arquitetura orientada a eventos (Event-Driven).

3. Padrões Arquiteturais
Padrões arquiteturais consolidados no mercado (Monolítica, Microservices, SOA, Event-Driven).
Escolha de padrões de acordo com os requisitos não funcionais: escalabilidade, desempenho, segurança e resiliência.
Discussão de casos de estudo reais e suas implementações no mercado (exemplos: Netflix, Amazon, Uber).
4. Visões de Arquitetura

5. Documentação de Arquitetura
Documentação arquitetural com base em práticas de mercado: como diferentes empresas abordam a documentação de sistemas de larga escala.
Uso de Diagramas UML e ferramentas colaborativas (ex: Lucidchart, Draw.io, Enterprise Architect).
Abordagem Lean para documentação arquitetural.
6. Design Patterns Aplicados à Arquitetura de Software


7. Arquiteturas Populares no Mercado
- Arquitetura em ambientes de alta disponibilidade (HA) e baixa latência (ex: sistemas financeiros, e-commerce).
- Arquitetura para o desenvolvimento de sistemas globais (multi-região, multi-cloud).
- Arquitetura evolutiva e abordagem baseada em migrações de sistemas legados para novas arquiteturas.

### 8. Tecnologias Emergentes e Ferramentas de Suporte
- Padrões de infraestrutura e tecnologias emergentes: containers (Docker, Kubernetes), serverless computing, edge computing, IA, e blockchain.
- Ferramentas para modelagem, automação e testes arquiteturais: Terraform, Ansible, Jenkins, AWS CloudFormation, entre outras.
- Ferramentas de observabilidade (ex: Prometheus, Grafana) no monitoramento e manutenção de arquiteturas complexas.

Conteúdo Programático
Introdução à Arquitetura de Software:

Definição, importância e desafios.
Diferença entre arquitetura e design detalhado.
Visões de Arquitetura:

Visão Lógica: Como o sistema é organizado em termos de componentes.
Visão de Desenvolvimento: Estrutura do código fonte, módulos e pacotes.
Visão Física: Componentes de hardware e sua interação com o software.
Visão de Processos: Modelagem de processos concorrentes e comunicação entre eles.
Diagramas UML:

Diagrama de Casos de Uso: Modelagem dos requisitos funcionais.
Diagrama de Classes: Modelagem da estrutura estática do sistema.
Diagrama de Sequência: Modelagem das interações entre componentes ao longo do tempo.
Diagrama de Componentes: Organização dos componentes de software e suas interações.
Diagrama de Estados: Representação do ciclo de vida dos objetos.
Padrões Arquiteturais:

Arquitetura Monolítica: Aplicações unificadas.
Microservices: Serviços independentes e escaláveis.
SOA: Arquitetura orientada a serviços.
Arquitetura Orientada a Eventos: Foco em comunicação assíncrona.
Design Patterns:

Singleton: Garantir uma única instância de uma classe.
Factory Method: Criação de objetos sem expor a lógica de criação ao cliente.
Observer: Comunicação entre objetos de forma desacoplada.
Chain of Responsibility: Passagem de responsabilidade de forma flexível.
Documentação e Qualidade de Arquitetura:


Avaliação da qualidade arquitetural (manutenibilidade, extensibilidade, desempenho).
Avaliação
A avaliação será baseada nos seguintes critérios:

Trabalhos Práticos (40%): Modelagem de sistemas utilizando diagramas UML.
Projeto Final (50%): Implementação de uma arquitetura completa para um sistema, utilizando os conceitos e padrões discutidos.
Participação e Discussão (10%): Envolvimento em discussões de casos práticos e debates sobre decisões arquiteturais.
Projeto Final
O projeto final consistirá na elaboração e documentação completa de uma arquitetura de software para um sistema de média complexidade. Os grupos deverão aplicar as visões de arquitetura, padrões de design e modelagem UML. A entrega deverá incluir:

Diagrama de Casos de Uso.
Diagrama de Classes.
Diagrama de Componentes.
Justificativa das escolhas arquiteturais e padrões.
Recursos
Para facilitar o aprendizado, recomenda-se a leitura dos seguintes materiais:

Livro "Software Architecture in Practice"
Design Patterns - Elements of Reusable Object-Oriented Software
Documentação UML

-->