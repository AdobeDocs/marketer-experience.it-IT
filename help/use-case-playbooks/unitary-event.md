---
title: Evento unitario
description: Questa è una pagina di istruzioni per simulare la "[!UICONTROL Evento unitario]' tipo di convalida del Percorso.
source-git-commit: 0a52611530513fc036ef56999fde36a32ca0f482
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 0%

---


# Evento unitario

## Passaggi da seguire {#steps-to-follow}

>[!CONTEXTUALHELP]
>id="marketerexp_sampledata_unitaryevent"
>title="Come si usa?"
>abstract="Per maggiori informazioni, fate clic sul link"

>[!IMPORTANT]
>
>Queste istruzioni potrebbero cambiare **[!UICONTROL Playbook]** pertanto, consulta sempre la sezione dei dati di esempio **[!UICONTROL Playbook]**.

## Prerequisito

* Il software Postman deve essere installato
* Utilizza Playbook per creare le risorse delle istanze come **[!UICONTROL Percorso]**, **[!UICONTROL Schemi]**, **[!UICONTROL Segmenti]**, **[!UICONTROL Messaggi]** ecc.

Le risorse create verranno visualizzate il `Bill Of Material` Pagina

![Pagina Distinta Base](../assets/bom-page.png)

## Prepara Postman con la raccolta richiesta

1. Visita **[!UICONTROL Playbook del caso d’uso]** applicazione.
1. Fai clic sul rispettivo **[!UICONTROL Playbook]** scheda da visitare **[!UICONTROL Playbook]** pagina dei dettagli.
1. Visita **[!UICONTROL Distinta base]** e trovare il **[!UICONTROL Dati di esempio]** sezione.
1. Scarica il file `postman.json` facendo clic sui rispettivi pulsanti nell’interfaccia utente.
1. Importa `postman.json` nel **[!DNL Postman Software]**.
1. Creare un ambiente Postman dedicato per questa convalida (ad es. `Adobe <PLAYBOOK_NAME>`).

## Recuperare il token IMS

>[!NOTE]
>
>Tutte le variabili di ambiente fanno distinzione tra maiuscole e minuscole, quindi utilizza sempre il nome esatto della variabile.

1. Segui [Autenticazione e accesso alle API di Experienci Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html) per generare il token di accesso.
1. Memorizza il valore del token di accesso nelle variabili di ambiente denominate `ACCESS_TOKEN`.
1. Memorizza altri valori correlati all’autenticazione come `API_KEY`, `IMS_ORG` e `SANDBOX_NAME` nelle variabili di ambiente.

>[!IMPORTANT]
>
>Prima di eseguire qualsiasi API da Postman, assicurati che siano state aggiunte tutte le variabili di ambiente richieste.

## Pubblica il Percorso creato da Playbook

Esistono 2 modi per pubblicare il percorso; puoi sceglierne uno:

1. **Utilizzo dell’interfaccia utente AJO** - fare clic sul link del Percorso su `Bill Of Material Page`; questo ti reindirizzerà alla pagina del Percorso su cui puoi fare clic **[!UICONTROL Pubblica]** e il Percorso verranno pubblicati.

   ![Oggetto percorso](../assets/journey-object.png)

1. **Utilizzo dell’API Postman**

   1. Trigger **[!DNL Publish Journey]** richiesta da **[!DNL Journey Publish]** > **[!DNL Queue journey publish job]**.
   1. La pubblicazione del percorso potrebbe richiedere un po’ di tempo, quindi per controllare lo stato esegui l’API Verifica stato pubblicazione del Percorso, fino al `response.status` è `SUCCESS`, assicurati di attendere 10-15 secondi se la pubblicazione del percorso richiede tempo.

   >[!NOTE]
   >
   >Tutte le variabili di ambiente fanno distinzione tra maiuscole e minuscole, quindi utilizza sempre il nome esatto della variabile.

## Acquisire il profilo cliente

>[!TIP]
>
>Puoi riutilizzare lo stesso indirizzo e-mail aggiungendo `+<variable>` nell’e-mail, ad es. `usertest@email.com` può essere riutilizzato come `usertest+v1@email.com` o `usertest+24jul@email.com`. Questo sarebbe utile per avere ogni volta un nuovo profilo, ma utilizzando comunque lo stesso ID e-mail.

1. Il primo utente deve creare **[!DNL customer dataset]** e **[!DNL HTTP Streaming Inlet Connection]**.
1. Se hai già creato **[!DNL customer dataset]** e **[!DNL HTTP Streaming Inlet Connection]**, passare al passaggio `5`.
1. Trigger **[!DNL Customer Profile Ingestion]** > **[!DNL Create Customer Profile InletId]** > **[!DNL Create Dataset]** per creare **[!DNL customer dataset]**; questo conterrà un `CustomerProfile_dataset_id` nelle variabili di ambiente postman.
1. Crea **[!DNL HTTP Streaming Inlet Connection]**, utilizza le API di Postman in **[!DNL Customer Profile Ingestion > Create Customer Profile InletId]**.

   1. `CustomerProfile_dataset_id` deve essere disponibile nelle variabili di ambiente postman; in caso contrario, fare riferimento al passaggio `3`.
   1. Trigger **[!DNL `CREATE Base Connection`]** a [!DNL create base connection].
   1. Trigger **[!DNL `CREATE Source Connection`]** a [!DNL create source connection].
   1. Trigger **[!DNL `CREATE Target Connection`]** a [!DNL create target connection].
   1. Trigger **[!DNL `CREATE Dataflow`]** a [!DNL create dataflow].
   1. Trigger **[!DNL `GET Base Connection`]**- questa opzione consente di memorizzare automaticamente `CustomerProfile_inlet_id` nelle variabili di ambiente postman.

