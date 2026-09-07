<!-- fix-0 -->
# mypersonal — Mijn Zaken

Een persoonlijke kluis om vast te leggen wat iemand anders moet weten als jij het zelf niet meer kan vertellen: adressen, bankzaken, en losse belangrijke notities.

Alles wordt **client-side versleuteld** (AES-GCM, sleutel afgeleid uit je hoofdwachtwoord) voordat het naar Firestore gaat. Er is geen inlogscherm — alleen het wachtwoord ontgrendelt de inhoud. Zonder dat wachtwoord is de data in Firestore onleesbare ruis.

## Eenmalige setup

1. Maak (of hergebruik) een Firebase-project op https://console.firebase.google.com.
2. **Authentication** → Sign-in method → schakel **Anonymous** in.
3. **Firestore Database** → maak een database aan (production mode).
4. Ga naar **Firestore → Rules** en gebruik:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
5. Ga naar **Project settings → Algemeen → Jouw apps → Web app**, kopieer het `firebaseConfig`-object.
6. Plak dat object in `index.html`, bovenaan het `<script>`-blok, in plaats van de `VUL_HIER_IN`-waarden.
7. Open `index.html` (via GitHub Pages of lokaal) en kies je hoofdwachtwoord — dit gebeurt automatisch bij de eerste keer ontgrendelen.

## Belangrijk om te onthouden

- Er is geen "wachtwoord vergeten"-functie. Als het hoofdwachtwoord kwijt is, is de data niet meer te ontsleutelen.
- Wil je dat iemand anders hier ooit bij kan? Deel het hoofdwachtwoord apart en veilig (niet via deze repo of app zelf).
