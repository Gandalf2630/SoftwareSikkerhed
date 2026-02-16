# ITTekniker

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



# Flat File DB (JSON) - Svær

## Hvad er det?
En lille database implementeret som en JSON-fil, der gemmer `person`-objekter med felter:
`{ person_id, first_name, last_name, address, street_number, password, enabled }`.

## Hvorfor er en flat_file_db smart?
- **Lav kompleksitet**: ingen DB-server, ingen migrations, nemt at komme i gang.
- **Let deployment**: én fil der kan flyttes/backup’es.
- **God til små projekter og prototyper**.
- **Ulemper**: dårligere performance på store datasæt, concurrency er sværere, ingen indekser.

## Implementerede funktioner
- `insert_person` (auto-id hvis mangler, validering, password hashing)
- `get_person_by_id`
- `find_persons` (filter på navn + enabled)
- `update_person` (patch update + hashing af password)
- `delete_person`
- `set_enabled`

## Test design teknikker (anvendt)
- **Equivalence Partitioning**: gyldige/ugyldige typer for `enabled`, `street_number`.
- **Boundary Value Analysis**: `street_number` > 0 (0 og -1 er ugyldige).
- **Negative testing**: duplicate `person_id`, update på ikke-eksisterende id, invalid patch.

## Unit tests (screenshots)
Indsæt screenshot her af `pytest -q` output.

## Gode testnavne (eksempler)
- `test_insert_person_shouldPersistPerson_whenValidPersonProvided`
- `test_insert_person_shouldThrow_whenPersonIdAlreadyExists`
- `test_update_person_shouldOnlyPatchProvidedFields_whenPatchProvided`

## Given/When/Then i test cases
Indsæt screenshot her af tests som viser `# Given`, `# When`, `# Then`.

## Risiko hvis tests ikke består (eksempler)
- Hvis duplicate-id testen fejler: **data corruption/overskrivning**.
- Hvis password-hash testen fejler: **password gemmes i plaintext (kritisk sårbarhed)**.
- Hvis patch-update testen fejler: **utilsigtet datatab**.
- Hvis delete testen fejler: **privacy/compliance og “ghost data”**.

## Sådan kører du det
```bash
pip install -r requirements.txt
pytest -q
