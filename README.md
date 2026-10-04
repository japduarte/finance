# Finanças em ordem

PWA *mobile-first* para organizar as transferências mensais do João e da Paula.

As transferências são geradas a partir de rubricas configuráveis e agrupadas por pessoa, conta de origem e destino. Cada grupo permite consultar a decomposição das rubricas, marcar o movimento como efetuado e manter o histórico mensal.

Os dados vivem num ficheiro JSON: a app funciona localmente e pode sincronizar esse ficheiro com Google Drive. A futura área de depósitos e património usará o mesmo JSON.

## Executar localmente

```powershell
python -m http.server 4173
```

Abra `http://localhost:4173` no browser.

## Google Drive

Para sincronizar, é necessário criar um OAuth Client ID Google do tipo **Web application**. A primeira sincronização cria `financas-pwa.json` no Drive.

1. Aceda à [Google Cloud Console](https://console.cloud.google.com/), crie ou selecione um projeto.
2. Em **APIs e serviços → Biblioteca**, pesquise por **Google Drive API** e escolha **Ativar**.
3. Em **Google Auth platform**, complete a configuração inicial de marca e público. Sendo uma app pessoal em teste, adicione a sua conta Google (e a da Paula, se necessário) em **Test users**.
4. Abra **Google Auth platform → Clients → Create client**, escolha **Web application** e atribua-lhe um nome, por exemplo `Finanças PWA`.
5. Em **Authorized JavaScript origins**, adicione:
   - `http://localhost:4173` para o teste local;
   - o domínio final onde publicar a app, por exemplo `https://utilizador.github.io`. Não inclua o caminho do repositório.
6. Crie o cliente e copie apenas o **Client ID** (termina em `.apps.googleusercontent.com`). Não é necessário, nem seguro, colocar o *Client secret* na PWA.
7. Na PWA, abra **Dados → Configurar**, cole o Client ID e deixe o ID do ficheiro vazio. Ao usar o botão de sincronização, autorize a conta Google: a app cria o ficheiro `financas-pwa.json` no Drive.

Noutro telemóvel, configure o mesmo Client ID e cole o URL completo de `financas-pwa.json` (ou apenas o ID visível nesse URL) antes de escolher **Carregar do Drive**. O nome `financas-pwa.json`, por si só, não é um ID válido para a Drive API.

As origens JavaScript autorizadas identificam os domínios a partir dos quais a app pode pedir acesso ao Google. A Drive API tem de estar ativada no mesmo projeto Google Cloud. Consulte a documentação oficial sobre [clientes OAuth Web](https://developers.google.com/identity/sign-in/web/server-side-flow) e [ativação da Drive API](https://developers.google.com/workspace/drive/api/quickstart/js).

## Documentação

A especificação funcional e o modelo de dados estão em [ESPECIFICACAO_FUNCIONALIDADE_1.md](ESPECIFICACAO_FUNCIONALIDADE_1.md).
