@startuml
left to right direction
skinparam packageStyle rectangle
skinparam usecase {
  BackgroundColor White
  BorderColor Black
  ArrowColor Black
}

actor "Membro" as M
actor "Administrador" as A

M --> (Consultar catálogo)
M --> (Devolver livro)
M --> (Registrar data e hora)
M --> (Requisitar empréstimo)
M --> (Realizar empréstimo)

A --> (Cadastrar novo livro)
A --> (Verificar popularidade dos livros)

(Realizar empréstimo) .u.> (Verificar popularidade dos livros) : <<extend>>
(Verificar popularidade dos livros) .d.> (Perguntar para clientes\nquais eles gostam mais) : <<extend>>
(Requisitar empréstimo) .d.> (Registrar data e hora) : <<extend>>

note bottom of (Requisitar empréstimo)
  • Registrar data e hora
  • Registrar data de devolução prevista
  • Sensor de controle
end note
@enduml
@startuml
actor Membro
participant ":TelaDeEmprestimo"
participant ":ControladorEmprestimo"
participant ":Livro"
participant ":Membro (ficha cadastral)"

Membro -> ":TelaDeEmprestimo" : Solicitar empréstimo
":TelaDeEmprestimo" -> ":ControladorEmprestimo" : Enviar requisição
":ControladorEmprestimo" -> ":Livro" : Verificar disponibilidade
":Livro" --> ":ControladorEmprestimo" : Disponível
":ControladorEmprestimo" -> ":Membro (ficha cadastral)" : Verificar pendências
":Membro (ficha cadastral)" --> ":ControladorEmprestimo" : Sem pendências
":ControladorEmprestimo" -> ":Livro" : Registrar empréstimo (status=emprestado)
":ControladorEmprestimo" -> ":TelaDeEmprestimo" : Confirmar empréstimo
":TelaDeEmprestimo" -> Membro : Mensagem de sucesso
@enduml
@startuml
class Membro {
  - idMembro : int
  - nome : string
  - email : string
  - telefone : string
  --
  + verificarPendencias() : boolean
  + listarEmprestimos() : List<Emprestimo>
}

class Livro {
  - idLivro : int
  - titulo : string
  - autor : string
  - status : string
  --
  + verificarDisponibilidade() : boolean
  + atualizarStatus() : void
}

class Emprestimo {
  - idEmprestimo : int
  - dataEmprestimo : Date
  - dataDevolucaoPrevista : Date
  - dataDevolucaoReal : Date
  --
  + registrarEmprestimo() : void
  + registrarDevolucao() : void
}

class Administrador {
  - idAdmin : int
  - nome : string
  --
  + cadastrarLivro() : void
  + gerenciarMembro() : void
  + verificarPopularidade() : void
}

Membro "1" --> "0..*" Emprestimo
Livro "1" --> "0..1" Emprestimo
Administrador "1" --> "0..*" Livro
Administrador "1" --> "0..*" Membro
@enduml
