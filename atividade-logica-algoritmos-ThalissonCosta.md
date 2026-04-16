INICIO

lista_oficial <- ["Joao", "Maria", "Carlos", "Ana"]
fila <- ["Joao", "Pedro", "Ana", "Lucas"]

i <- 0

ENQUANTO i < 4 FACA

    aluno <- fila[i]
    encontrado <- falso
    j <- 0

    ENQUANTO j < 4 FACA
        SE aluno == lista_oficial[j] ENTAO
            encontrado <- verdadeiro
        FIMSE
        j <- j + 1
    FIMENQUANTO

    SE encontrado == verdadeiro ENTAO
        ESCREVA "Entrada permitida para ", aluno
    SENAO
        ESCREVA "Erro: ", aluno, " nao esta na lista"
    FIMSE

    i <- i + 1

FIMENQUANTO

FIM
