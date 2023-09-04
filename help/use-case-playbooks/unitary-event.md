---
title: Evento unitario
description: Questa è una pagina di istruzioni per la simulazione del tipo di “[!UICONTROL evento unitario]” di convalida del percorso.
source-git-commit: 0a52611530513fc036ef56999fde36a32ca0f482
workflow-type: ht
source-wordcount: '749'
ht-degree: 100%

---


# Evento unitario

## Passaggi da seguire {#steps-to-follow}

>[!CONTEXTUALHELP]
>id="marketerexp_sampledata_unitaryevent"
>title="Modalità di utilizzo"
>abstract="Segui il collegamento per maggiori dettagli"

>[!IMPORTANT]
>
>Queste istruzioni potrebbero cambiare tra **[!UICONTROL playbook]** quindi fai sempre riferimento alla sezione dei dati di esempio del rispettivo **[!UICONTROL playbook]**.

## Prerequisito

* È necessario che sia installato il software Postman
* Utilizza il playbook per creare risorse di istanze come **[!UICONTROL Percorso]**, **[!UICONTROL Schemi]**, **[!UICONTROL Segmenti]**, **[!UICONTROL Messaggi]** e così via.

Le risorse create verranno visualizzate nella pagina `Bill Of Material`

![Pagina della Distinta base](../assets/bom-page.png)

## Prepara Postman con la raccolta richiesta

1. Visita l’applicazione per il **[!UICONTROL playbook sui casi d’uso]**.
1. Fai clic sulla rispettiva scheda del **[!UICONTROL Playbook]** per visitare la pagina dei dettagli del **[!UICONTROL Playbook]**.
1. Visita la pagina **[!UICONTROL Distinta base]** e trova la sezione relativa ai **[!UICONTROL dati di esempio]**.
1. Scarica il `postman.json` facendo clic sui rispettivi pulsanti dell’interfaccia utente.
1. Importa `postman.json` nel **[!DNL Postman Software]**.
1. Crea un ambiente Postman dedicato per questa convalida (ad es. `Adobe <PLAYBOOK_NAME>`).

## Recupera il token IMS

>[!NOTE]
>
>Tutte le variabili di ambiente fanno distinzione tra maiuscole e minuscole, quindi utilizza sempre il nome esatto della variabile.

1. Segui la documentazione dell’[autenticazione e accesso alle API di Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=it) per generare il token di accesso.
1. Memorizza il valore del token di accesso nelle variabili di ambiente denominate `ACCESS_TOKEN`.
1. Memorizza altri valori relativi all’autenticazione come `API_KEY`, `IMS_ORG` e `SANDBOX_NAME` nelle variabili di ambiente.

>[!IMPORTANT]
>
>Prima di eseguire qualsiasi API da Postman, assicurati che tutte le variabili di ambiente richieste siano state aggiunte.

## Pubblica il percorso creato dal playbook

Ci sono 2 modi per pubblicare il percorso; puoi sceglierne uno qualsiasi:

1. **Utilizzo dell’interfaccia utente AJO**: fai clic sul collegamento del percorso su `Bill Of Material Page`; questo ti reindirizzerà alla pagina del percorso in cui puoi fare clic sul pulsante **[!UICONTROL Pubblica]** per pubblicarlo.

   ![Oggetto del percorso](../assets/journey-object.png)

1. **Utilizzo dell’API Postman**

   1. Attiva la richiesta di **[!DNL Publish Journey]** da **[!DNL Journey Publish]** >**[!DNL Queue journey publish job]**.
   1. La pubblicazione del percorso potrebbe richiedere del tempo, quindi per verificare lo stato esegui l’API dello stato di pubblicazione del percorso, fino a che `response.status` è `SUCCESS`. Se la pubblicazione del percorso richiede del tempo, attendi 10-15 secondi.

   >[!NOTE]
   >
   >Tutte le variabili di ambiente fanno distinzione tra maiuscole e minuscole, quindi utilizza sempre il nome esatto della variabile.

## Acquisisci il profilo cliente

>[!TIP]
>
>Puoi riutilizzare lo stesso indirizzo e-mail aggiungendo `+<variable>` nella tua e-mail, ad esempio `usertest@email.com` può essere riutilizzato come `usertest+v1@email.com` o `usertest+24jul@email.com`. Questo sarebbe utile per avere ogni volta un nuovo profilo, ma utilizzando sempre lo stesso ID e-mail.

1. L’utente che lo utilizza per la prima volta deve creare il file **[!DNL customer dataset]** e **[!DNL HTTP Streaming Inlet Connection]**.
1. Se hai già creato il **[!DNL customer dataset]** e **[!DNL HTTP Streaming Inlet Connection]**, vai al passaggio `5`.
1. Attiva **[!DNL Customer Profile Ingestion]** > **[!DNL Create Customer Profile InletId]** > **[!DNL Create Dataset]** per creare **[!DNL customer dataset]**; questo memorizzerà un `CustomerProfile_dataset_id` nelle variabili d’ambiente Postman.
1. Crea **[!DNL HTTP Streaming Inlet Connection]**, utilizza le API Postman in **[!DNL Customer Profile Ingestion > Create Customer Profile InletId]**.

   1. `CustomerProfile_dataset_id` deve essere disponibile nelle variabili d’ambiente Postman, in caso contrario consulta il passaggio `3`.
   1. Attiva **[!DNL `CREATE Base Connection`]** su [!DNL create base connection].
   1. Attiva **[!DNL `CREATE Source Connection`]** su [!DNL create source connection].
   1. Attiva **[!DNL `CREATE Target Connection`]** su [!DNL create target connection].
   1. Attiva **[!DNL `CREATE Dataflow`]** su [!DNL create dataflow].
   1. Attiva **[!DNL `GET Base Connection`]**: questo memorizzerà automaticamente `CustomerProfile_inlet_id` nelle variabili d’ambiente Postman.

