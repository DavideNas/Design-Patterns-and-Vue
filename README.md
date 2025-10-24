# Design-Patterns-and-Vue

Nel mondo **Vue (v3+)**, i *design pattern* non sono identici a quelli dei linguaggi orientati agli oggetti (come Java o C#), ma esistono **pattern ricorrenti** che aiutano a strutturare, riutilizzare e mantenere facilmente il codice nelle applicazioni complesse.

Ecco la lista dei **design pattern più usati in Vue 3**, con una breve spiegazione di ciascuno:

---

### 🧱 **1. Container/Presentational Pattern**

* **Scopo**: Separare la logica di business dalla UI.
* **Descrizione**:

  * **Container Component**: gestisce stato, chiamate API, logica.
  * **Presentational Component**: mostra i dati, riceve solo `props`.
* **Alternativa moderna**: logica nei *composables* (`useSomething()`) e componenti “dumb” per la sola UI.

---

### 🧩 **2. Composables (Composition API Pattern)** ✅ **(modernissimo e centrale)**

* **Scopo**: Incapsulare e riutilizzare logica reattiva tra componenti.
* **Descrizione**: Funzioni che usano la Composition API (`ref`, `computed`, `watch`, ecc.) e ritornano dati/metodi condivisibili.
* **Esempio**:

  ```js
  // useFetch.js
  import { ref, onMounted } from 'vue'

  export function useFetch(url) {
    const data = ref(null)
    const error = ref(null)

    onMounted(async () => {
      try {
        const res = await fetch(url)
        data.value = await res.json()
      } catch (e) {
        error.value = e
      }
    })

    return { data, error }
  }
  ```

---

### 🧠 **3. Provide/Inject Pattern**

* **Scopo**: Condividere stato o funzioni tra componenti senza prop drilling.
* **Descrizione**: Un componente *parent* usa `provide()` per esporre valori, i discendenti li leggono con `inject()`.
* **Esempio tipico**: Temi, autenticazione, configurazioni globali.

---

### 📦 **4. Slot/Scoped Slot Pattern**

* **Scopo**: Passare contenuti dinamici e flessibili ai componenti figli.
* **Descrizione**:

  * **Slot normali** → sostituiscono contenuto.
  * **Scoped slots** → passano dati o metodi dal figlio al contenuto del genitore.
* **Esempio**:

  ```vue
  <List :items="users" v-slot="{ item }">
    <UserCard :user="item" />
  </List>
  ```

---

### 🧮 **5. State Management Pattern**

* **Scopo**: Gestire stato condiviso e complesso in modo scalabile.
* **Descrizione**:

  * Usa **Pinia** o **Vuex** come store globale.
  * Incapsula mutazioni, azioni e getter.
* **Alternativa leggera**: `reactive()` + `provide/inject`.

---

### 🧰 **6. Composable Factory Pattern**

* **Scopo**: Generare *composables* configurabili.
* **Esempio**:

  ```js
  export function createApiComposable(baseUrl) {
    return function useApi(endpoint) {
      const data = ref(null)
      async function fetchData() {
        const res = await fetch(`${baseUrl}${endpoint}`)
        data.value = await res.json()
      }
      return { data, fetchData }
    }
  }

  const useUserApi = createApiComposable('/api/users')
  ```

---

### 🧩 **7. Compound Components Pattern (via Slot + Provide/Inject)**

* **Scopo**: Costruire componenti flessibili e componibili.

* **Descrizione**: Un componente principale espone sottocomponenti interni che condividono lo stesso contesto.

* **Esempio**:

  ```vue
  <Tabs>
    <TabsList>
      <Tab>Uno</Tab>
      <Tab>Due</Tab>
    </TabsList>
    <TabPanels>
      <TabPanel>Contenuto 1</TabPanel>
      <TabPanel>Contenuto 2</TabPanel>
    </TabPanels>
  </Tabs>
  ```

* **Implementazione tipica**: `provide/inject` per condividere stato tra le parti.

---

### 🌐 **8. Global Provider Pattern**

* **Scopo**: Avvolgere l’app con fornitori di stato o servizi (tema, auth, store).
* **Esempio**:

  ```vue
  <ThemeProvider>
    <App />
  </ThemeProvider>
  ```

  Con `ThemeProvider` che usa `provide('theme', themeValue)`.

---

### ⚙️ **9. Controlled vs Uncontrolled Components**

* **Scopo**: Gestire input e stato dei form.
* **Controlled**: valore gestito da `v-model`.
* **Uncontrolled**: valore gestito nativamente (via `ref`), utile per performance o compatibilità con librerie esterne.

---

### 🔁 **10. Render Function / Strategy-like Pattern**

* **Scopo**: Passare comportamento come props o funzioni.
* **Descrizione**: Vue consente di passare funzioni personalizzate per gestire strategie o logiche diverse.
* **Esempio**:

  ```vue
  <Button :onClick="customLogic" />
  ```

  Oppure con `render()` function per massima flessibilità.

---

### 💡 **BONUS: Reactive Object Pattern**

* **Scopo**: Gestire stato complesso in modo modulare.
* **Descrizione**: Usa `reactive()` o `ref()` per creare oggetti reattivi e passarli ad altri componenti o composables.

---

### 🟢 In sintesi, i pattern più moderni e usati in Vue oggi sono:

* ✅ **Composables (Composition API Pattern)**
* ✅ **Provide/Inject Pattern**
* ✅ **Compound Components Pattern**
* ✅ **Composable Factory Pattern**
* ✅ **Global Provider / State Management Pattern**

---

👉 Spiegazione completa dei pattern disponibili al repo:
📘 [https://github.com/DavideNas/Design-Patterns-and-Angular](https://github.com/DavideNas/Design-Patterns-and-Angular) *(link originale di riferimento)*