1. A questo punto è necessario disporre di `CustomerProfile_dataset_id` e `CustomerProfile_inlet_id` nelle variabili di ambiente postman; in caso contrario, fare riferimento al passaggio `3` o `4` rispettivamente.
1. Per acquisire il cliente, l’utente deve archiviare `customer_country_code`, `customer_mobile_no`, `customer_first_name`, `customer_last_name` e `email` nelle variabili di ambiente postman.

   1. `customer_country_code` potrebbe essere il codice del paese del numero di cellulare, ad esempio `91` o `1`
   1. `customer_mobile_no` potrebbe essere un numero di cellulare, ad es. `9987654321`
   1. `customer_first_name` sarà il nome dell&#39;utente
   1. `customer_last_name` sarebbe il cognome dell’utente
   1. `email` sarebbe l’indirizzo e-mail dell’utente; questo è fondamentale per utilizzare un id e-mail distinto, in modo che sia possibile acquisire un nuovo profilo.

1. Aggiornare la richiesta Postman **[!DNL Customer Ingestion]** > **[!DNL Customer Streaming Ingestion]** per cambiare il canale preferito del cliente; per impostazione predefinita [!DNL `email`] è configurato nella richiesta.

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

1. Cambia il canale preferito in `sms` o `push` e rendere il rispettivo valore di canale per `y` e `n` ad altri valori, ad es.

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

1. Trigger Finale **[!DNL `Customer Profile Ingestion > Customer Profile Streaming Ingestion`]** per acquisire il profilo cliente.

## Acquisisci evento

1. È la prima volta che un utente deve creare **[!DNL event dataset]** e **[!DNL HTTP Streaming Inlet Connection for events]**
1. Se hai già creato **[!DNL event dataset]** e **[!DNL HTTP Streaming Inlet Connection for events]**, passare al passaggio `5`.
1. Trigger **[!DNL `Schemas Data Ingestion > AEP Demo Schema Ingestion > Create AEP Demo Schema InletId > Create Dataset`]** per creare **[!DNL event dataset]**, in questo modo verrà memorizzato un `AEPDemoSchema_dataset_id` nelle variabili di ambiente postman
1. Crea **[!DNL HTTP Streaming Inlet Connection for events]**, utilizza le API di Postman in **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL Create AEP Demo Schema InletId]**.

   1. `AEPDemoSchema_dataset_id` deve essere disponibile nelle variabili di ambiente postman; in caso contrario, fare riferimento al passaggio `3`
   1. Trigger **[!DNL `CREATE Base Connection`]** a [!DNL create base connection]
   1. Trigger **[!DNL `CREATE Source Connection`]** a [!DNL create source connection]
   1. Trigger **[!DNL `CREATE Target Connection`]** a [!DNL create target connection]
   1. Trigger **[!DNL `CREATE Dataflow`]** a [!DNL create dataflow]
   1. Trigger **[!DNL `GET Base Connection`]**- questa opzione consente di memorizzare automaticamente `AEPDemoSchema_inlet_id` nelle variabili di ambiente postman

1. A questo punto è necessario disporre di `AEPDemoSchema_dataset_id` e `AEPDemoSchema_inlet_id` nelle variabili di ambiente postman, in caso contrario, fare riferimento al passaggio `3` o `4` rispettivamente
1. Per acquisire l’evento, l’utente deve modificare la variabile temporale `timestamp` nel corpo della richiesta di **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL AEP Demo Schema Streaming Ingestion]** in postino.

   1. `timestamp` se l’ora dell’occorrenza dell’evento è, utilizza la marca temporale corrente, ad esempio `2023-07-21T16:37:52+05:30` regola il fuso orario in base alle tue esigenze.

1. Trigger **[!DNL Schemas Data Ingestion > AEP Demo Schema Ingestion > AEP Demo Schema Streaming Ingestion]** per acquisire l’evento, in modo che possa essere attivato il percorso

## Convalida finale

Devi ricevere un messaggio sul canale preferito selezionato, utilizzato in **[!DNL Ingest the Customer Profile]** passaggio `8`

* `SMS` se il canale preferito è `sms` il `customer_country_code` e `customer_mobile_no`
* `Email` se il canale preferito è `email` il `email`

In alternativa, è possibile selezionare `Journey Report`, per verificarlo, fai clic su `Journey Object` il `Bill of Materials page` ti reindirizzerà a `Journey Details page`.

Per qualsiasi Percorso pubblicato, l’utente deve ottenere un **[!UICONTROL Visualizza rapporto]** pulsante
![Pagina report percorso](../assets/journey-report-page.png)


## Pulisci

Non avere più istanze di `Journey` in esecuzione simultaneamente, arrestare il Percorso se è solo per la convalida una volta completata la convalida.
