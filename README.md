# ANGULAR TRAINING: CREAZIONE MENU DINAMICO

![Preview](menu-preview.gif "Preview")

# APIS:

* [API: /loginAsGuest, /loginAsAdmin, /menu](https://my-json-server.typicode.com/training-api/dynamic-menu)

* GET: allowed
* POST, PUT, PATCH DELETE: allowed (but fake 200 response. Changes are not persistent)

---


## ESERCIZIO

Crea un nuovo progetto Angular:

### Crea il componente

Crea un componente `<app-super-menu>` da utilizzare come illustrato di seguito:

```javascript
<app-super-menu [data]="menu" userRole="admin"></app-super-menu>
```

---

### Creazione menu dinamico

Creare il menu dinamicamente modificando il codice di `<app-super-menu>`.

Gestisci al massimo due livelli di menu.
Per comprendere la struttura dati ti consiglio di visualizzare la response del servizio REST [/menu](https://my-json-server.typicode.com/training-api/dynamic-menu/menu) e la preview della demo all'inizio di questo file.

Puoi usare Bootstrap con la seguente struttura HTML (che ovviamente dovrà diventare dinamica)

```html
<ul class="list-group list-group-flush">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">
    <ul class="list-group list-group-flush">
      <li class="list-group-item">Cras justo odio</li>
      <li class="list-group-item">Dapibus ac facilisis in</li>
      <li class="list-group-item">Morbi leo risus</li>
      <li class="list-group-item">Porta ac consectetur ac</li>
      <li class="list-group-item">Vestibulum at eros</li>
    </ul>
  </li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

---

### Styling

Applica ai menu-items il colore del font e del background che ricevi dal JSON

 
---

### Gestione Ruoli
Al momento, visto che ancora non hai realizzato il login (allo scopo di acquisire il ruolo dell'utente), puoi provare il componente passando delle stringhe alla proprietà: `admin` o `guest` in modo da poter verificare velocemente il funzionamento di entrambi gli scenari.

```javascript
<app-super-menu [data]="menu" userRole="admin"></app-super-menu>
<app-super-menu [data]="menu" userRole="admin"></app-super-menu>
```

Ruoli:

* `admin`: admin users, quando loggati, posso vedere tutto il contenuto, quindi tutti gli items (indipendentemente dal ruolo)
* `guest`: possono visualizzare gli item con `role` che ha valore `public` oppure `guest`
* `public`: se un item ha come ruolo `public` potrà essere visualizzato anche da utenti non loggati.


---

### Azioni
Quando l'utente clicca su un menu item il componente `<app-super-menu>` dovrebbe emettere un evento custom `itemClick` che conterrà, come parametro, le informazioni sull'azione: tipo di azione e contenuto azione.
 
In questo modo, quando se si utilizzerà questo componente in varie applicazioni, potrai decidere di gestire le azioni in modo differente.
Ad esempio, alla `action` di tipo `router` puoi reindirizzare l'utente alla route passata come parametro. Oppure, all'azione `openURL` potrai aprire un link esterno e così via.
 
```javascript
<app-super-menu 
  [data]="menu" 
  userRole="admin"
  (itemClick)="doSomething($event)"
></app-super-menu>
``` 

Nell'esempio di codice precedente, che illustra come dovrebbe essere utilizzato il componente, la funzione `doSomething` riceverà l'azione nel seguente formato:

```javascript
{
  action: string,
  params: string  
}
```

Ad esempio:

```javascript
{ action: 'openUrl', params: 'http://www.google.com' }
```

oppure:

```javascript
{ action: 'router', params: 'admin' }
```


Ai fini dell'esercizio è sufficiente ricevere le informazioni sull'azione ed effettuare un semplice console.log per verificare che arrivino tutte le informazioni per poterla gestire.
Se hai tempo (e voglia 😀 ) puoi comunque gestire il cambio di route oppure usare `window.open(url)` per aprire un link esterno al click di un item.


---

## SIMULARE LOGIN

Aggiungere due pulsanti per simulare il login come `guest` e come `admin` allo scopo di simulare un login (vedi preview del risultato finale all'inizio di questo documento).


Puoi usare due diverse API per simulare il login come 'guest' o 'admin':
 
* [https://my-json-server.typicode.com/training-api/dynamic-menu/loginAsAdmin](https://my-json-server.typicode.com/training-api/dynamic-menu/loginAsAdmin)
* [https://my-json-server.typicode.com/training-api/dynamic-menu/loginAsGuest](https://my-json-server.typicode.com/training-api/dynamic-menu/loginAsGuest)


> Non è necessario passare credenziali o gestire token. Sarà sufficiente invocare l'endpoint per il login e far in modo che il menu visualizzi i suoi menu-item sulla base del ruolo utente: public, guest or admin.

> Guarda il paragrafo "Gestione Ruoli" per una spiegazione su come dovrebbe funzionare la gestione dei ruoli.


A questo punto, mi aspetto che nel file `app.component` ci siano due pulsanti per effettuare il login come `guest` e `admin` e  un solo componente `app-super-menu`  che visualizzerà i contenuti corretti sulla base del tipo di utente.


```javascript
<button (click)="signInAs('Admin')">SignInAsAdmin</button>
<button (click)="signInAs('Guest')">SignInAsGuest</button>
<hr>
<app-super-menu [data]="menu" [userRole]="user?.role" (tabClick)="doSomething($event)"></app-super-menu>
<pre>{{user | json}}</pre>
```



* All'avvio dell'applicazione il menu visualizzerà solo gli item con `role` che hanno un valore `public` (saranno quindi nascosti gli item che richiedono ruolo `admin` o `guest`.
* Non appena si effettuerà il login come `admin` si vedranno tutti i menu item, indipentemente dal loro ruolo di accesso necessario per essere visualizzati.
* Se ci si logga come `guest` si vedranno solo gli item che hanno ruolo `public` o `guest` (non quelli admin)


