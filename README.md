# Gerenciador de Contatos em Node.js

Este é um projeto simples desenvolvido para a disciplina de Arquitetura de Software (AS64B), ministrada pelo professor Diego Addan. O objetivo do projeto é criar um sistema básico de gerenciamento de contatos utilizando Node.js.

## Funcionalidades

O sistema oferece as seguintes funcionalidades:

- Adicionar contatos
- Remover contatos
- Listar contatos
- Buscar contatos por nome

## Padrões de Projeto Utilizados

#### Strategy: 
    A classe abstrata BuscaContatoStrategy define uma interface para estratégias de busca de contatos. As classes concretas BuscaContatoPorNomeStrategy e BuscaContatoPorEmailStrategy implementam essa interface, fornecendo diferentes maneiras de buscar contatos.
    
### Facade: 
    A classe GerenciadorContatosFacade encapsula a complexidade das classes GerenciadorContatos e BuscaContatoStrategy, fornecendo uma interface simples para adicionar, remover, listar e buscar contatos.

### Princípio da Responsabilidade Única (SRP): 
    Cada classe tem uma responsabilidade única. Por exemplo, a classe Contato é responsável apenas por manter as informações do contato. A classe GerenciadorContatos é responsável por gerenciar os contatos.

### Princípio Aberto-Fechado (OCP): 
    As classes BuscaContatoStrategy e BuscaContatoPorNomeStrategy exemplificam esse princípio. A classe base BuscaContatoStrategy é aberta para extensão (como demonstrado pela classe BuscaContatoPorNomeStrategy), mas fechada para modificação.

### Princípio da Substituição de Liskov (LSP): 
    A classe BuscaContatoPorNomeStrategy pode ser substituída pela classe base BuscaContatoStrategy sem afetar a corretude do programa.

### Princípio da Segregação da Interface (ISP): 
    Embora JavaScript não tenha interfaces no sentido tradicional, o código ainda segue este princípio. Cada classe tem uma "interface" clara em termos de métodos que podem ser chamados, e nenhuma classe é forçada a implementar métodos que não usa.

### Princípio da Inversão de Dependência (DIP): 
    A classe GerenciadorContatos depende de uma abstração (BuscaContatoStrategy) e não de uma implementação específica. Isso é demonstrado pela passagem de uma instância de BuscaContatoPorNomeStrategy para o construtor de GerenciadorContatos.

### Princio de Segregação de interfaces (ISP):
     classe GerenciadorContatos depende de uma abstração (BuscaContatoStrategy) e não de uma implementação específica. Isso é demonstrado pela passagem de uma instância de BuscaContatoPorNomeStrategy para o construtor de GerenciadorContatos.

### Casos de uso

![image](https://github.com/zVihugo/armazendo/assets/118476125/8811b381-6034-4887-b9d6-bfa65798e37c)

### Diagrama UML

' Módulo Readline para interação com o usuário
' Interface para estratégias de busca
class BuscaContatoStrategy {
    + buscar(contatos: List<Contato>, nome: String): List<Contato> # Método abstrato
}

' Estratégia de busca por nome
class BuscaContatoPorNomeStrategy extends BuscaContatoStrategy {
    + buscar(contatos: List<Contato>, nome: String): List<Contato>
        : Filtra contatos por nome
}

' Classe para gerenciar contatos
class GerenciadorContatos {
    - contatos: List<Contato>
    - buscaContatoStrategy: BuscaContatoStrategy

    + GerenciadorContatos(buscaContatoStrategy: BuscaContatoStrategy)
    + adicionarContato(nome: String, telefone: String, email: String): void
    + removerContato(nome: String): void
    + listarContatos(): void
    + buscarContato(nome: String): void
        : Delega a busca para a estratégia definida
}

' Interface para gerenciadores de contatos
' (Opcional para ISP completo)
interface GerenciadorContatosInterface {
    + adicionarContato(nome: String, telefone: String, email: String): void
    + removerContato(nome: String): void
    + listarContatos(): void
    + buscarContato(nome: String): void
}

' Classe para gerenciar vários gerenciadores de contatos
class GerenciadorContatosFacade {
    - gerenciadores: List<GerenciadorContatos>

    + GerenciadorContatosFacade()
    + adicionarGerenciador(gerenciador: GerenciadorContatos): void
    + adicionarContato(nome: String, telefone: String, email: String): void
        : Envia para todos os gerenciadores
    + removerContato(nome: String): void
        : Envia para todos os gerenciadores
    + listarContatos(): void
        : Envia para todos os gerenciadores
    + buscarContato(nome: String): void
        : Envia para todos os gerenciadores
}

' Classe de exemplo de gerenciador de contatos
class GerenciadorContatosSimples implements GerenciadorContatosInterface {
    - contatos: List<Contato>

    + adicionarContato(nome: String, telefone: String, email: String): void
    + removerContato(nome: String): void
    + listarContatos(): void
    + buscarContato(nome: String): void
}

' Relações de agregação e implementação
Contato <|-- o Contato
BuscaContatoStrategy o-- BuscaContatoPorNomeStrategy
GerenciadorContatos o-- GerenciadorContatosFacade
GerenciadorContatosFacade o-- GerenciadorContatos
GerenciadorContatos implements GerenciadorContatosInterface

' Direção do diagrama
right to left direction

' Criação e desativação de instâncias (opcional)
activate GerenciadorDeGerenciadores

create GerenciadorDeGerenciadores

deactivate GerenciadorDeGerenciadores