1. A questo punto, `CustomerProfile_dataset_id` e `CustomerProfile_inlet_id` saranno disponibili nelle variabili d’ambiente Postman; in caso contrario, consulta il passaggio `3` o `4` rispettivamente.
1. Per acquisire il cliente, l’utente deve archiviare `customer_country_code`, `customer_mobile_no`, `customer_first_name`, `customer_last_name` e `email` nelle variabili d’ambiente Postman.

   1. `customer_country_code` corrisponde al prefisso internazionale del numero di cellulare, ad esempio `91` o `1`
   1. `customer_mobile_no` corrisponde al numero di cellulare, ad esempio`9987654321`
   1. `customer_first_name` corrisponde al nome dell’utente
   1. `customer_last_name` corrisponde al cognome dell’utente
   1. `email` corrisponde all’indirizzo e-mail dell’utente, che è fondamentale per utilizzare un ID e-mail distinto in modo che possa essere importato un nuovo profilo.

1. Aggiorna la richiesta di Postman **[!DNL Customer Ingestion]** > **[!DNL Customer Streaming Ingestion]** per cambiare il canale preferito del cliente; per impostazione predefinita, nella richiesta è configurato [!DNL `email`].

   ```js
   "consents": {
       "marketing": {
           "preferred": "email",
           "email": {
               "val": "y"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "n"
           }
       }
   }
   ```

1. Cambia il canale preferito in `sms` o `push` e imposta il rispettivo valore del canale su `y` e `n` ad altri valori, ad esempio

   ```js
   "consents": {
       "marketing": {
           "preferred": "sms",
           "email": {
               "val": "n"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "y"
           }
       }
   }
   ```

1. Infine Attiva **[!DNL `Customer Profile Ingestion > Customer Profile Streaming Ingestion`]** per importare il profilo cliente.

## Acquisisci evento

1. L’utente che lo utilizza per la prima volta deve creare **[!DNL event dataset]** e **[!DNL HTTP Streaming Inlet Connection for events]**
1. Se hai già creato **[!DNL event dataset]** e **[!DNL HTTP Streaming Inlet Connection for events]**, vai al passaggio `5`.
1. Attiva **[!DNL `Schemas Data Ingestion > AEP Demo Schema Ingestion > Create AEP Demo Schema InletId > Create Dataset`]** per creare **[!DNL event dataset]**, questo memorizzerà `AEPDemoSchema_dataset_id` nelle variabili d’ambiente Postman
1. Crea **[!DNL HTTP Streaming Inlet Connection for events]**, utilizza le API Postman in **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL Create AEP Demo Schema InletId]**.

   1. `AEPDemoSchema_dataset_id` deve essere disponibile nelle variabili d’ambiente Postman, in caso contrario, consulta il passaggio `3`
   1. Attiva **[!DNL `CREATE Base Connection`]** su [!DNL create base connection]
   1. Attiva **[!DNL `CREATE Source Connection`]** su [!DNL create source connection]
   1. Attiva **[!DNL `CREATE Target Connection`]** su [!DNL create target connection]
   1. Attiva **[!DNL `CREATE Dataflow`]** su [!DNL create dataflow]
   1. Attiva **[!DNL `GET Base Connection`]**: questo memorizzerà automaticamente `AEPDemoSchema_inlet_id` nelle variabili d’ambiente Postman

1. A questo punto, `AEPDemoSchema_dataset_id` e `AEPDemoSchema_inlet_id` saranno disponibili nelle variabili d’ambiente Postman, in caso contrario, consulta il passaggio `3` o `4` rispettivamente
1. Per acquisire un evento, l’utente deve modificare la variabile temporale `timestamp` nel corpo della richiesta di **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL AEP Demo Schema Streaming Ingestion]** in Postman.

   1. `timestamp` corrisponde all’ora in cui si è verificato l’evento; utilizza il timestamp corrente, ad esempio `2023-07-21T16:37:52+05:30` regola il fuso orario secondo le tue necessità.

1. Attiva **[!DNL Schemas Data Ingestion > AEP Demo Schema Ingestion > AEP Demo Schema Streaming Ingestion]** per acquisire l’evento, in modo che il percorso possa essere attivato

## Convalida finale

Devi ricevere un messaggio sul canale preferito selezionato utilizzato nel passaggio **[!DNL Ingest the Customer Profile]** `8`

* `SMS` se il canale preferito è `sms` su `customer_country_code` e `customer_mobile_no`
* `Email` se il canale preferito è `email` su `email`

In alternativa, puoi verificare il `Journey Report`. Per farlo, fai clic su `Journey Object` su `Bill of Materials page` e verrai reindirizzato a `Journey Details page`.

Per ogni percorso pubblicato l’utente deve ottenere un pulsante **[!UICONTROL Visualizza rapporto]**
![Pagina del rapporto del percorso](../assets/journey-report-page.png)


## Pulizia

Non avere più istanze di `Journey` in esecuzione contemporaneamente. Una volta completata la convalida, interrompi il percorso se è solo per la convalida.
