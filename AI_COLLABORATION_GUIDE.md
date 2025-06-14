# Fleetopia.co - Ghid de Colaborare pentru Asistenți AI

**Ultima actualizare:** 14 Iunie 2024

Acest document descrie starea actuală a proiectului Fleetopia.co și protocolul de colaborare stabilit.

---

## 1. Prezentare Generală a Proiectului

**Fleetopia.co** este o platformă logistică avansată, dezvoltată în Next.js, care utilizează concepte de inteligență artificială pentru a eficientiza managementul flotelor de transport.

### Componente Principale:

1.  **AI Dispatcher (`/ai-agents`):** Un centru de comandă centralizat care monitorizează și gestionează operațiunile. Acesta este "creierul" sistemului.
2.  **Logistics Marketplace (`/marketplace`):** O piață digitală unde utilizatorii pot posta oferte de marfă (cerere) și pot căuta transport (ofertă).
3.  **Real-time & Analytics (`/real-time`):** Un panou de bord pentru monitorizarea live a flotei (hărți, alerte) și pentru analiza performanței.

---

## 2. Starea Funcțională Actuală

Platforma a depășit stadiul de prototip static și are un prim flux funcțional complet implementat:

*   **Publicare Ofertă:** Utilizatorii pot completa și trimite formularul din `Marketplace -> Post Cargo`.
*   **Salvare în Baza de Date:** Oferta este salvată în baza de date PostgreSQL prin intermediul unui API (`/api/marketplace/cargo`).
*   **Afișare Dinamică:** Lista de oferte din `Marketplace -> Find Cargo` se încarcă direct din baza de date, afișând în timp real toate ofertele active.
*   **Ștergere Securizată:** Fiecare ofertă are un buton de ștergere care deschide un dialog de confirmare. La confirmare, oferta este eliminată definitiv din baza de date printr-un API dedicat (`/api/marketplace/cargo/[id]`).
*   **Notificarea Dispecerului:** La publicarea unei noi oferte, se creează automat o `SystemAlert` în baza de date.
*   **Recepția Alertei:** Pagina `AI Dispatcher` interoghează periodic un API (`/api/dispatcher/alerts`) și afișează noile alerte, închizând astfel bucla funcțională între Marketplace și centrul de comandă.
*   **Editare Oferte:** A fost implementată funcționalitatea de editare a ofertelor, incluzând un API `PUT` și un dialog de modificare care se pre-completează cu datele existente.
*   **Curățare și Refactorizare:** Pagina `Real-time Analytics` a fost curățată de cod redundant și datele simulate au fost traduse pentru consistență.

---

## 3. Tehnologii Utilizate

*   **Framework:** Next.js (App Router)
*   **Limbaj:** TypeScript
*   **Baza de Date:** PostgreSQL
*   **ORM:** Prisma
*   **Stilizare:** Tailwind CSS
*   **Componente UI:** shadcn/ui
*   **Animații:** Framer Motion

---

## 4. Protocol de Colaborare (Workflow)

Orice modificare adusă proiectului trebuie să urmeze acest protocol strict:

1.  **Propunerea:** Asistentul AI analizează starea curentă și propune următorul pas logic de dezvoltare.
2.  **Confirmarea:** Utilizatorul (tu) analizează propunerea și își dă acordul explicit pentru a începe.
3.  **Implementarea:** Asistentul AI execută modificările tehnice necesare pentru a implementa pasul aprobat.
4.  **Salvarea Locală:** Imediat după implementare, progresul este salvat **doar local** folosind următoarele comenzi Git:
    *   `git add .`
    *   `git commit -m "feat: [descriere clară a modificării]"`
5.  **Verificarea:** Utilizatorul verifică local modificările pentru a se asigura că totul funcționează corect și nu există erori.
6.  **Sincronizarea (Push):** Doar la comanda **explicită** a utilizatorului, se va rula comanda `git push` pentru a urca modificările pe GitHub.
7.  **Iterația:** Ciclul se reia cu propunerea următorului pas.

### Regula de Aur:

**Nu se modifică codul existent, ci se extinde.** Se adaugă funcționalități noi fără a altera fundamental logica deja implementată și confirmată. Obiectivul este să construim peste fundația existentă, nu să o reconstruim.

Acest proces asigură o dezvoltare incrementală, sigură și foarte bine documentată.

---

## 5. Depanare Majoră și Stabilizare (14 Iunie 2024)

Înainte de a continua dezvoltarea, am trecut printr-o etapă critică de depanare și stabilizare.

1.  **Context:** După implementarea hărții interactive pe pagina `Real-time & Analytics`, aplicația a început să genereze erori de conectivitate la baza de date și erori de "hidratare" a componentelor pe client.
2.  **Cauza Rădăcină:** Problema a fost identificată ca fiind o gestionare ineficientă a clientului Prisma. Se crea o nouă instanță `PrismaClient` pentru fiecare request API, epuizând pool-ul de conexiuni la baza de date și corupând starea clientului.
3.  **Soluția:**
    *   Am refactorizat codul pentru a utiliza un **singleton PrismaClient** (`lib/prisma.ts`), asigurând o singură instanță partajată în întreaga aplicație.
    *   Am refactorizat componenta hărții (`RealTimeVehicleMap`) pentru a utiliza corect hook-urile `useRef` și `useEffect`, eliminând eroarea `Map container is already initialized`.
    *   Deoarece problemele persistau, am efectuat o resetare completă: am oprit serverul, am șters `node_modules`, am reinstalat pachetele și am regenerat clientul Prisma. Această acțiune a curățat orice stare coruptă și a rezolvat definitiv erorile.

**Status Curent:** Aplicația este acum stabilă, performantă și complet funcțională pe toate modulele implementate (Marketplace, AI Dispatcher, Real-time).

---

## 6. Următorii Pași: Funcționalități Noi

Cu platforma stabilizată, începem implementarea a două noi funcționalități:

1.  **Calculator de Rută:** Un panou pe pagina `Real-time` unde utilizatorul poate introduce adrese pentru a calcula și vizualiza rute pe hartă.
2.  **Simulator de Vehicule:** O pagină dedicată pentru a modifica manual starea vehiculelor (locație, status) și a testa reactivitatea sistemului în timp real.

---

## 7. Istoric Modificări (tasks)

*   **Task 1:** Descrierea task-ului 1
*   **Task 2:** Descrierea task-ului 2
*   **Task 3:** Descrierea task-ului 3 