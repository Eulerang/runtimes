---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-27"

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Utilizza JRE Oracle
{: #using_oraacle_jre}

Puoi eseguire la tua applicazione Liberty su {{site.data.keyword.Bluemix}} con il JRE Oracle se lo desideri.  Per far ciò devi
* ospitare il JRE in un'ubicazione da cui il pacchetto di build può scaricarlo,
* ospitare un file `index.yml` che fornisce l'ubicazione del JRE ospitato e
* configurare la tua applicazione per utilizzare tale JRE.

## Ospitare JRE e index.yml
{: #hosting_jre}

Il file JRE Oracle deve essere ospitato su un server Web e il pacchetto di build Liberty deve essere in grado di scaricarlo da tale server. Puoi ospitarlo in {{site.data.keyword.Bluemix_notm}} stesso utilizzando una qualunque delle funzioni del server disponibili, oppure puoi ospitarlo in alcune ubicazioni disponibili pubblicamente.  Il server deve essere configurato con un file `index.yml` che specifica i dettagli sul file JRE. Completa i seguenti passi per ospitare JRE e il file `index.yml`:
  1. Acquisisci il JRE Oracle.  Tieni presente che JRE deve essere la versione per l'utilizzo su un SO a 64-Bit Unix e deve essere un file `tar.gz`.
  2. Ospita il file JRE dell'agent in un'ubicazione da cui il pacchetto di build Liberty può scaricarlo.
  3. Assicurati di fornire un file `index.yml` all'ubicazione che lo ospita. Il file `index.yml` deve contenere una voce composta dall'ID versione del JRE Oracle seguito da due punti e l'URL completo dell'ubicazione di tale file JRE. Il formato del `index.yml` è:
```
   ---
   jre_version: https://hostingLocation/jreName.tar.gz
```
{: codeblock}

    * Ad esempio:
    ```
       ---
       1.8.0_91: https://myHostingApp.ng.bluemix.net/jre-8u91-fcs-bin-b14-linux-x64-01_apr_2016.tar.gz
    ```

## Configura l'applicazione
{: #configure_app}

Devono essere configurati due ambienti nell'applicazione Liberty. La variabile *JBP_CONFIG_OPENJDK* deve essere configurata per specificare l'ubicazione del file `index.yml` e la variabile di ambiente *JVM* deve essere impostata su *openjdk* per configurare il pacchetto di build in modo che utilizzi un JRE alternativo.

Per la variabile JBP_CONFIG_OPENJDK il valore è
```
   'repository_root: "https://locationToIndexYml"'
```
{: codeblock}

Per configurarlo puoi emettere un comando come:
```
   ibmcloud cf se myApp JBP_CONFIG_OPENJDK 'repository_root: https://myHostingApp.ng.bluemix.net'
```
{: codeblock}

Tieni presente che l'URL *repository_root* non include `index.yml`, ma si arresta subito prima della sua ubicazione.

Per configurare la variabile di ambiente JVM emetti un comando come:
```
   ibmcloud cf se myApp JVM 'openjdk'
```
{: codeblock}

**Nota**: devi ripreparare la tua applicazione dopo la configurazione delle variabili di ambiente.

## Conferma
{: #confirmation}

Per confermare che sta venendo utilizzato il JRE previsto, controlla il log della fase di preparazione per un messaggio simile al seguente:
```
   -----> Downloading OpenJdk 1.8.0_91 from https://myHostingApp.ng.bluemix.net/jre-8u91-fcs-bin-b14-linux-x64-01_apr_2016.tar.gz (6.2s)
```
{: codeblock}

# rellinks
{: #rellinks notoc}
## general
{: #general notoc}
* [Runtime Liberty](index.html)
* [Panoramica di Liberty Profile](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/cwlp_about.html)
