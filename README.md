# AOC Hub de carpetes ciutadanes
Projecte de documentació per als integradors del hub de carpetes ciutadanes del Consorci AOC.

_Aquest document està en fase d'esborrany i pot patir alguns canvis._

- [Model](#model)
  * [Actuacio](#actuacio)
    + [Exemple JSON](#exemple-json)
    + [Descripcio camps](#descripcio-camps)
  * [Expedient](#expedient)
    + [Exemple JSON](#exemple-json-1)
    + [Descripcio camps](#descripcio-camps-1)
- [Consultes](#consultes)
  * [Consulta detallada](#consulta-detallada)
    + [Actuacio](#actuacio-1)
      - [Peticio](#peticio)
      - [Descripcio camps](#descripcio-camps-2)
      - [Exemple peticio](#exemple-peticio)
      - [Exemple resposta](#exemple-resposta)
    + [Actuacions](#actuacions)
      - [Peticio](#peticio-1)
      - [Descripcio camps](#descripcio-camps-3)
      - [Exemple peticio](#exemple-peticio-1)
      - [Exemple resposta](#exemple-resposta-1)
    + [Expedient](#expedient-1)
      - [Peticio](#peticio-2)
      - [Descripcio camps](#descripcio-camps-4)
      - [Exemple peticio](#exemple-peticio-2)
      - [Exemple resposta](#exemple-resposta-2)
    + [Expedients](#expedients)
      - [Perticio](#perticio)
      - [Descripcio camps](#descripcio-camps-5)
      - [Exemple peticio](#exemple-peticio-3)
      - [Exemple resposta](#exemple-resposta-3)
  * [Consulta agrupada](#consulta-agrupada)
    + [Peticio](#peticioo)
    + [Descripcio camps](#descripcio-camps-6)
    + [Exemple peticio](#exemple-peticio-4)
    + [Exemple resposta](#exemple-resposta-4)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



## Model

### Actuacio

#### Exemple JSON

```json  
{
   "codiINE10":"658",
   "codiDIR3Organisme":"34554",
   "tipusPersona":"REPRESENTANT",
   "tipus":"ENTRADA",
   "identificador":"abc371",
   "dataActuacio":1604401681121,
   "assumpte":"Assumpte...",
   "url":"www.abc.com/698",
   "viaPresentacio":"Via...",
   "numeroExpedient":"225",
   "procediment":"Procediment...",
   "observacions":"Obs...",
   "referenciaExterna":"Referencia....",   
   "canalPreferentNotificacio":"COMPAREIXENCA_ELECTRONICA"
}
```  

#### Descripcio camps  

|       Parametre      | Descripcio | Obligatori |  
| ----------------------| --- | --- |  
| codiINE10              | Codi INE10 | NO (si s'ha inidicat el codiDIR3Organisme) |  
| codiDIR3Organisme      | Codi DIR3 | NO (si s'ha inidicat el codiINE10) |
| tipusPersona           | Tipus de persona | SI (INTERESSAT, REPRESENTANT |  
| tipus                  | Tipus d'actuacio | SI (ENTRADA, SORTIDA)|  
| identificador          | Identificador únic | SI |  
| dataActuacio           | Data de l'actuació o la data registre segons apliqui | SI |  
| assumpte               | Assumpte | SI |  
| url                    | URL | SI |  
| viaPresentacio         | Via presentacio | NO |  
| numeroExpedient        | Numero de l'expedient | NO |  
| procediment            | Procediment| NO |  
| observacions           | Observacions| NO |  
| referenciaExterna      | Referencia externa | NO |
| canalPreferentNotificacio         | Canal preferent | NO (DIRECCIO_POSTAL, COMPAREIXENCA_ELECTRONICA, DIRECCIO_ELECTRONICA_HABILITADA) |  
    
### Expedient  

#### Exemple JSON

```json  
{
   "identificador":"id_43",
   "assumpte":"assumpte",   
   "codiINE10": "1234",
   "codiDIR3Organisme":"789",
   "procediment":"procediment",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":1604401681254,
   "descripcioFase":"Descripcio",
   "estat":"FINALITZAT",
   "obervacions":"Observacions",
   "numeroRegistre":"NUM_43",
   "dataRegistre":1604401681254,
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase"
}
```  
 
#### Descripcio camps  

|   Parametre  |  Descripcio  |  Obligatori  |   
| --- | --- | --- |   
| identificador | Identificador únic | SI |   
| assumpte | Assumpte |  SI  |   
| codiINE10 | Codi INE10 | NO (si s'ha inidicat el codiDIR3Organisme)|
| codiDIR3Organisme | Codi DIR3 organisme | NO (si s'ha inidicat el codiINE10)|   
| procediment | Procediment |  SI |   
| dataInici | Data inici | SI |   
| url | URL | SI |   
| familia |  Familia  | NO |   
| dataPrevistaResolucio | Data prevista resolucio | NO |   
| dataFinalitzacio | Data finalitzacio | NO |   
| descripcioFase | Descripcio fase | NO |   
| estat | Estat | NO (OBERT,FINALITZAT) |   
| obervacions | Observacions | NO |   
| numeroRegistre | Número de registre | NO |   
| dataRegistre | Data registre | NO |   
| actuacioCiutada | NO (SI, NO, NO_APLICA) |   
| fase | Fase | NO |   
  
  
## Consultes   

El **hubcarpetes** farà les següents crides als sistemes integrats per consultar les dades necessàries.  
  
### Consulta detallada  
    
#### Actuacio  

Retorna el detall d'una actuacio única a partir del seu identificador

##### Peticio

` GET /consultaActuacioDetallada?{identificador,codiINE10,codiDIR3Organisme}`  
  
##### Descripcio camps 

|  Parametre | Obligatori |  
|---|---|  
|identificador|SI|  
|codiINE10|NO|
|codiDir3Organisme|NO|

##### Exemple peticio

` GET /consultaActuacioDetallada?identificador=3fh54h6hfh4h43jd24354`  
    
##### Exemple resposta 

```json   
{
   "codiINE10":"703",
   "tipusPersona":"INTERESSAT",
   "tipus":"ENTRADA",
   "identificacio":"abc768",
   "dataRegistre":1604401681121,
   "assumpte":"Assumpte...",
   "url":"www.abc.com/28"
}
 ```  
    
#### Actuacions

Retorna el detall d'una o més actuacions  
  
##### Peticio  

` GET /consultaActuacionsDetallada?{documentIdentificador,codiINE10,codiDIR3Organisme,dataInici,dataFi}`  
  
##### Descripcio camps   

|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| tipus | NO |   
  
  
##### Exemple peticio  

` GET /consultaActuacionsDetallada?documentIdentificador=12345678A&codiINE10=987645&dataInici=2020-10-02&dataFi=2021-10-02&tipus=ENTRADA` 
    
##### Exemple resposta 

```json  
{
   "identificador":"id_43",
   "assumpte":"assumpte...",
   "codiINE10": "2343"
   "codiDIR3Organisme":"789",
   "procediment":"procediment...",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":1604401681254,
   "descripcioFase":"Descripcio...",
   "estat":"FINALITZAT",
   "obervacions":"Observacions...",
   "numeroRegistre":"NUM_43",
   "dataRegistre":1604401681254,
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase"
}
``` 
   
#### Expedient  

Retorna el detall d'un expedient únic a partir del seu identificador

##### Peticio  

` GET /consultaExpedientDetallada?{identificador, codiINE10, codiDIR3Organisme}`  
  
##### Descripcio camps   

|   Parametre  |  Obligatori  |   
| --- | --- |   
| identificador | SI |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
  
##### Exemple peticio

` GET /consultaExpedientDetallada?identificador=54g657h243k234h`  
  
##### Exemple resposta 

```json  
{
   "identificador":"id_43",
   "assumpte":"assumpte...",
   "codiINE10":"827",
   "codiDIR3Organisme":"789",
   "procediment":"procediment...",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "tipusPersona":"INTERESSAT",
   "familia":"F1",
   "dataPrevistaResolucio":1604401681254,
   "descripcioFase":"Descripcio...",
   "estat":"FINALITZAT",
   "obervacions":"Observacions...",
   "numeroRegistre":"NUM_43",
   "dataRegistre":1604401681254,
   "actuacioCiutada":"NO_APLICA",
   "fase":"fase"
}
```  
  
#### Expedients  

Retorna el detall d'un o més expedients  
  
##### Perticio  

` GET /consultaExpedientsDetallada?{documentIdentificador,codiINE10,codiDIR3Organisme,dataInici,dataFi,estat}`  
  
##### Descripcio camps   

|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
| dataInici | NO |   
| dataFi | NO |   
| estat | NO (OBERT,TANCAT) |   
   
##### Exemple peticio  

` GET /consultaExpedientsDetallada?documentIdentificador=34867564R&codiINE10=2345654&dataInici=2020-10-21&dataFi=2021-10-21&estat=OBERT}`  

##### Exemple resposta 

```json  
{
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripcio",
   "expedients":[
      {
         "identificador":"id_65",
         "assumpte":"assumpte...",
         "codiDIR3Organisme":"973",
         "procediment":"procediment...",
         "dataInici":1604401681254,
         "url":"www.test.com",
         "tipusPersona":"REPRESENTANT",
         "familia":"F1",
         "dataPrevistaResolucio":1604401681254,
         "descripcioFase":"Descripcio...",
         "estat":"OBERT",
         "obervacions":"Observacions...",
         "numeroRegistre":"NUM_43",
         "dataRegistre":1604401681254,
         "actuacioCiutada":"NO_APLICA",
         "fase":"fase"
      },
      {
         "identificador":"id_82",
         "assumpte":"assumpte...",
         "codiDIR3Organisme":"338",
         "procediment":"procediment...",
         "dataInici":1604401681254,
         "url":"www.test.com",
         "tipusPersona":"REPRESENTANT",
         "estat":"OBERT"
      },
      {
         "..."
      }
   ]
}  
```  

### Consulta agrupada  

Retorna un resum quantitatiu de les actuacions i/o expedients donat un document identificador. En el cas de les actuacions agrupades segons el tipus, entrada o sortida. I en el cas dels expedients, agrupats per estat.  
  
#### Peticio

` GET /consultaAgrupada?{documentIdentificador,codiINE10,codiDIR3}`  
  
#### Descripcio camps   

|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificador | SI |   
| codiINE10 | NO |   
| codiDIR3Organisme | NO |   
  
#### Exemple peticio   

` GET /consultaAgrupada?documentIdentificador=98906049A&codiINE10=34668342`  
  
#### Exemple resposta 

```json  
 {
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripcio resultat",
   "documentIdentificador":"98906049A",   
   "agrupacions":[
      {
         "codiINE10":"34668342", 
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
            "expedientsFinalitzats":{
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
 
