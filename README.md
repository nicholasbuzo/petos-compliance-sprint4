# Testes automatizados – PetOS (Selenium IDE)

Testes automatizados com **Selenium IDE** no modelo Record & Playback para nosso sistema PetOS, com interface web.

## Casos de teste

| ID   | Caso                                  | Tipo                           | Resultado esperado                                                                                 |
| ---- | ------------------------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------- |
| CT01 | Cadastro de tutor com sucesso         | Positivo                       | Conta criada e redirecionamento para o login com a mensagem "Conta criada"                         |
| CT02 | Login com credenciais inválidas       | Negativo                       | Permanece no login com "E-mail ou senha inválidos"                                                 |
| CT03 | Tutor cadastra pet e abre a caderneta | Positivo                       | Mensagem "Caderneta criada", página de vacinas do pet e sem botão de registrar vacina para o tutor |
| CT04 | Tutor registra cuidado no histórico   | Positivo                       | Mensagem "Cuidado registrado" e item na linha do tempo                                             |
| CT05 | Clínica registra vacina aplicada      | Positivo (fluxo ponta a ponta) | Vacina "Antirrábica" listada com situação "Aplicada"                                               |
| CT06 | Busca por pet inexistente             | Negativo                       | Mensagem "Nenhum pet com esse nome"                                                                |
| CT07 | Tutor tenta registrar vacina pela URL | Segurança                      | Página de acesso negado ("Seu perfil não permite esta ação")                                       |

Os testes **Apoio - Criar tutor e entrar** e **Apoio - Tutor cadastra pet** são reutilizados pelo comando `run` e não fazem parte da suíte.
Cada execução gera e-mails e nomes de pet únicos (`Date.now()`), então a suíte pode rodar várias vezes sem limpar o banco.

## Como executar

1. Suba a aplicação (perfil `dev`, porta 8080) ou use a URL publicada no Azure.
2. Instale a extensão **Selenium IDE** no Chrome ou Firefox.
3. No Selenium IDE: **Open an existing project** → `testes/PetOS.side`.
4. Se for testar no Azure, troque a URL base no topo do Selenium IDE (campo "Playback base URL").
5. Selecione a suíte **Suite PetOS** e clique em **Run all tests in suite**.

Execução pela linha de comando (opcional):

```bash
npm install -g selenium-side-runner
selenium-side-runner testes/PetOS.side --base-url http://localhost:8080
```

## Link Azure DevOps:


## Link Repositório GitHub:
[(https://github.com/nicholasbuzo/petos-compliance-sprint4/edit/main/README.md)]

## Vídeo com demonstração da execução dos testes:
[![Apresentação PetOS - Testes Automatizados com Selenium IDE - 2TDSPO](https://img.youtube.com/vi/poKlc9I0kds/maxresdefault.jpg)](https://youtu.be/poKlc9I0kds)
## 👥 Equipe

**Turma:** 2TDSPO — FIAP

| Aluno | RM |
|---|---|
| Gustavo Gomes Martins | 555999 |
| Pedro dos Anjos | 563832 |
| Matheus de Mattos Vecchi | 561716 |
| Nicholas Albuquerque Buzo | 561082 |
| Nicholas Camillo Canadas de Paula | 561262 |
