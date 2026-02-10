# ITTekniker
This is a dummy project. And none of the details in it are real, it is for demonstration and testing purposes only.
1. Ækvivalensklasser (Testteknik)

Ækvivalensklasser bruges til at gruppere input, der forventes at give samme resultat.

Eksempel: Login (password-længde)

Gyldig klasse: Password med 8–64 tegn

Ugyldig klasse 1: Password < 8 tegn

Ugyldig klasse 2: Password > 64 tegn

Ugyldig klasse 3: Tomt password

Formål: Reducere antallet af tests uden at miste dækning.

Security gate:

Code / Dev security gate (input validation)

2. Grænseværditest (Boundary Value Testing)

Tester grænser mellem gyldige og ugyldige input.

Eksempel: Password-længde

7 tegn Afvist (lige under)

8 tegn Accepteret (lige på)

9 tegn Accepteret (lige over)

64 tegn Accepteret

65 tegn Afvist

Security gate:

Code / Dev security gate

3. CRUD-test (Testteknik)

Tester håndtering af data i systemet.

Eksempel: Brugeradministration

Create: Opret ny bruger med korrekt validering

Read: Hent brugerdata (kun egne data for User-rolle)

Update: Opdater email/password

Delete: Slet bruger (kun Admin)

List: Admin kan se liste over alle brugere

Security fokus: Autorisation og least privilege.

Security gate:

Integration security gate

4. Cycle Process Test (Testteknik)

Tester gentagne processer over tid.

Eksempel: Login / Logout-cyklus

Gentag login til handling til logout 100 gange

Ingen session leaks

Tokens invalideres korrekt

Formål: Sikre stabilitet og korrekt session-håndtering.

Security gate:

System security gate

5. Decision Table Test (Testteknik)

Bruges når forretningslogik afhænger af flere betingelser.

Eksempel: Adgang til Admin-side
Rolle	Logget ind	Forventet adgang
Admin	Ja	        Tilladt
User	Ja	        Afvist
Ingen	Nej	        Afvist

Formål: Sikre korrekt autorisation i alle kombinationer.

Security gate:

Code / Dev security gate

6. Testpyramiden

Teststrategien følger testpyramiden:

Unit-tests: Input validation, decision tables (flest)

Integration tests: API + database + auth

System/E2E-tests: Brugerrejser (færrest)

Dette giver hurtig feedback og lav risiko.

Security gate:

Konsolidering med tooling / Quality gates