# AOC Hub de carpetes ciutadanes

Projecte de documentació per als integradors del Hub de Carpetes Ciutadanes (HCC) del Consorci AOC. El HCC consultarà a cada ens integrat informació sobre actuacions (accions de relació entre la ciutadania i l'Administració entesos des d'un punt de vista ampli i tipificats per entrada o sortida) i expedients que hauran d'exposar a través d'una interfície REST que compleixi el model i les consultes descrites a continuació.

Com a primers passos, abans de les tasques d'integració, es recomana la lectura del [document de presentació](/presentacio_myg_20201105.pdf).

## Integració
Per integrar-se al servei, els passos a fer són els següents:

### A. Preproducció
1. Dur a terme el desenvolupament explicat en aquest repositori documental.
2. Omplir el formulari de contacte del portal de Suport: https://suport-integradors.aoc.cat/hc/ca/requests/new indicant a l'assumpte "Integració en proves de El meu espai - Hub de Carpetes Ciutadanes". Afegint al mateix la **URL** de l'endpoints amb exemples de les consultes.
3. Un cop validades a preproducció les funcionalitats dels endpoints per part de l'AOC, fixar la data de sortida a producció i seguir els passos de B) Producció.

El HCC revisara la integració a l'entorn de proves (preproducció) per poder per validar la correcta integració al servei. 
Per fer correctament les comprovacions dels vostres endpoints a preproducció, podeu fer servir HTTPS/TLS però **es recomana no afegir cap filtratge d'IP**, ni autenticar el certificat client durant aquesta fase. Si tot i així és necessari filtrar per IP, el HCC arribarà a l'entorn de proves amb alguna d'aquestes IP `63.34.38.187`, `34.251.91.75`, `34.254.144.227`

Per a les proves, si us plau **no** fer servir NIFs de persones reals. En concret, cal fer servir:
|       NIF      | Nom o Raó social | Cognom1 | Cognom2 | Casos d'ús |  
| ---------------| --- | --- | --- | --- |
| 00000000T | Ciutadania | Fictícia | PF | Persona física |
| 00000000T | Ciutadania | Fictícia | PFRPJ | Persona física representant de persona jurídica |
| Q0000000J | PJurídica Ficticia PJ |  |  | Persona jurídica representada per 99999974E |

Per a validar els endpoints a preproducció cal:

**consultaActuacions**
- 3 o més actuacions del NIF 00000000T.
- 3 o més actuacions del NIF 00000000T (representant de la persona jurídica Q0000000J).

**consultaExpedients**
- 2 o més expedients del NIF 00000000T.
- 2 o més expedients del NIF 00000000T (representant de la persona jurídica Q0000000J).

Per assegurar la qualitat de les proves a preproducció, és necessari que tant els assentaments registrals o actuacions com els expedients siguin els més reals possibles, però sense contenir dades personals.

Si us plau, per a les proves cal fer servir les dades reals identificatives de les entitats. **Les dades de la respostes a preproducció han de ser remeses com una de les entitats reals en proves que esteu integrant**. Per tant, ens calen dades consistents entre NIF, DIR3 i INE10. Si us és impossible respondre segons aquest criteri, contacteu amb nosaltres per pautar-vos un conjunt de dades de proves.

Un cop l'AOC tingui la informació necessària per consultar les dades, validarà que la integració funcioni correctament a preproducció.

