# 🐾 PetOS — Caderneta digital de saúde animal
O PetOS reúne pets, registros de vacinação, rotinas de cuidado e alertas preventivos em uma caderneta digital. Tutores acompanham a saúde dos próprios animais; profissionais com perfil de clínica gerenciam os registros de vacinas.

## 🎯 Problema e solução

Informações de saúde dos pets muitas vezes ficam espalhadas em papéis, mensagens e anotações. Isso dificulta a consulta ao histórico e o acompanhamento de vacinas prestes a vencer ou já vencidas.

O PetOS oferece:

- Cadastro e consulta de pets por tutor.
- Registro, atualização e exclusão de vacinas pelo perfil **CLINICA**.
- Classificação preventiva das doses: pendente, próxima do vencimento, vencida ou aplicada.
- Geração e atualização de alertas **dentro da aplicação**, conforme o estado da vacinação.
- Envio automático de lembretes de vacinação por **e-mail (Gmail SMTP)** aos tutores cadastrados, quando habilitado.
- Registro de rotinas, como consultas veterinárias, e histórico consolidado por animal.
- Autenticação, autorização por perfil e isolamento dos dados entre tutores.

## 🧪 Testes

A qualidade do PetOS é validada em três frentes: testes de unidade e integração no código, testes manuais de sistema planejados no **Azure Boards** e testes automatizados de interface com **Selenium IDE**.

### Testes de unidade e integração (JUnit)

Execute a partir de `project/`:

```powershell
.\mvnw.cmd -B clean verify
```

A suíte inclui testes de regras de vacinação, persistência, autenticação, autorização, proteção CSRF, isolamento por tutor, fluxo de vacinas, histórico, tratamento de erros e páginas web.

### Testes manuais de sistema (Azure Boards)

🔗 **Plano de testes:** [dev.azure.com/RM555999/PetOS](https://dev.azure.com/RM555999/PetOS/_workitems/recentlyupdated/) — work items do tipo **Test Case** (#153 a #173).

Cada Test Case contém **objetivo**, **pré-condição**, **dados de entrada**, **dados de saída esperados** e o **procedimento em passos** (Action / Expected result). Todos estão ligados à Feature do backlog (*Parent*) e ao PBI que validam (*Tests*), na sprint em que a funcionalidade foi entregue. Os dados são controlados: as contas `tutor.teste@petos.com` e `clinica.teste@petos.com`, o pet **Bidu** e as vacinas V10, Antirrábica e Giárdia, com datas predefinidas.

| ID | Caso de teste | Feature | PBI validado |
| --- | --- | --- | --- |
| #153 | CT01 – Cadastrar tutor com dados válidos | #12 Cadastro e Autenticação | #36 Criar conta |
| #154 | CT02 – Cadastrar usuário com senha abaixo do mínimo | #12 | #36 |
| #155 | CT03 – Cadastrar usuário com e-mail já existente | #12 | #36 |
| #156 | CT04 – Login com credenciais válidas (tutor) | #12 | #37 Realizar login |
| #157 | CT05 – Login com senha incorreta | #12 | #37 |
| #158 | CT06 – Cadastrar e acessar o sistema como Clínica | #13 Controle de Acesso | #40 Acessar como Clínica |
| #159 | CT07 – Tutor tenta acessar funcionalidade exclusiva da clínica | #13 | #39 Acessar como Tutor |
| #160 | CT08 – Logout | #12 | #38 Realizar logout |
| #161 | CT09 – Cadastrar pet com dados válidos | #7 Cadastro e Perfil do Pet | #19 Cadastrar Pet |
| #162 | CT10 – Cadastrar pet com nome inválido | #7 | #19 |
| #163 | CT11 – Buscar pet por nome | #7 | #20 Consultar dados do Pet |
| #164 | CT12 – Atualizar dados do pet (API) | #7 | #21 Atualizar dados do Pet |
| #165 | CT13 – Clínica registra vacina aplicada | #9 Gestão de Vacinas | #26 Registrar vacina |
| #166 | CT14 – Vacina vencida gera alerta de atraso | #9 | #29 Identificar vacina atrasada |
| #167 | CT15 – Vacina próxima do vencimento gera alerta | #11 Alertas Preventivos | #33 Gerar alerta / #28 Próximas vacinas |
| #168 | CT16 – Vencimento anterior à data de aplicação | #9 | #26 |
| #169 | CT17 – Tutor visualiza vacinas e alertas pendentes | #11 | #34 Visualizar alertas pendentes |
| #170 | CT18 – Clínica registra aplicação de vacina pendente | #9 | #26 |
| #171 | CT19 – Clínica exclui registro de vacina | #9 | #26 |
| #172 | CT20 – Registrar cuidado de rotina | #10 Rotinas e Cuidados | #30 Registrar rotina / #24 Evento de saúde |
| #173 | CT21 – Filtrar histórico consolidado por categoria | #8 Histórico de Saúde | #23 Consultar histórico / #25 Linha do tempo |

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

## Link Repositório GitHub:
[petos-compliance-sprint4](https://github.com/nicholasbuzo/petos-compliance-sprint4/)

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
