# Quiz UniESBAM – versão GitHub Pages + Firebase

Projeto estático com **20 questões**, correção com **comentário para cada alternativa**, painel do professor ao vivo e exportação CSV para Excel.

## Publicação online

1. Crie um projeto em https://console.firebase.google.com/ e cadastre um **aplicativo Web** (ícone `</>`). Copie `apiKey`, `authDomain`, `projectId` e `appId` para `config.js`.
2. Em **Authentication > Sign-in method**, ative **Anonymous** (para os alunos) e **Email/Password** (para o professor). Crie a conta do professor em Authentication > Users e anote o **UID**.
3. Em **Firestore Database**, crie o banco. Cole o conteúdo de `firestore.rules` na aba **Rules**, substitua `SUBSTITUA_PELO_UID_DO_PROFESSOR` pelo UID do professor e **publique** as regras.
4. Preencha `professorEmail` e `professorUid` em `config.js`. O UID precisa ser exatamente o mesmo usado nas regras do Firestore.
5. Em **Authentication > Settings > Authorized domains**, inclua `SEU-USUARIO.github.io` (domínio real do seu GitHub Pages).
6. Envie **todos os arquivos** para a raiz de um repositório do GitHub e ative **Settings > Pages > Deploy from a branch > main / (root)**. Acesse `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.
7. Entre como professor, abra o quiz e acompanhe os alunos. Para testar, abra a URL em dois navegadores/dispositivos diferentes.

**IMPORTANTE:** GitHub Pages só hospeda os arquivos; para sincronização em tempo real entre dispositivos é necessário Firebase configurado. Se `config.js` estiver vazio, os botões de acesso ficam **bloqueados** para impedir o uso de uma versão local sem sincronização. A nota exibida no painel durante a prova é a nota parcial com base nas respostas já marcadas; após finalizar, é a nota final. A presença online significa atividade nos últimos 60 segundos e não comprova presença física ou conexão contínua.

## Segurança e privacidade

- Não coloque senha, token administrativo nem chave privada no repositório; `apiKey` do aplicativo Firebase Web é uma configuração pública.
- As regras do Firestore restringem a leitura global das tentativas ao UID do professor. Alunos só podem consultar e escrever a própria tentativa.
- Use dados acadêmicos apenas conforme as regras de privacidade da instituição. Oriente os alunos sobre identificação, armazenamento e acesso às notas.
- O frontend calcula a nota; **não é um sistema antifraude** e alunos com conhecimento técnico podem modificar o próprio navegador. Para uma avaliação oficial e resistente a manipulação, é necessária correção no servidor (Cloud Functions ou backend próprio), regras adicionais e autenticação institucional.
- Não há mais login de demonstração: o professor usa autenticação real do Firebase.
- Ao iniciar uma nova sessão pelo painel, o sistema apaga as tentativas existentes. Exporte primeiro as notas.

## Estrutura

- `index.html` — quiz, correção detalhada, painel ao vivo e exportação Excel/CSV.
- `config.js` — configuração pública do Firebase e identificação do professor.
- `firestore.rules` — permissões do banco (publicar manualmente no console Firebase).
- `README.md` — instruções de implantação.

## Diagnóstico da mensagem “outros dispositivos NÃO aparecem”

Ela era exibida porque o `config.js` original estava com valores vazios. Esta versão bloqueia o login até que os serviços online estejam configurados, em vez de fingir que os alunos estão sendo sincronizados.

**Checklist para testar:**

1. Abra o GitHub Pages e confira se o aviso de configuração pendente desapareceu.
2. No Firebase, confirme que **Authentication > Anonymous** e **Email/Password** estão habilitados.
3. Confira se o **Firestore Database** foi criado e se as regras foram publicadas com o UID exato do professor.
4. Confira se o domínio do GitHub Pages está autorizado na autenticação.
5. Entre como professor em um dispositivo e como aluno em outro, usando a **mesma URL publicada**.
6. O cadastro do aluno deve surgir imediatamente na lista do professor. Ao marcar cada resposta, a nota parcial e o progresso são atualizados.
7. Se surgir `Missing or insufficient permissions`, verifique o UID do professor, as regras e a identidade da conta autenticada.

**Não é possível distribuir um ZIP já conectado à sua conta Firebase sem receber os dados públicos do aplicativo Web e o UID do professor.** As credenciais devem ser fornecidas pelo proprietário do projeto; nunca inclua a senha do professor no código.
