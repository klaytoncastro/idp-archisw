# Tarefa: Criação de Diagramas UML para um Sistema de Reservas Online

## Objetivo da Tarefa

Vocês irão trabalhar em duplas para criar diferentes tipos de diagramas UML (Unified Modeling Language). Esses diagramas ajudam a visualizar e planejar a estrutura e o funcionamento de um sistema de software, como um "Sistema de Reservas Online". Cada dupla será responsável por um tipo específico de diagrama, que é como uma "foto" de uma parte do sistema. No final, todos os diagramas juntos darão uma visão completa de como o sistema funciona.

## Explicação dos Diagramas

### 1. Diagrama de Casos de Uso
- **O que é?**: Este diagrama mostra o que o sistema deve fazer e quem vai interagir com ele. Ele foca nas funcionalidades (chamadas de "casos de uso") e nas pessoas ou outros sistemas que vão usar essas funcionalidades (chamados de "atores").
- **Exemplo**:
  - **Atores**: Cliente, Administrador.
  - **Casos de Uso**: Fazer Reserva, Cancelar Reserva, Consultar Disponibilidade, Gerenciar Reservas.
- **Por que é importante?**: Ajuda a entender os requisitos principais do sistema, ou seja, o que o sistema deve fazer.

### 2. Diagrama de Classes
- **O que é?**: Este diagrama mostra a "estrutura" do sistema, como uma planta baixa de uma casa. Ele exibe as "classes" (como as categorias principais do sistema), os "atributos" dessas classes (características), e os "métodos" (ações que elas podem executar). Também mostra como essas classes se relacionam entre si.
- **Exemplo**:
  - **Classes**: Reserva, Cliente, Quarto, Pagamento.
  - **Atributos**: reservaID, data, hora (para a classe Reserva).
  - **Métodos**: criarReserva(), cancelarReserva().
- **Por que é importante?**: Dá uma visão clara de como o sistema está organizado internamente, ajudando a planejar o código.

### 3. Diagrama de Sequência
- **O que é?**: Este diagrama mostra como os diferentes "objetos" do sistema interagem entre si ao longo do tempo para realizar uma tarefa específica. Ele é como um roteiro que descreve passo a passo o que acontece quando alguém, por exemplo, faz uma reserva.
- **Exemplo**:
  - **Interações**: Cliente faz uma reserva -> Sistema consulta disponibilidade -> Quarto responde com disponibilidade -> Sistema cria a reserva -> Sistema processa o pagamento.
- **Por que é importante?**: Ajuda a entender o fluxo de atividades e como os diferentes componentes do sistema trabalham juntos.

### 4. Diagrama de Estados
- **O que é?**: Este diagrama mostra os diferentes estados pelos quais um objeto pode passar durante sua vida útil, e o que faz ele mudar de um estado para outro. No caso de uma reserva, ele mostraria como ela vai de "Criada" para "Confirmada" e possivelmente para "Cancelada".
- **Exemplo**:
  - **Estados**: Reserva Criada, Reserva Confirmada, Reserva Cancelada.
  - **Transições**: Do estado "Criada" para "Confirmada" quando o pagamento é realizado, ou para "Cancelada" se o cliente decidir cancelar.
- **Por que é importante?**: Ajuda a entender o ciclo de vida de uma reserva e as condições que a afetam.

### 5. Diagrama de Componentes
- **O que é?**: Este diagrama mostra os diferentes "blocos" do sistema (chamados de componentes) e como eles se conectam. É como um mapa que mostra os diferentes módulos de software e como eles interagem.
- **Exemplo**:
  - **Componentes**: Módulo de Reservas, Módulo de Pagamentos, Módulo de Gestão de Clientes.
  - **Conexões**: Mostra como o Módulo de Pagamentos se comunica com o Módulo de Reservas, por exemplo.
- **Por que é importante?**: Dá uma visão de alto nível da arquitetura do sistema, mostrando como as partes do software se conectam.

## Como os Diagramas se Relacionam
- **Casos de Uso**: Mostra o que o sistema deve fazer.
- **Classes**: Mostra como o sistema está estruturado para realizar esses casos de uso.
- **Sequência**: Mostra como as classes interagem para realizar uma tarefa.
- **Estados**: Mostra os possíveis estados de um objeto e como ele transita entre eles.
- **Componentes**: Mostra os módulos maiores e como eles se conectam para realizar as funcionalidades.

## Desenvolvimento dos Diagramas
- Cada dupla será responsável por desenhar um diagrama específico. Trabalhem juntos para entender o que cada elemento do diagrama representa e como conectá-los.
- Mantenham o diagrama simples e claro. O objetivo é que qualquer pessoa que olhe para o diagrama possa entender a parte do sistema que ele representa.

## Apresentação
- Após a criação dos diagramas, cada dupla apresentará o seu para a turma. Explique o que cada elemento representa e como ele ajuda a entender o sistema de reservas.

## Conclusão
- No final, discutiremos como os diferentes diagramas juntos oferecem uma visão completa do sistema e como isso pode ser aplicado em outros projetos de software.

