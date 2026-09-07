<!-- fix-1 -->
# mypersonal — Mijn Zaken

Een persoonlijke kluis om vast te leggen wat iemand anders moet weten als jij het zelf niet meer kan vertellen: adressen, bankzaken, en losse belangrijke notities.

Toegang gaat in twee lagen: eerst moet je inloggen met een geautoriseerd account (Google of e-mail/wachtwoord), daarna ontgrendel je pas met het hoofdwachtwoord dat de inhoud versleutelt/ontsleutelt. Wie geen account heeft, komt niet verder dan het inlogscherm. Wie wél een account heeft maar het hoofdwachtwoord niet kent, ziet alleen onleesbare ruis.

## Eenmalige setup

1. Maak (of hergebruik) een Firebase-project op https://console.firebase.google.com.
2. **Authentication → Sign-in method** → schakel **Google** en **E-mail/wachtwoord** in.
3. **Authentication → Users → Add user** — maak hier een account aan voor jezelf en voor elke vertrouwde persoon die e-mail/wachtwoord gebruikt. (Iemand die met Google inlogt heeft geen vooraf aangemaakt account nodig — die krijgt vanzelf toegang zodra zijn/haar e-mailadres op de toegangslijst staat, zie stap 6.)
4. **Firestore Database** → maak een database aan (production mode).
5. Ga naar **Firestore → Rules** en vul de toegestane e-mailadressen in:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null &&
           request.auth.token.email in [
             "jouw@email.com"
             // , "vertrouwd.persoon@example.com"
           ];
       }
     }
   }
   ```
   Dit is de échte toegangscontrole — de lijst in `index.html` (volgende stap) is alleen voor een nette foutmelding in de app zelf.
6. Ga naar **Project settings → Algemeen → Jouw apps → Web app**, kopieer het `firebaseConfig`-object en plak dat in `index.html`, bovenaan het `<script>`-blok, in plaats van de `VUL_HIER_IN`-waarden. Vul in datzelfde blok ook `ALLOWED_EMAILS` met dezelfde adressen als in de Firestore rules.
7. Open `index.html` (via GitHub Pages of lokaal), log in, en kies bij de eerste keer ontgrendelen je hoofdwachtwoord.

## Belangrijk om te onthouden

- Er is geen "wachtwoord vergeten"-functie. Als het hoofdwachtwoord kwijt is, is de data niet meer te ontsleutelen.
- Wil je dat iemand anders hier ooit bij kan? Deel het hoofdwachtwoord apart en veilig (niet via deze repo of app zelf).
