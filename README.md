# Quiz acadêmico — UniESBAM

Programação Web Front-End • Professor MSc. Ronilson Silva

Interface completa em `index.html`: 20 questões, identificação de alunos, correção, painel de progresso, edição de questões e exportação CSV para Excel. O Firebase sincroniza as participações entre dispositivos quando configurado.

## 1. Teste local

Abra `index.html` ou execute `python -m http.server 8000` nesta pasta e acesse http://localhost:8000. Professor de demonstração: `professor@demo.local`, senha `demo123`. Abra duas abas no mesmo navegador para testar a atualização local. Não há sincronização entre computadores neste modo.

## 2. Firebase (necessário para tempo real)

1. Acesse https://console.firebase.google.com/ e crie um projeto. Adicione um aplicativo Web.
2. Em Authentication, habilite Email/senha e Anônimo. Crie o usuário do professor com e-mail e senha próprios. Copie o UID exibido na lista de usuários.
3. Crie um banco Cloud Firestore em modo produção. Na aba Regras, cole o conteúdo de `firestore.rules`, substituindo `UID_DO_PROFESSOR` pelo UID real, e publique.
4. No `index.html`, procure `const CONFIG` e preencha apiKey, authDomain, projectId e appId com a configuração Web do projeto. Preencha `PROFESSOR_EMAIL` e `PROFESSOR_UID`. Não inclua a senha nem chaves de conta de serviço no código.
5. Em Authentication > Settings > Authorized domains, autorize o domínio do GitHub Pages (`SEU-USUARIO.github.io`) e localhost se necessário.
6. Entre primeiro como professor para criar a configuração inicial do quiz. Em seguida, compartilhe o endereço com os alunos.

## 3. GitHub Pages

Crie um repositório vazio no GitHub, por exemplo `quiz-academico`. Envie os arquivos desta pasta (index.html na raiz) pela interface Add file > Upload files ou pelo terminal:

```sh
git init
git add .
git commit -m "Projeto quiz academico"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/quiz-academico.git
git push -u origin main
```

Em Settings > Pages, selecione Deploy from a branch, branch main e pasta / (root), e salve. O endereço será `https://SEU-USUARIO.github.io/quiz-academico/`.

## 4. Teste de aceitação

Abra o endereço publicado em dois dispositivos. No primeiro, faça login como professor. No segundo, entre como aluno e responda uma questão. Confira o progresso no painel; finalize e confira a nota. Exporte CSV e abra no Excel (UTF-8, separador ponto e vírgula). Teste que um aluno não consegue abrir o painel ou ler documentos de outros alunos. A planilha XLSX incluída é um modelo separado, não é alimentada automaticamente.

## 5. Limitações e cuidados

Projeto para revisão, não prova oficial. O gabarito está acessível no código e no documento de configuração para alunos autenticados; a identidade do aluno é autodeclarada, podendo haver duplicatas. Não há validação de matrícula institucional nem controle de presença online; "Respondendo" indica uma tentativa não finalizada, não conexão ativa. O fechamento impede novas entradas, mas permite finalizar tentativas existentes; reiniciar a sessão invalida tentativas anteriores e apaga resultados. Exporte antes de reiniciar. Não altere questões durante uma sessão. Não publique CSV com dados pessoais no GitHub. O arquivo de regras precisa ser publicado no Firebase; hospedar no GitHub não aplica regras automaticamente. O modo demonstração usa armazenamento local, sem segurança real. O teste de integração depende de um projeto Firebase configurado pelo responsável; não foi executado neste pacote.
