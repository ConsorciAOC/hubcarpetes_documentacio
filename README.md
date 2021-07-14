# AOC Hub de carpetes ciutadanes
Projecte de documentació per als integradors del Hub de Carpetes Ciutadanes (HCC) del Consorci AOC. El HCC consultarà a cada ens integrat informació sobre actuacions i expedients que aquests hauràn d'exposar a través d'una interfície REST que compleixi el model i consultes descrites a continuació.

Per integrar-se al servei caldrà fer arribar a l'AOC les **url** de les consultes:
1. [consulta agrupada](#consulta-agrupada).
2. [consulta detallada d'una actuació](#actuacio-1).
3. [consulta detallada d'un expedient](#expedient-1).
4. [consulta detallada d'una o més actuacions](#actuacions).
5. [consulta detallada d'un o més expedients](#expedients).

Es recomana securitzar els endpoints exposats:
- Comunicacions via HTTPS.
- Validar el [certificat client](/SEGELL_AOC.cer) que l'AOC presentarà al establir la connexió.
- Filtrant per la següent IP per la qual arribarà el CAOC als serveis web oferts: `157.97.64.126`

Un cop l'AOC tingui la informació necessària per consultar les dades validarà que la integració funcioni correctament.



_Aquest document està en fase d'esborrany i pot patir alguns canvis._

- [Model](#model)
  * [Actuacio](#actuacio)
    + [Exemple JSON](#exemple-json)
    + [Descripció camps](#descripcio-camps)
  * [Expedient](#expedient)
    + [Exemple JSON](#exemple-json-1)
    + [Descripció camps](#descripcio-camps-1)
- [Consultes](#consultes)
  * [Consulta agrupada](#consulta-agrupada)
    + [Petició](#peticioo)
    + [Descripció camps](#descripcio-camps-6)
    + [Exemple petició](#exemple-peticio-4)
    + [Exemple resposta](#exemple-resposta-4)
  * [Consulta detallada](#consulta-detallada)
    + [Actuacio](#actuacio-1)
      - [Petició](#peticio)
      - [Descripció camps](#descripcio-camps-2)
      - [Exemple petició](#exemple-peticio)
      - [Exemple resposta](#exemple-resposta)
    + [Expedient](#expedient-1)
      - [Petició](#peticio-2)
      - [Descripció camps](#descripcio-camps-4)
      - [Exemple petició](#exemple-peticio-2)
      - [Exemple resposta](#exemple-resposta-2)
   + [Actuacions](#actuacions)
      - [Petició](#peticio-1)
      - [Descripció camps](#descripcio-camps-3)
      - [Exemple petició](#exemple-peticio-1)
      - [Exemple resposta](#exemple-resposta-1)
    + [Expedients](#expedients)
      - [Petició](#perticio)
      - [Descripció camps](#descripcio-camps-5)
      - [Exemple petició](#exemple-peticio-3)
      - [Exemple resposta](#exemple-resposta-3)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Model

A continuació de descriuen els atributs i estructura dels objectes d'intercanvi d'informació entre el HCC i els ens integrats.

### Actuacio

#### Exemple JSON

```json  
{
   "codiINE10":"9821920002",
   "codiDIR3Organisme":"L01080193",
   "tipusPersona":"REPRESENTANT",
   "tipus":"ENTRADA",
   "identificador":"abc371",
   "dataActuacio":"2012-04-23T18:25:43.511Z",
   "assumpte":"Assumpte",
   "url":"http://www.abc.com/698",
   "viaPresentacio":"Via",
   "numeroExpedient":"225",
   "procediment":"Procediment",
   "observacions":"Observacions",
   "referenciaExterna":"Referencia",   
   "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA",
   "fue":"false"
}
```  

#### Descripció camps  

|       Paràmetre      | Descripció | Obligatori |  
| ----------------------| --- | --- |  
| codiINE10              | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO (si s'ha indicat el codiDIR3Organisme) |  
| codiDIR3Organisme      | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO (si s'ha indicat el codiINE10) |
| tipusPersona           | Tipus de persona | SI (INTERESSAT, REPRESENTANT |  
| tipus                  | Tipus d'actuació | SI (ENTRADA, SORTIDA)|  
| identificador          | Identificador únic | SI |  
| dataActuacio           | Data de l'actuació o la data registre segons apliqui. Format YYYY-MM-DDThh:mm:ss.sssZ | SI |  
| assumpte               | Assumpte | SI |  
| url                    | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | SI |  
| viaPresentacio         | Via presentacio | NO |  
| numeroExpedient        | Numero de l'expedient | NO |  
| procediment            | Procediment| NO |  
| observacions           | Observacions sobre l'expedient que l'Ens vulgui informar | NO |  
| referenciaExterna      | Referencia externa | NO |
| canalPreferentNotificacio         | Canal preferent | NO (DIRECCIO_POSTAL, COMPAREIXENCA_ELECTRONICA, DIRECCIO_ELECTRONICA_HABILITADA) |  
| fue					 | Actuació FUE? | NO (defecte false, true)
    
### Expedient  

#### Exemple JSON

```json  
{
   "identificador":"id_43",
   "assumpte":"assumpte",   
   "codiINE10": "9821920002",
   "Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov":"L01080193",
   "procediment":"procediment",
   "dataInici": "2020-04-23T18:25:43.511Z",
   "url":"www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":"2020-04-24T18:25:43.511Z",
   "descripcioFase":"Descripció",
   "estat":"TANCAT",
   "observacions":"Observacions",
   "numeroRegistre":"NUM_43",
   "dataRegistre":"2020-01-23T18:25:43.511Z",
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase",
   "fue":"false"
}
```  
 
#### Descripció camps  

|   Paràmetre  |  Descripció  |  Obligatori  |   
| --- | --- | --- |   
| identificador | Identificador únic | SI |   
| assumpte | Assumpte |  SI  |   
| codiINE10 | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO (si s'ha indicat el codiDIR3Organisme)|
| codiDIR3Organisme | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO (si s'ha indicat el codiINE10)|   
| procediment | Procediment |  SI |   
| dataInici | Data inici. Format YYYY-MM-DDThh:mm:ss.sssZ | SI |   
| url | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | SI |   
| familia |  Família o materia sobre el qual tracta el procediment. | NO |   
| dataPrevistaResolucio | Data prevista resolució. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| dataFinalitzacio | Data finalització. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| descripcioFase | Descripció de la FASE de tramitació de l'expedient, d'acord a la terminologia interna que faci servir l'Ens | NO |   
| estat | Estat | NO (OBERT,TANCAT) |   
| observacions | Observacions sobre l'expedient que l'Ens vulgui informar | NO |   
| numeroRegistre |  Número de registre d'entrada o sortida que ha donat origen a l'expedient | NO |   
| dataRegistre | Data i hora del número de registre d'entrada o sortida que ha donat origen a l'expedient. Format YYYY-MM-DDThh:mm:ss.sssZ | NO |   
| actuacioCiutada | Indica si l'expedient es troba pendent de realitzar alguna acció per part de la ciutadania | NO (SI, NO, NO_APLICA) |   
| fase |  Fase en que es troba la tramitació de l'expedient. Pendent de definir el model tancat de les fases de tramitació de l'expedient. | NO | 
| fue | Camp que indica si es tracta d'un expedient FUE. | NO (defecte false, true)
  
  
## Consultes   

El **HCC** farà les següents crides als sistemes integrats per consultar les dades.  
  
### Consulta detallada  
    
#### Actuació  

Retorna el detall d'una actuació única a partir del seu identificador

##### Petició

` GET /consultaActuacioDetallada?{identificador,codiINE10,codiDIR3Organisme}`  
  
##### Descripció camps 

|  Paràmetre | Obligatori |  
|---|---|  
|identificador|SI|  
|codiINE10|NO|
|codiDIR3Organisme|NO|

##### Exemple petició

` GET /consultaActuacioDetallada?identificador=3fh54h6hfh4h43jd24354`  
    
##### Exemple resposta 

```json   
{
   "codiINE10":"9821920002",
   "tipusPersona":"INTERESSAT",
   "tipus":"ENTRADA",
   "identificador":"3fh54h6hfh4h43jd24354",
   "dataRegistre":"2020-04-24T18:25:43.511Z",
   "assumpte":"Assumpte",
   "url":"http://www.abc.com/28"
}
 ```  
    
#### Actuacions

Retorna el detall d'una o més actuacions  
  
##### Petició 

` GET /consultaActuacionsDetallada?{documentIdentificador,tipusDocumentIdentificador,codiINE10,codiDIR3Organisme,dataInici,dataFi}`  
  
##### Descripció camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI |   
| tipusDocumentIdentificador | SI (NIF,NIE,PASSAPORT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| tipus | NO |   
| fue | NO (no informat: totes, true: actuacions FUE, false: actuacions no FUE)
  
  
##### Exemple petició  

` GET /consultaActuacionsDetallada?documentIdentificador=12345678A&tipusDocumentIdentificador=NIF&codiINE10=9821920002&dataInici=2020-04-23T18:25:43.511Z&dataFi=2021-04-23T18:25:43.511Z&tipus=ENTRADA&fue=true` 
    
##### Exemple resposta 

```json  
{
   "identificador":"id_43",
   "assumpte":"Assumpte",
   "codiINE10":"9821920002",
   "codiDIR3Organisme":"L01080193",
   "procediment":"Procediment",
   "dataInici":"2020-05-23T18:25:43.511Z",
   "url":"http://www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":"2022-04-23T18:25:43.511Z",
   "descripcioFase":"Descripció",
   "estat":"TANCAT",
   "observacions":"Observacions",
   "numeroRegistre":"NUM_43",
   "dataRegistre":"2020-05-22T18:25:43.511Z",
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase",
   "fue":"true"
}
``` 
   
#### Expedient  

Retorna el detall d'un expedient únic a partir del seu identificador

##### Petició  

` GET /consultaExpedientDetallada?{identificador,codiINE10,codiDIR3Organisme}`  
  
##### Descripció camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| identificador | SI |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
  
##### Exemple petició

` GET /consultaExpedientDetallada?identificador=54g657h243k234h`  
  
##### Exemple resposta 

```json  
{
   "identificador":"id_43",
   "assumpte":"Assumpte",
   "codiINE10":"9821920002",
   "codiDIR3Organisme":"L01080193",
   "procediment":"Procediment",
   "dataInici":"2020-04-23T18:25:43.511Z",
   "url":"http://www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":"2020-05-23T18:25:43.511Z",
   "descripcioFase":"Descripció",
   "estat":"TANCAT",
   "observacions":"Observacions",
   "numeroRegistre":"NUM_43",
   "dataRegistre":"2020-04-24T18:25:43.511Z",
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase",
   "fue":"false"
}
```  
  
#### Expedients  

Retorna el detall d'un o més expedients  
  
##### Perticio  

` GET /consultaExpedientsDetallada?{documentIdentificador,tipusDocumentIdentificador,codiINE10,codiDIR3Organisme,dataInici,dataFi,estat,fue}`  
  
##### Descripció camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI |   
| tipusDocumentIdentificador | SI (NIF,NIE,PASSAPORT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| estat | NO (OBERT,TANCAT) |   
| fue | NO (no informat: tots, true: expedients FUE, false: expedients no FUE)
   
##### Exemple petició  

` GET /consultaExpedientsDetallada?documentIdentificador=34867564R&tipusDocumentIdentificador=NIF&codiINE10=9821920002&dataInici=2019-04-26T08:25:43.123Z&dataFi=2022-04-26T08:25:43.123Z&estat=OBERT&fue=false}`  

##### Exemple resposta 

```json  
{
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripció",
   "expedients":[
      {
         "identificador":"id_65",
         "assumpte":"Assumpte",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "procediment":"Procediment",
         "dataInici":"2020-04-24T01:25:43.987Z",
         "url":"http://www.test.com",
         "tipusPersona":"REPRESENTANT",
         "familia":"F1",
         "dataPrevistaResolucio":"2020-04-26T08:25:43.123Z",
         "descripcioFase":"Descripció",
         "estat":"OBERT",
         "observacions":"Observacions",
         "numeroRegistre":"NUM_43",
         "dataRegistre":"2020-04-23T18:25:43.511Z",
         "actuacioCiutada":"NO_APLICA",
         "fase":"fase",
         "fue":"false"
      },
      {
         "identificador":"id_82",
         "assumpte":"Assumpte",
         "codiINE10":"9821920002",
         "codiDIR3Organisme":"L01080193",
         "procediment":"Procediment",
         "dataInici":"2020-01-26T14:25:43.001Z",
         "url":"http://www.test.com",
         "tipusPersona":"REPRESENTANT",
         "estat":"OBERT",
         "fue":"false"
      },
      {
         "..."
      }
   ]
}  
```  

### Consulta agrupada  

Retorna un resum quantitatiu de les actuacions i/o expedients donat un document identificador. En el cas de les actuacions agrupades segons el tipus, entrada o sortida. I en el cas dels expedients, agrupats per estat.  
  
#### Petició

` GET /consultaAgrupada?{documentIdentificador,tipusDocumentIdentificador,codiINE10,codiDIR3Organisme,fue}`  
  
#### Descripció camps   

|   Paràmetre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI | 
| tipusDocumentIdentificador | SI (NIF,NIE,PASSAPORT) |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| fue | NO (no informat: tots, true: actuacions/expedients FUE, false: actuacions/expedients no FUE)
  
#### Exemple petició   

` GET /consultaAgrupada?documentIdentificador=98906049A&tipusDocumentIdentificador&codiINE10=9821920002`  
  
#### Exemple resposta 

```json  
 {
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripció resultat",
   "agrupacions":[
      {
         "codiINE10":"9821920002", 
         "agrupacioActuacions":{
            "entrades":{
               "numTotal":9,
               "identificadors":[
                  "384",
                  "260",
                  "462",
                  "189",
                  "637",
                  "156",
                  "313",
                  "425",
                  "320"
               ]
            },
            "sortides":{
               "numTotal":2,
               "identificadors":[
                  "208",
                  "41"
               ]
            }
         },
         "agrupacioExpedients":{
            "expedientsOberts":{
               "numTotal":8,
               "identificadors":[
                  "592",
                  "235",
                  "367",
                  "0",
                  "436",
                  "890",
                  "839",
                  "205"
               ]
            },
            "expedientsTancats":{
               "numTotal":2,
               "identificadors":[
                  "565",
                  "522"
               ]
            }
         }
      }
   ]
}
 ```  
 
