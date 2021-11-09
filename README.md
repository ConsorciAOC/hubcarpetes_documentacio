# AOC Hub de carpetes ciutadanes
Projecte de documentació per als integradors del Hub de Carpetes Ciutadanes (HCC) del Consorci AOC. El HCC consultarà a cada ens integrat informació sobre actuacions (accions de relació entre la ciutadania i l'Administració entesos des d'un punt de vista ampli i tipificats per entrada o sortida) i expedients que hauran d'exposar a través d'una interfície REST que compleixi el model i les consultes descrites a continuació.

Com a primers passos, abans de les tasques d'integració, es recomana la lectura del [document de presentació](/presentacio_myg_20201105.pdf).

Per integrar-se al servei, caldrà fer arribar a l'AOC les **URLs** de les consultes. El model de desenvolupament es concretarà en fases i podria variar durant el temps:

1. Primera fase de desenvolupament (Prioritzada)
    1. [Consulta d'actuacions](#1-consulta-dactuacions) (Prioritat alta)
    2. [Consulta d'expedients](#2-consulta-dexpedients) (Prioritat mitja)

2. Segona fase de desenvolupament (No prioritzada)
    1. Consulta agrupada
    2. Consulta detallada d'una actuació
    3. Consulta detallada d'un expedient

    Aquesta segona fase està pendent de ser concretada i es troba en estat d'esborrany.

Es recomana securitzar els endpoints exposats:
- Comunicacions via HTTPS.
- Validar el [certificat client](/SEGELL_AOC.cer) que l'AOC presentarà al establir la connexió.
- Filtrar per la IP `157.97.64.126` (per la qual arribarà el CAOC als serveis web oferts)

Un cop l'AOC tingui la informació necessària per consultar les dades, validarà que la integració funcioni correctament.


_Aquest document està en fase d'esborrany i pot patir canvis._

- [Model](#model)
  * [Actuacio](#actuacio)
    + [Exemple JSON](#exemple-json)
    + [Descripcio camps](#descripcio-camps)
  * [Expedient](#expedient)
    + [Exemple JSON](#exemple-json-1)
    + [Descripcio camps](#descripcio-camps-1)
- [Consultes](#consultes)
  * [Fase 1](#fase-1)
    + [1. Consulta d'actuacions](#1-consulta-d-actuacions)
      - [Peticio](#peticio)
      - [Descripcio camps](#descripcio-camps-2)
      - [Exemple peticio](#exemple-peticio)
      - [Descripcio camps resposta](#descripcio-camps-resposta)
      - [Exemple resposta](#exemple-resposta)
    + [2. Consulta d'expedients](#2-consulta-d-expedients)
      - [Peticio](#peticio-1)
      - [Descripcio camps](#descripcio-camps-3)
      - [Exemple peticio](#exemple-peticio-1)
      - [Descripcio camps resposta](#descripcio-camps-resposta-1)
      - [Exemple resposta](#exemple-resposta-1)
  * [Fase 2](#fase-2)


<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Model

Abans de les tasques d'integració, es recomana la lectura del [document de presentació](/presentacio_myg_20201105.pdf).

A continuació, es descriuen els atributs i l'estructura dels objectes d'intercanvi d'informació entre el HCC i els ens integrats. Podeu trobar la informació en detall a cadascuna de les consultes.

### Actuacio

#### Exemple JSON

El model s'ha dividit en blocs d'informació integrats en la resposta: Identificador de l'objecte, de la persona, de l'ens, de l'actuació i de l'expedient.

```json  
{
  "idActuacio":"id_43",
  "documentIdentificatiu":"12345678A",
  "tipusDocumentIdentificatiu":"NIF",
  "tipusPersona":"REPRESENTANT",
  "codiINE10":"9821920002",
  "codiDIR3Organisme":"L01080193",
  "tipusActuacio":"ENTRADA",
  "dataActuacio":"2012-04-23T18:25:43.511Z",
  "numeroRegistre":"2021-E-225",
  "assumpteActuacio":"Assumpte",
  "urlActuacio":"http://www.abc.com/698",
  "viaPresentacio":"Electrònica",
  "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
  "observacionsActuacio":"Observacions",
  "idExpedient":"2021-225",
  "urlExpedient":"http://www.abc.com/2021_225",
  "procediment":"Llicència d'obres",
  "referenciaExterna":"Llicència 116/2021 - Reforma planta baixa",   
  "fue":"false"
}
```  

#### Descripcio camps  

També trobareu la totalitat de paràmetres previstos actualment en el [model de dades de la resposta](/model_dades_mygov.xslx).
Els paràmetres segueixen una estructura determinada pel nom del mateix en minúscules i, opcionalment, l'àmbit: "parametre" i "parametreAmbit".

|       Paràmetre      | Descripció | Obligatori |  
| ----------------------| --- | --- |  
| idActuacio          | Identificador de l'actuació | SI |  
| documentIdentificatiu          | Document identificatiu únic de la persona | SI |  
| tipusDocumentIdentificatiu          | Tipus de document identificatiu únic de la persona | SI (NIF,NIE,PASSAPORT)|  
| tipusPersona           | Tipus de persona | SI (INTERESSAT, REPRESENTANT |  
| codiINE10              | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | SI |  
| codiDIR3Organisme      | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO |
| tipusActuacio          | Tipus d'actuació | SI (ENTRADA, SORTIDA)|  
| dataActuacio           | Data de l'actuació. Format YYYY-MM-DDThh:mm:ss.sssZ | SI |  
| numeroRegistre           | Número de registre en format string | NO |  
| assumpteActuacio               | Assumpte | SI |  
| urlActuacio            | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'actuació farà clic en aquest enllaç | SI |  
| viaPresentacio         | Via de presentació | NO |  
| canalPreferentNotificacio         | Canal preferent | NO (DIRECCIO_POSTAL, COMPAREIXENCA_ELECTRONICA, DIRECCIO_ELECTRONICA_HABILITADA) |  
| observacionsActuacio   | Observacions sobre l'expedient que l'Ens vulgui informar | NO |  
| idExpedient        | Número de l'expedient | NO |  
| urlExpedient            | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | NO |
| procediment            | Procediment | NO |  
| referenciaExterna      | Referencia externa | NO |
| fue | Paràmetre que indica si es tracta d'un expedient FUE. | NO (defecte false, true)

### Expedient  

#### Exemple JSON

El model s'ha dividit en blocs d'informació integrats en la resposta: Identificador de l'objecte, de la persona, de l'ens, de l'actuació i de l'expedient. El paràmetre "dataFinalitzacioExpedient" està informat però a mode d'exemple il·lustratiu però, en aquest cas concret, és incongruent donat que l'expedient es troba "En tramitació" i no hauria de tenir data de finalització.

```json  
{
   "idExpedient":"2021_225",
   "documentIdentificatiu":"12345678A",
   "tipusDocumentIdentificatiu":"NIF",
   "tipusPersona":"INTERESSAT",
   "codiINE10":"9821920002",
   "codiDIR3Organisme":"L01080193",
   "procediment":"OBRES1 - Llicència d'obres",
   "dataIniciExpedient":"2020-04-24T01:25:43.987Z",
   "assumpteExpedient":"Llicència d'obres - Carrer principal, 34",   
   "urlExpedient":"www.seu.com/e/2021_225",
   "familia":"Obres",
   "dataPrevistaResolucio":"2020-04-26T08:25:43.123Z",
   "dataFinalitzacioExpedient":"2020-10-24T18:25:43.511Z",
   "codiFase":"02",
   "descripcioFaseExpedient":"En tramitació",
   "estatExpedient":"Obert",
   "observacionsExpedient":"Llicència - Reforma planta baixa",
   "idActuacio":"2021-E-225",
   "dataActuacio":"2021-04-23T18:25:43.511Z",
   "numeroRegistre":"2021-E-225",
   "assumpteActuacio":"Sol·licitud de llicència d'obres",
   "urlActuacio":"http://www.seu.cat/a/2021-E-225",
   "actuacioCiutada":"NO_APLICA",
   "fue":"false"
}
```  

#### Descripcio camps  

També trobareu la totalitat de paràmetres previstos actualment en el [model de dades de la resposta](/model_dades_mygov.xslx).
Els paràmetres segueixen una estructura determinada pel nom del mateix en minúscules i, opcionalment, l'àmbit: "parametre" i "parametreAmbit".

A continuació, trobareu la totalitat de paràmetres previstos actualment en el model de la resposta.

|   Paràmetre  |  Descripció  |  Obligatori  |   
| --- | --- | --- |   
| idExpedient | Identificador de l'expedient | SI |  
| documentIdentificatiu | Document identificatiu únic de la persona | SI |   
| tipusDocumentIdentificatiu | Tipus de document identificatiu | SI (NIF,NIE,PASSAPORT) |  
| tipusPersona  | Tipus de persona | SI (INTERESSAT, REPRESENTANT) |  
| codiINE10 | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | SI|
| codiDIR3Organisme | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO|    
| procediment | Procediment |  SI |   
| dataIniciExpedient | Data inici. Format YYYY-MM-DDThh:mm:ss.sssZ | SI |   
| assumpteExpedient | Assumpte |  SI  |   
| urlExpedient | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | SI |   
| familia |  Família o materia sobre el qual tracta el procediment. | NO |   
| dataPrevistaResolucio | Data prevista resolució. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| dataFinalitzacioExpedient | Data finalització. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| codiFase |  Codificació de la fase en que es troba la tramitació de l'expedient. Pendent de definir el model tancat de les fases de tramitació de l'expedient per part de l'AOC. | NO |
| descripcioFase | Descripció de la FASE de tramitació de l'expedient, d'acord a la terminologia interna que faci servir l'Ens | NO |   
| estatExpedient | Estat | NO (OBERT,TANCAT) |   
| observacionsExpedient | Observacions sobre l'expedient que l'Ens vulgui informar | NO |   
| idActuacio | Identificador de l'actuació  | NO |   
| dataActuacio | Data i hora del número de registre d'entrada o sortida que ha donat origen a l'expedient. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| numeroRegistre | Número de registre que ha donat origen a l'expedient | NO |  
| actuacioCiutada | Indica si l'expedient es troba pendent de realitzar alguna acció per part de la ciutadania | NO (SI, NO, NO_APLICA) |   
| fue | Paràmetre que indica si es tracta d'un expedient FUE. | NO (defecte false, true)


## Consultes   

El **HCC** farà les següents crides als sistemes integrats per consultar les dades.  

### Fase 1

#### 1. Consulta d'actuacions

Retorna el detall d'actuacions en haver-se informat un documentIdentificatiu a la petició.  

##### Peticio

`GET /consultaActuacions?{documentIdentificatiu,tipusDocumentIdentificatiu,tipusPersona,codiINE10,codiDIR3Organisme,dataInici,dataFi,tipusActuacio,fue}`  

##### Descripcio camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| documentIdentificatiu | SI |   
| tipusDocumentIdentificatiu | SI (NIF,NIE,PASSAPORT) |
| tipusPersona           | NO (INTERESSAT, REPRESENTANT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| tipusActuacio | NO |   
| fue | NO (no informat: totes, true: actuacions FUE, false: actuacions no FUE)


##### Exemple peticio  

`GET /consultaActuacions?documentIdentificatiu=12345678A&tipusDocumentIdentificatiu=NIF&tipusPersona=INTERESSAT&dataInici=2020-04-23T18:25:43.511Z&dataFi=2021-04-23T18:25:43.511Z&tipusActuacio=ENTRADA&fue=true`


##### Descripcio camps resposta   

|       Paràmetre      | Descripció | Obligatori |  
| --- | --- | --- |  
| codiResultat | Retorna el codi d'error | SI (200, 301, 302, 400, 401, 403 ,404, 500, 504, 509) |   
| descripcioResultat | Retorna un string de detall de l'error | SI |
| actuacions | Array de resposta de les actuacions | SI |


##### Exemple resposta

A continuació trobareu un exemple de resposta que, per un documentIdentificatiu, es retornen tres actuacions. La primera està vinculada a un expedient, la segona encara no en té i la tercera és una actuació que no esdebé registre d'entrada però genera evidència. Aquest darrer cas, és una situació extraordinària i es contemplada per relacions amb l'Administració que s'han deixat fora del registre central (reserva de pistes exportives, cita prèvia, trucades informatives, peticions informals, etc).

```json  
{
   "codiResultat":"200",
   "descripcioResultat":"OK",
   "Actuacions":[
      {
         "idActuacio":"2021-E-225",
         "documentIdentificatiu":"12345678A",
         "tipusDocumentIdentificatiu":"NIF",
         "tipusPersona":"INTERESSAT",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "tipusActuacio":"ENTRADA",
         "dataActuacio":"2020-05-23T18:25:43.511Z",
         "numeroRegistre":"2021-E-225",
         "assumpteActuacio":"Sol·licitud de llicència d'obres",
         "urlActuacio":"http://www.abc.cat/a/id_43",
         "viaPresentacio":"Electrònica",
         "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",         
         "observacionsActuacio":"Si us plau, voldrem començar l'actuació lo abans possible",
         "idExpedient":"2021_225",
         "urlExpedient":"http://www.abc.cat/e/2021_225",
         "procediment":"OBRES1 - Llicència d'obres",
         "referenciaExterna":"Llicència 116/2021 - Reforma planta baixa",   
         "fue":"false"
      }
      {
         "idActuacio":"2021-E-357",
         "documentIdentificatiu":"12345678A",
         "tipusDocumentIdentificatiu":"NIF",
         "tipusPersona":"INTERESSAT",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "tipusActuacio":"ENTRADA",
         "dataActuacio":"2021-08-22T19:02:43.511Z",
         "numeroRegistre":"2021-E-357",
         "assumpteActuacio":"Sol·licitud bonificació IBI",
         "urlActuacio":"http://www.abc.cat/a/id_434",
         "viaPresentacio":"Electrònica",
         "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
         "observacionsActuacio":"Bonificació per instal·lació de planta fotovoltaica d'autoconsum",
         "fue":"false"
      }
      {
         "idActuacio":"CP_3452",
         "documentIdentificatiu":"12345678A",
         "tipusDocumentIdentificatiu":"NIF",
         "tipusPersona":"INTERESSAT",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "tipusActuacio":"ENTRADA",
         "dataActuacio":"2021-10-22T19:02:43.511Z",
         "assumpteActuacio":"Reserva cita prèvia - Cultura",
         "urlActuacio":"http://www.abc.cat/a/id_434",
         "viaPresentacio":"Electrònica",
         "observacionsActuacio":"Reserva de cita prèvia pel 17/12/2021",
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
| tipusPersona           | NO (INTERESSAT, REPRESENTANT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| estat | NO (OBERT,TANCAT) |   
| fue | NO (no informat: tots, true: expedients FUE, false: expedients no FUE)

##### Exemple peticio  

`GET /consultaExpedients?documentIdentificatiu=12345678A&tipusDocumentIdentificatiu=NIF&dataInici=2019-04-26T08:25:43.123Z&dataFi=2022-04-26T08:25:43.123Z&estat=OBERT&fue=false}`  

##### Descripcio camps resposta   

|   Paràmetre  |  Descripció  |  Obligatori  |   
| --- | --- | --- |   
| codiResultat | Retorna el codi d'error | SI (200, 301, 302, 400, 401, 403 ,404, 500, 504, 509) |   
| descripcioResultat | Retorna un string de detall de l'error | SI |
| expedients | Array de resposta dels expedients | SI |


##### Exemple resposta

```json  
{
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripció",
   "expedients":[
      {
         "idExpedient":"2021_225",
         "documentIdentificatiu":"12345678A",
         "tipusDocumentIdentificatiu":"NIF",
         "tipusPersona":"INTERESSAT",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "procediment":"OBRES1 - Llicència d'obres",
         "dataIniciExpedient":"2020-04-24T01:25:43.987Z",
         "assumpteExpedient":"Llicència d'obres",
         "urlExpedient":"http://www.abc.cat/e/2021_225",
         "familia":"Obres",
         "dataPrevistaResolucio":"2020-04-26T08:25:43.123Z",
         "codiFase":"02",
         "descripcioFaseExpedient":"En tramitació",
         "estatExpedient":"OBERT",
         "observacionsExpedient":"Llicència - Reforma planta baixa",
         "idActuacio":"2021-E-225",
         "dataActuacio":"2020-04-23T18:25:43.511Z",
         "actuacioCiutada":"NO_APLICA",
         "fue":"false"
      },
      {
         "idExpedient":"2021-356",
         "documentIdentificatiu":"12345678A",
         "tipusDocumentIdentificatiu":"NIF",
         "tipusPersona":"INTERESSAT",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "procediment":"Sol·licitud d'accés a la informació pública",
         "dataIniciExpedient":"2020-01-26T14:25:43.001Z",
         "urlExpedient":"http://www.abc.cat/e/2021_356",
         "estatExpedient":"OBERT",
      },
      {
         "..."
      }
   ]
}  
```  
### Fase 2

Donada la naturalesa de la metodologia àgil escollida pel desenvolupament del projecte, la fase 2 és troba pendent de definició.
Per tant, s'aconsella contactar amb la persona responsable del servei si es vol continuar amb les tasques d'integració de la fase 2.