### B. Producció
Cal presentar un nou formulari de contacte del portal de Suport: [https://suport-integradors.aoc.cat/hc/ca/requests/new](https://suport-integradors.aoc.cat/hc/ca/requests/new) annexant [el document de sol·licitud d’integració](https://github.com/ConsorciAOC/Integracio-Serveis/raw/main/documentAlta/formulari_sol-licitud_integracio_serveis_caoc.pdf) signat. Indicant a l'assumpte "Integració en producció de El meu espai - Hub de Carpetes Ciutadanes" i determinant al cos del formulari de suport les URLS definitives de les consultes (PRE i PRO o, com a mínim, PRO).

Els endpoints de les consultes han d'estar publicats a la mateixa url base, per exemple:
- URL de consulta d'actuacions > https://serveis.ajuntament.cat/hub/consultaActuacions
- URL de consulta d'expedients > https://serveis.ajuntament.cat/hub/consultaExpedients

Per a la sortida a producció:
- Cal securitzar els endpoints exposats a través de comunicacions **HTTPS/TLS**. És important que el domini del certificat servidor correpongui al domini exposat al HCC.
- Si es vol **filtrar les peticions del HCC per IP** aquestes arribaran a través d'una d'aquestes ips: `157.97.64.126`, `157.97.65.88`, `18.200.107.87`, `52.19.83.31`, `99.81.75.214`
- És recomenabe també establir un canal TLS **validant el certificat client** que presentarà el HCC (Serveis_Administracio_Electronica_AOC.cer)

Un cop l'AOC tingui la informació necessària per consultar les dades, validarà que la integració funcioni correctament.

El model de desenvolupament es concretarà en fases i podria variar durant el temps:

1. Primera fase de desenvolupament (Prioritzada)
    1. [Consulta d'actuacions](#1-consulta-dactuacions) (Prioritat alta)
    2. [Consulta d'expedients](#2-consulta-dexpedients) (Prioritat mitja)
    3. [Consulta de cites prèvies](#2-consulta-cites-previes) (Prioritat mitja)

2. Segona fase de desenvolupament (No prioritzada)
    1. Consulta agrupada
    2. Consulta detallada d'una actuació
    3. Consulta detallada d'un expedient

    Aquesta segona fase està pendent de ser concretada i es troba en estat d'esborrany.


_Aquest document està en fase d'esborrany i pot patir canvis._

## Model

A continuació, es descriuen els atributs i l'estructura JSON dels objectes d'intercanvi d'informació entre el HCC i els ens integrats. Podeu trobar la informació en detall a cadascuna de les consultes.

Al construir les respostes JSON, cal no informar aquells camps opcionals on no es retorni cap valor o valor buit.

Incorrecte (s'informa el camp _observacionsActuacio_ com una cadena buida de caràcters):
```json
"actuacions": [
    {
        "idActuacio": "1",
        "tipusPersona": "INTERESSAT",       
        "observacionsActuacio": ""        
        ...
    }
```

Correcte:
```json
"actuacions": [
    {
        "idActuacio": "1",
        "tipusPersona": "INTERESSAT"       
    }
```
    
### Actuacio

#### Exemple JSON

El model s'ha dividit en blocs d'informació integrats en la resposta: Identificador de l'objecte, de la persona, de l'ens, de l'actuació i de l'expedient.

```json  
{
  "idActuacio":"2015-E-126",
  "tipusPersona":"INTERESSAT",
  "codiINE10":"9821920002",
  "codiDIR3Organisme":"L01080193",
  "tipusActuacio":"SORTIDA",
  "dataActuacio":"2015-02-03T12:01:05",
  "numeroRegistre":"2015-E-126",
  "assumpteActuacio":"Duplicat padró",
  "urlActuacio":"http://www.abc.cat/a/id_2015_126",
  "viaPresentacio":"Electrònica",
  "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
  "observacionsActuacio":"Duplicat padró",
  "idExpedient":"EXP-ID-1",
  "fue":"false",
  "estatNotificacio":"DIPOSITADA"
}
```  

#### Descripcio camps  

També trobareu la totalitat de paràmetres previstos actualment en el [model de dades de la resposta](/model_dades_mygov.xlsx).
Els paràmetres segueixen una estructura determinada pel nom del mateix en minúscules i, opcionalment, l'àmbit: "parametre" i "parametreAmbit".

|       Paràmetre      | Descripció | Obligatori |  
| ----------------------| --- | --- |  
| idActuacio          | Identificador de l'actuació | SI |  
| tipusPersona           | Tipus de persona | SI (INTERESSAT, REPRESENTANT) |  
| codiINE10              | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub de Carpetes ciutadanes de MyGov | SI |  
| tipusActuacio          | Tipus d'actuació | SI (ENTRADA, SORTIDA)|  
| dataActuacio           | Data de l'actuació. Format ISO_8601 YYYY-MM-DDThh:mm:ss | SI |  
| assumpteActuacio               | Assumpte | SI |  
| urlActuacio            | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'actuació farà clic en aquest enllaç | SI |  
| codiDIR3Organisme      | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub de Carpetes ciutadanes de MyGov | NO |
| numeroRegistre           | Número de registre en format string | NO |  
|dataRegistre | Data del registre en cas de ser diferent de la dataActuació, per acotar els terminis. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |
| viaPresentacio         | Via de presentació | NO |  
| canalPreferentNotificacio         | Canal preferent | NO (DIRECCIO_POSTAL, COMPAREIXENCA_ELECTRONICA, DIRECCIO_ELECTRONICA_HABILITADA) |  
| observacionsActuacio   | Observacions sobre l'expedient que l'Ens vulgui informar | NO |  
| idExpedient        | Número de l'expedient | NO |  
| urlExpedient            | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | NO |
| procediment            | Procediment | NO |  
| referenciaExterna      | Referencia externa | NO |
| fue | Paràmetre que indica si es tracta d'un expedient FUE. | NO (defecte false, true)
| estatNotificacio | Estat de l'actuació, determinada pel llistat. Dipositada és l'estat quan un registre de sortida ha estat disposat a la persona destinatària. Acceptada és quan aquesta ha estat practicada mitjaçant la seva acceptació. Rebutjada és quan ha estat practicada per haver-se rebutjat o bé ha expirat el termini. Aplica parcial o totalment a les comunicacions i les notificacions, segons cada cas.  | NO (DIPOSITADA, ACCEPTADA, REBUTJADA)

### Expedient  

#### Exemple JSON

El model s'ha dividit en blocs d'informació integrats en la resposta: Identificador de l'objecte, de la persona, de l'ens, de l'actuació i de l'expedient.

```json  
{
   "idExpedient":"EXP-2021-1",
   "tipusPersona":"INTERESSAT",
   "codiINE10":"801930008",
   "codiDIR3Organisme":"L01080193",
   "procediment":"OBRES1 - llicència d'obres",
   "dataIniciExpedient":"2021-07-25T01:25:43",
   "assumpteExpedient":"llicència d'obres",
   "urlExpedient":"http://www.abc.cat/e/2021_225",
   "familia":"Obres",
   "dataPrevistaResolucio":"2021-09-25",
   "codiFase":"02",
   "descripcioFaseExpedient":"En tramitació",
   "estatExpedient":"OBERT",
   "observacionsExpedient":"llicència - Reforma planta baixa",
   "idActuacio":"2021-E-225",
   "dataActuacio":"2021-05-23T18:25:43",
   "actuacioCiutada":"NO_APLICA",
   "fue":"false"
}
```  

#### Descripcio camps  

També trobareu la totalitat de paràmetres previstos actualment en el [model de dades de la resposta](/model_dades_mygov.xslx).
Els paràmetres segueixen una estructura determinada pel nom del mateix en minúscules i, opcionalment, l'àmbit: "parametre" i "parametreAmbit".

A continuació, trobareu la totalitat de paràmetres previstos actualment en el model de dades:

|   Paràmetre  |  Descripció  |  Obligatori  |   
| --- | --- | --- |   
| idExpedient | Identificador de l'expedient | SI |  
| tipusPersona  | Tipus de persona | SI (INTERESSAT, REPRESENTANT) |  
| codiINE10 | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | SI|
| procediment | Procediment |  SI |   
| dataIniciExpedient | Data inici. Format ISO_8601 YYYY-MM-DDThh:mm:ss| SI |   
| assumpteExpedient | Assumpte |  SI  |   
| urlExpedient | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | SI |   
| codiDIR3Organisme | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO|    
| familia |  Família o materia sobre el qual tracta el procediment. | NO |   
| dataPrevistaResolucio | Data prevista resolució. Format ISO_8601 YYYY-MM-DD | NO |   
| dataFinalitzacioExpedient | Data finalització. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |   
| codiFase |  Codificació de la fase en que es troba la tramitació de l'expedient. Pendent de definir el model tancat de les fases de tramitació de l'expedient per part de l'AOC. | NO |
| descripcioFase | Descripció de la FASE de tramitació de l'expedient, d'acord a la terminologia interna que faci servir l'Ens | NO |   
| estatExpedient | Estat | SI (OBERT,TANCAT) |   
| observacionsExpedient | Observacions sobre l'expedient que l'Ens vulgui informar | NO |   
| idActuacio | Identificador de l'actuació  | NO |   
| dataActuacio | Data i hora del número de registre d'entrada o sortida que ha donat origen a l'expedient. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |   
| numeroRegistre | Número de registre que ha donat origen a l'expedient | NO |  
| actuacioCiutada | Indica si l'expedient es troba pendent de realitzar alguna acció per part de la ciutadania | NO (SI, NO, NO_APLICA) |   
| fue | Paràmetre que indica si es tracta d'un expedient FUE. | NO (defecte false, true)

### Cita Prèvia

#### Exemple JSON


```json  
{
  "idCita":"345f4kdj4",
  
}
```  

#### Descripcio camps  

|       Paràmetre      | Descripció | Obligatori |  
| ----------------------| --- | --- |  
| idCita          | Identificador de la cita | SI |    
| assumpteCita | Assumpte de la cita | SI
| codiINE10              | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el HCC | SI |  
| codiDIR3Organisme      | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el HCC | NO |
| dataActuacio | Data de l'assentament o actuació. En els casos de Cita Previa serà la data que la persona va fer l'acció de reservar-la. No és una data de registre ni és la data reservada per ser atesa la persona. Format ISO_8601 YYYY-MM-DDThh:mm:ss | SI |
| dataCita | Data reservada per la persona o assignada per ser atesa. Format ISO_8601 YYYY-MM-DDThh:mm:ss | SI |
| urlCita | URL d'enllaç a la Cita Prèvia | SI |
| actuacioCiutada | Indica si l'expedient es troba pendent de realitzar alguna acció per part del ciutadà | NO (SI, NO, NO_APLICA) |
| nomInteressat | Nom de la persona interessada | NO |
| nomOficinaRegistreDesti | Nom de l'oficina de registre destí | NO |
| nomOficinaRegistreOrigen | Nom de l'oficina de registre origen | NO |
| nomRepresentant | Nom de la persona representant | NO |
| procediment | Identificador o nom del procediment | NO |
| dataAlerta | Data que servirà per iniciar el període d'alerta, avís, inici de periode voluntari de pagament, inici de termini administratiu, etc... Format ISO_8601 YYYY-MM-DD  | NO |
| dataRegistre | Data del registre de l'actuació. Format ISO_8601 YYYY-MM-DD | NO |
| viaPresentacio | Via de presentació. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |
| urlEdicio | URL d'enllaç a la gestió de la Cita Prèvia | NO |
| urlCancellacio | URL d'enllaç a la cancel·lació de la Cita Prèvia | NO |
| puntAtencio | Canal o punt d'atenció que realitzarà o va realitzar l'atenció | NO |
| adrecaAtencio | Adreça del canal o punt d'atenció que realitzarà o va realitzar l'atenció | NO |
| telefonAtencio | Telefon del canal o punt d'atenció que realitzarà o va realitzar l'atenció | NO |
| coordenadesGPS | Coordenades en format GPS de latitud i longitud. En cas de cita prèvia servirà per geolocalitzar el punt físic d'atenció | NO |
| contacteAtencio | Altres dades de contacte del canal o punt d'atenció que realitzarà o va realitzar l'atenció | NO |
| URLCoordenadesGPSGoogleMaps | Coordenades en format GPS de latitud i longitud. En cas de cita prèvia servirà per geolocalitzar el punt físic d'atenció. Específic Google Maps | NO |
| URLcoordenadesGPSCustom | Coordenades en format GPS de latitud i longitud. En cas de cita prèvia servirà per geolocalitzar el punt físic d'atenció. Específic de la solució pròpia | NO |


## Consultes   

El **HCC** farà les següents crides als sistemes integrats per consultar les dades.  

### Fase 1

#### 1. Consulta d'actuacions

Retorna el detall d'actuacions en haver-se informat un documentIdentificatiu a la petició.  

##### Peticio

`GET /consultaActuacions?{documentIdentificatiu,tipusDocumentIdentificatiu,tipusPersona,codiINE10,codiDIR3Organisme,dataInici,dataFi,tipusActuacio,fue}`  

##### Descripcio camps   

|   Paràmetre  | Descripció | Obligatori  |   
| --- | --- | --- |   
| documentIdentificatiu | Document identificatiu de la persona | SI |   
| tipusDocumentIdentificatiu | Tipus de document identificatiu | SI (NIF,NIE,PASSAPORT) |
| tipusPersona | Tipus de rol de la persona en relació a l'actuació | NO (INTERESSAT, REPRESENTANT) |   
| codiINE10 | Codi INE10 | NO |   
| codiDIR3Organisme | Codi DIR3 | NO |   
| dataInici | Data d'inici de la informació consultada | NO (Format ISO_8601 YYYY-MM-DD) |   
| dataFi | Data de fi de la informació consultada | NO (Format ISO_8601 YYYY-MM-DD) |   
| tipusActuacio | Tipus d'actuació | NO (ENTRADA, SORTIDA)| 
| fue | Determinació de si es tracta d'un tràmit FUE | NO (no informat: totes, true: actuacions FUE, false: actuacions no FUE)


##### Exemple peticio  

`GET /consultaActuacions?documentIdentificatiu=99999018D&tipusDocumentIdentificatiu=NIF&tipusPersona=INTERESSAT`


##### Descripcio camps resposta   

|       Paràmetre      | Descripció | Obligatori |  
| --- | --- | --- |  
| codiResultat | Retorna el codi resultant de l'operació | SI (200, 404, 500) |   
| descripcioResultat | Retorna un string de detall de l'operació | SI |
| actuacions | Array de resposta de les actuacions | SI |

Per exemple, la possibilitat de no trobar cap actuació per fer el retorn, en base a un document identificatiu, el resultat hauria de ser:

```json  
{
"codiResultat":"404",
"descripcioResultat":"No s'ha trobat cap informació"
}
``` 

##### Exemple resposta

En aquest exemple, la resposta conté dues actuacions: un registre d'entrada i una petició de cita prèvia.

```json  
{
   "codiResultat":"200",
   "descripcioResultat":"OK",
   "actuacions":[
      {       
       "idActuacio":"2015-E-123",
       "tipusPersona":"INTERESSAT",
       "codiINE10":"9821920002",
       "codiDIR3Organisme":"L01080193",
       "tipusActuacio":"ENTRADA",
       "dataActuacio":"2015-01-01T01:25:43",
       "numeroRegistre":"2015-E-357",
       "assumpteActuacio":"Sol·licitud bonificació IBI",
       "urlActuacio":"http://www.abc.cat/a/id_2015_123",
       "viaPresentacio":"Electrònica",
       "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
       "observacionsActuacio":"Bonificació per instal·lació de planta fotovoltaica d'autoconsum",
       "fue":"false"
     },
     {      
       "idActuacio":"2015-E-124",
       "tipusPersona":"INTERESSAT",
       "codiINE10":"9821920002",
       "codiDIR3Organisme":"L01080193",
       "tipusActuacio":"ENTRADA",
       "dataActuacio":"2015-01-02T01:25:43",
       "numeroRegistre":"2015-E-124",
       "assumpteActuacio":"Reserva cita prèvia - Cultura",
       "urlActuacio":"http://www.abc.cat/a/id_2015_124",
       "viaPresentacio":"Electrònica",
       "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
       "observacionsActuacio":"Reserva cita prèvia - Cultura",
       "fue":"false"
     }
    ]
 }
```

#### 2. Consulta d'expedients

Retorna el detall d'expedients en haver-se informat un identificador.

##### Peticio

`GET /consultaExpedients?{documentIdentificatiu,tipusDocumentIdentificatiu,tipusPersona,codiINE10,codiDIR3Organisme,dataInici,dataFi,estat,fue}`  

##### Descripcio camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| documentIdentificatiu | SI |   
| tipusDocumentIdentificatiu | SI (NIF,NIE,PASSAPORT) |   
| tipusPersona | NO (INTERESSAT, REPRESENTANT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO (Format ISO_8601 YYYY-MM-DD) |   
| dataFi | NO (Format ISO_8601 YYYY-MM-DD) |   
| estat | NO (OBERT,TANCAT) |   
| fue | NO (no informat: tots, true: expedients FUE, false: expedients no FUE)

##### Exemple peticio  

`GET /consultaExpedients?documentIdentificatiu=99999018D&tipusDocumentIdentificatiu=NIF}`  

##### Descripcio camps resposta   

|   Paràmetre  |  Descripció  |  Obligatori  |   
| --- | --- | --- |   
| codiResultat | Retorna el codi resultant de l'operació | SI (200, 404, 500) |   
| descripcioResultat | Retorna un string de detall de l'operació | SI |
| expedients | Array de resposta dels expedients | SI |

Per la possibilitat de no trobar cap expedient per fer el retorn, en base a un document identificatiu, el resultat hauria de ser:

```json  
{
"codiResultat":"404",
"descripcioResultat":"No s'ha trobat cap expedient"
}
``` 

##### Exemple resposta

En aquest exemple, la resposta conté dos expedients: un sobre una llicència d'obres i un altre sobre una beca.

```json  
{
   "codiResultat":"200",
   "descripcioResultat":"OK",
   "expedients":[
      {
      "idExpedient":"EXP-2021-1",
      "tipusPersona":"INTERESSAT",
      "codiINE10":"801930008",
      "codiDIR3Organisme":"L01080193",
      "procediment":"OBRES1 - llicència d'obres",
      "dataIniciExpedient":"2021-07-25T01:25:43",
      "assumpteExpedient":"llicència d'obres",
      "urlExpedient":"http://www.abc.cat/e/2021_225",
      "familia":"Obres",
      "dataPrevistaResolucio":"2021-09-25",
      "codiFase":"02",
      "descripcioFaseExpedient":"En tramitació",
      "estatExpedient":"OBERT",
      "observacionsExpedient":"llicència - Reforma planta baixa",
      "idActuacio":"2021-E-225",
      "dataActuacio":"2021-05-23T18:25:43",
      "actuacioCiutada":"NO_APLICA",
      "fue":"false"
    },
    {
      "idExpedient":"EXP-2021-2",
      "tipusPersona":"INTERESSAT",
      "codiINE10":"801930008",
      "codiDIR3Organisme":"L01080193",
      "procediment":"Sol·licitud beca menjador escolar",
      "dataIniciExpedient":"2021-07-25T01:25:43",
      "assumpteExpedient":"Beca menjador",
      "urlExpedient":"http://www.abc.cat/e/2021_2",
      "familia":"Beques",
      "dataPrevistaResolucio":"2021-07-25",
      "codiFase":"02",
      "descripcioFaseExpedient":"En tramitació",
      "estatExpedient":"OBERT",
      "observacionsExpedient":"En curs",
      "idActuacio":"2021-E-225",
      "dataActuacio":"2021-05-23T18:25:43",
      "actuacioCiutada":"NO_APLICA",
      "fue":"false"
    }
   ]
}  
```

#### 3. Consulta de cites previes

Retorna el detall de cites previes en haver-se informat un documentIdentificatiu a la petició.  

##### Peticio

`GET /consultaCitesPrevies?{documentIdentificatiu,tipusDocumentIdentificatiu,codiINE10,codiDIR3Organisme,dataInici,dataFi}`  

##### Descripcio camps   

|   Paràmetre  | Descripció | Obligatori  |   
| --- | --- | --- |   
| documentIdentificatiu | Document identificatiu de la persona | SI |   
| tipusDocumentIdentificatiu | Tipus de document identificatiu | SI (NIF,NIE,PASSAPORT) |
| codiINE10 | Codi INE10 | NO |   
| codiDIR3Organisme | Codi DIR3 | NO |   
| dataInici | Data d'inici de la informació consultada | NO (Format ISO_8601 YYYY-MM-DD) |   
| dataFi | Data de fi de la informació consultada | NO (Format ISO_8601 YYYY-MM-DD) |   

##### Exemple peticio  

`GET /consultaCitesPrevies?documentIdentificatiu=99999018D&tipusDocumentIdentificatiu=NIF`


##### Descripcio camps resposta   

|       Paràmetre      | Descripció | Obligatori |  
| --- | --- | --- |  
| codiResultat | Retorna el codi resultant de l'operació | SI (200, 404, 500) |   
| descripcioResultat | Retorna un string de detall de l'operació | SI |
| actuacions | Array de resposta de les actuacions | SI |


Per exemple, la possibilitat de no trobar cap actuació per fer el retorn, en base a un document identificatiu, el resultat hauria de ser:

```json  
{
"codiResultat":"404",
"descripcioResultat":"No s'ha trobat cap informació"
}
``` 

##### Exemple resposta

En aquest exemple, la resposta conté dues cites.

```json  
{
   "codiResultat":"200",
   "descripcioResultat":"OK",
   "citesprevies":[
      {       
       "idCita":"2015-E-123"      
     },
     {      
       "idCita":"2015-E-124"     
     }
    ]
 }
```

### Taula de significats dels codis d'error

| Codi | Significat |
| --- | --- |
| 200 | Petició provessada correctament, es retornen resultats |
| 404 | No s'ha pogut trobar el contingut sol·licitat. | 
| 500 | El servidor ha trobat una situació que no sap com gestionar-la | 


### Fase 2

Donada la naturalesa de la metodologia àgil escollida pel desenvolupament del projecte, la fase 2 és troba pendent de definició.
Per tant, s'aconsella contactar amb la persona responsable del servei si es vol continuar amb les tasques d'integració de la fase 2.
