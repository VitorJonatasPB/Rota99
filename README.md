# Projeto Rota99

**O que o sistema faz:**
O **Rota99** é um sistema que a gente criou para organizar e controlar as visitas que os consultores fazem nas escolas. Ele tem duas partes principais: um painel na web onde fica todo o cadastro (das escolas, professores, regiões, etc.) e um aplicativo de celular que o consultor usa na rua para registrar que chegou na escola e preencher uns relatórios.

---

## As Tecnologias (Nossa Stack)

Eu dividi certinho como a gente usou cada coisa no código:

### Back-end (A nossa API e o Painel Web)
- **Python e Django:** A gente fez o back-end inteiro em Python usando o Django (versão 6). Escolhemos o Django porque ele já vem com bastante coisa pronta.
- **Django REST Framework (DRF):** Usamos ele pra criar os endpoints da API que o aplicativo do celular chama pra pegar e salvar os dados. Pra parte de login, a gente colocou o **SimpleJWT**, que gera aquele token de acesso pra saber quem está logado no app.
- **Banco de Dados:** Aqui no dev, pra testar rápido, tá usando o **SQLite**. Mas deixamos tudo configurado direitinho com uma biblioteca chamada *psycopg* pra conectar e rodar no **PostgreSQL** quando for pra produção.
- **Painel Bonitão:** Sabe aquela tela padrão de admin do Django que é meio feia? A gente instalou uma biblioteca chamada *django-jazzmin* que deixou o painel com uma cara bem mais profissional e fácil para o pessoal usar.
- **Poetry:** Em vez de usar aquele bloco de notas `requirements.txt` pra instalar os pacotes, a gente usou o Poetry. Ajuda muito a gente não ter dor de cabeça com versão de biblioteca quebrando no computador de cada um.

### Front-end (O site principal / Páginas da Web)
- **Django Templates (HTML/CSS):** Pra fazer a interface das telas que abrem no navegador do computador (tipo os formulários pro pessoal da gestão web ou da escola acessar), a gente utilizou o sistema de *Templates* que já vem embutido no Django. A gente cria o HTML dentro da pasta `frontend/templates` e o próprio Django se vira pra enviar as páginas prontinhas com as informações do banco de dados. Como a ideia era entregar essa parte o mais rápido possível e sem dores de cabeça com configurações, a gente resolveu não usar nenhuma biblioteca JS muito complexa ou pesada (tipo React.js) lá na web; mantivemos no arroz com feijão bem feito do Django que resolve e não dá "tilt".

### App Mobile (Aplicativo do Consultor)
- **React Native com Expo:** O aplicativo que o consultor instala no celular foi feito com React Native, usando o Expo (versão 54). Assim, com o mesmo código, a gente consegue gerar o app tanto para Android quanto para iPhone.
- **TypeScript:** A gente escreveu o código em TypeScript em vez de JavaScript puro. Foi bom porque o editor avisa dos erros antes da gente rodar, evitando aqueles bugs chatos de variável escrita errada.
- **Requisições para a API:** Pra chamar a nossa API do Django lá no celular, usamos o `@tanstack/react-query`. Ele é muito bom porque faz o carregamento automático, guarda os dados em cache e deixa tudo bem rápido sem a tela ficar travando.

---

## O Que o App Consegue Fazer na Prática?

- **Ponto com GPS (Check-in e Check-out):** A gente usou o pacote *expo-location* do React Native. Quando o consultor toca no botão de "cheguei na escola", o app pega a localização (latitude e longitude) dele e manda pro back-end, só pra gente ter certeza de que ele estava mesmo lá no raio da escola.
- **Responder Questionários:** O pessoal pelo painel web consegue criar várias perguntas diferentes. O app puxa isso e monta a tela pro consultor responder: se tem arquivos em dia, colocar notas, marcar sim ou não, etc.
- **Assinatura na Tela:** Quando o consultor termina a visita, precisa da assinatura de um responsável lá da escola. A gente colocou um pacote chamado *react-native-signature-canvas* que basicamente deixa a tela do celular virar um caderninho pro cara assinar com o dedo. Depois a gente transforma isso numa string gigante (Base64) e salva no nosso servidor.
- **Modo Offline:** Essa foi a parte que deu mais trabalho, mas ficou legal. Se a escola não tiver internet, o app guarda as respostas do consultor na memória do próprio aparelho (usamos o *AsyncStorage* pra isso). Lá no back-end a gente tem uma flag avisando que foi feito offline, e quando a internet do celular volta, ele manda tudo certinho pra API.
- **Reportar o Bug:** Tem um botão no app que se der algum erro, o consultor clica, e a gente já manda tudo os dados do aparelho pra nós podermos ver o que deu errado e arrumar.

---

## Pré-requisitos

Para rodar este projeto na sua máquina, você vai precisar ter instalado:

- [Python](https://www.python.org/downloads/) (versão >= 3.14)
- [Poetry](https://python-poetry.org/docs/#installation) (para instalar e gerenciar as bibliotecas)
- **Git** (para clonar o repositório)

## Como Rodar o Projeto (Manual)

Siga o passo a passo padrão:

**1. Clone o repositório:**

```bash
git clone https://github.com/SEU-USUARIO/rota99.git
cd rota99
```

**2. Instale as dependências com o Poetry:**
Como o projeto usa o arquivo `pyproject.toml`, o Poetry vai gerenciar todas as versões pra você.

```bash
poetry install
```

**3. Ative o ambiente virtual (se necessário):**

```bash
poetry shell
```

**4. Rode as migrações do Banco de Dados:**

```bash
python manage.py migrate
```

**5. Inicie o Servidor:**

```bash
python manage.py runserver
```

Após isso, o sistema deve ficar acessível normalmente pelo seu navegador ou ferramentas como o Postman na porta padrão do Django (`http://127.0.0.1:8000`).

---

**Nota:** Por motivos de segurança, arquivos sensíveis (como `.env` e o banco de dados `db.sqlite3`) estão em nosso `.gitignore` e não devem ir para o GitHub. Certifique-se de configurar suas chaves locais para que o sistema funcione.
