Nível 3: A Abstração Computacional
Algoritmo que representa o processo de forma genérica e universal.


INICIO Algoritmo LoginATLAS

  exibir_tela_login()
  email <- obter_email()
  senha <- obter_senha()

  SE (validar_credenciais(email, senha) == VERDADEIRO) ENTAO
      redirecionar("dashboard")
  SENAO
      exibir_erro("Credenciais inválidas")
      voltar_para_login()
  FIM SE

FIM Algoritmo