usecaseDiagram
  actor Aluno as A
  actor Bibliotecário as B
  
  A --> (Pesquisar livros no acervo)
  A --> (Verificar disponibilidade do livro)
  A --> (Reservar livro)
  
  B --> (Gerenciar empréstimos)
  B --> (Registrar retirada de livro)
  B --> (Registrar devolução de livro)
  
  (Pesquisar livros no acervo) --> (Verificar disponibilidade do livro)
  (Registrar retirada de livro) --> (Reservar livro)
  (Registrar devolução de livro) --> (Pesquisar livros no acervo)
