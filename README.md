# HubCarpetes
Projecte de documentacio per als integradors del hub de carpetes del Consorci AOC

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
   "id":"13576",
   "codiINE10":"658",
   "persones":[
      {
         "nom":"nom2",
         "cognoms":"cognoms2",
         "numeroIdentificatiu":"43434343A",
         "tipus":"REPRESENTANT"
      }
   ],
   "tipus":"ENTRADA",
   "identificacio":"abc371",
   "dataRegistre":1604401681121,
   "assumpte":"Assumpte...",
   "url":"www.abc.com/698",
   "viaPresentacio":"Via...",
   "numeroExpedient":"225",
   "procediment":"Procediment...",
   "observacions":"Obs...",
   "referenciaExterna":"Referencia....",
   "notificacio":{
      "canalPreferent":"COMPAREIXENCA_ELECTRONICA",
      "adreca":"adreca",
      "correuElectronic":"correu"
   },
   "observacionsInteressat":"Obs interessat"
}
```  

#### Descripcio camps  
|       Parametre      | Descripcio | Obligatori |  
| ----------------------| --- | --- |  
| id                     | identificador intern | ? |  
| codiINE10              | Codi INE10 | SI |  
| persones               | Llista de persones. Pot tenir 1 interessat o interessat i representant) | SI |  
| tipus                  | Tipus d'actuacio | SI (ENTRADA, SORTIDA)|  
| identificacio          | Identifiador únic | SI |  
| dataRegistre           | Data registre | SI |  
| assumpte               | Assumpte | SI |  
| url                    | URL | SI |  
| viaPresentacio         | Via presentacio | NO |  
| numeroExpedient        | Numero de l'expedient | NO |  
| procediment            | Procediment| NO |  
| observacions           | Observacions| NO |  
| referenciaExterna      | Referencia externa | NO |  
| notificacio            | Dades de notificacio | NO |  
| canalPreferent         | Canal preferent | SI (DIRECCIO_POSTAL, COMPAREIXENCA_ELECTRONICA, DIRECCIO_ELECTRONICA_HABILITADA) |  
| adreca                 | Adreça | ? |  
| correuElectronic       | Correu electrònic | ? |  
| observacionsInteressat | Observacions interessat | NO |  
    
### Expedient  

#### Exemple JSON
```json  
{
   "id":"377",
   "identificador":"id_43",
   "assumpte":"assumpte",
   "codiDIR3OrganResponsable":"827",
   "codiDIR3Organisme":"789",
   "procediment":"procediment",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "persones":[
      {
         "nom":"nom2",
         "cognoms":"cognoms2",
         "numeroIdentificatiu":"43434343A",
         "tipus":"INTERESSAT"
      }
   ],
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
| id |  identificador intern  | ? |   
| identificador | Identificador únic | SI |   
| assumpte | Assumpte |  SI  |   
| codiDIR3OrganResponsable | Codi DIR3 òrgan responsable | SI (ENTRADA, SORTIDA) |   
| codiDIR3Organisme | Codi DIR3 organisme | SI |   
| procediment | Procediment |  SI |   
| dataInici | Data inici | SI |   
| url | URL | SI |   
| familia |  Via presentacio  | NO |   
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
Retorna el detall d'una actuacio única 

##### Peticio
` GET /consultaActuacioDetallada?{identificacio}`  
  
##### Descripcio camps 
|  Parametre | Obligatori |  
|---|---|  
|identificacio|SI|  
  
##### Exemple peticio
` GET /consultaActuacioDetallada?identificacio=3fh54h6hfh4h43jd24354`  
    
##### Exemple resposta 
```json   
{
   "id":"25447",
   "codiINE10":"703",
   "persones":[
      {
         "nom":"nom3",
         "cognoms":"cognoms3",
         "numeroIdentificatiu":"00000000A",
         "tipus":"INTERESSAT"
      },
      {
         "nom":"nom1",
         "cognoms":"cognoms1",
         "numeroIdentificatiu":"12345678A",
         "tipus":"REPRESENTANT"
      }
   ],
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
` GET /consultaActuacionsDetallada?{documentIdentificatiu,codiINE10,dataInici,dataFi}`  
  
##### Descripcio camps   
|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificatiu | SI |   
| codiINE10 | NO |   
| codiDIR3 | NO |   
| dataInici | NO |   
| dataFi | NO |   
| tipus | NO |   
  
  
##### Exemple peticio  
` GET /consultaActuacionsDetallada?documentIdentificatiu=12345678A&codiINE10=987645&dataInici=2020-10-02&dataFi=2021-10-02&tipus=ENTRADA` 
    
##### Exemple resposta 
```json  
{
   "id":"377",
   "identificador":"id_43",
   "assumpte":"assumpte...",
   "codiDIR3OrganResponsable":"827",
   "codiDIR3Organisme":"789",
   "procediment":"procediment...",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "persones":[
      {
         "nom":"nom2",
         "cognoms":"cognoms2",
         "numeroIdentificatiu":"43434343A",
         "tipus":"INTERESSAT"
      }
   ],
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
Retorna el detall d'un expedient únic  

##### Peticio  
` GET /consultaExpedientDetallada?{identificador}`  
  
##### Descripcio camps   
|   Parametre  |  Obligatori  |   
| --- | --- |   
| identificador | SI |   
  
##### Exemple peticio
` GET /consultaExpedientDetallada?identificador=54g657h243k234h`  
  
##### Exemple resposta 
```json  
{
   "id":"377",
   "identificador":"id_43",
   "assumpte":"assumpte...",
   "codiDIR3OrganResponsable":"827",
   "codiDIR3Organisme":"789",
   "procediment":"procediment...",
   "dataInici":1604401681254,
   "url":"www.test.com",
   "persones":[
      {
         "nom":"nom2",
         "cognoms":"cognoms2",
         "numeroIdentificatiu":"43434343A",
         "tipus":"INTERESSAT"
      }
   ],
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
` GET /consultaExpedientsDetallada?{documentIdentificatiu,codiINE10,codiDIR3,dataInici,dataFi,estat}`  
  
##### Descripcio camps   
|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificatiu | SI |   
| codiINE10 | NO |   
| codiDIR3 | NO |   
| dataInici | NO |   
| dataFi | NO |   
| estat | NO (OBERT,TANCAT) |   
   
##### Exemple peticio  
` GET /consultaExpedientsDetallada?documentIdentificatiu=34867564R&codiINE10=2345654&dataInici=2020-10-21&dataFi=2021-10-21&estat=OBERT}`  

##### Exemple resposta  
```json  
{
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripcio",
   "expedients":[
      {
         "id":"151",
         "identificador":"id_65",
         "assumpte":"assumpte...",
         "codiDIR3OrganResponsable":"492",
         "codiDIR3Organisme":"973",
         "procediment":"procediment...",
         "dataInici":1604401681254,
         "url":"www.test.com",
         "persones":[
            {
               "nom":"nom2",
               "cognoms":"cognoms2",
               "numeroIdentificatiu":"43434343A",
               "tipus":"REPRESENTANT"
            }
         ],
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
         "id":"802",
         "identificador":"id_82",
         "assumpte":"assumpte...",
         "codiDIR3OrganResponsable":"733",
         "codiDIR3Organisme":"338",
         "procediment":"procediment...",
         "dataInici":1604401681254,
         "url":"www.test.com",
         "persones":[
            {
               "nom":"nom2",
               "cognoms":"cognoms2",
               "numeroIdentificatiu":"43434343A",
               "tipus":"REPRESENTANT"
            }
         ],
         "estat":"OBERT"
      },
      {
         "..."
      }
   ]
}  
```  

### Consulta agrupada  
Retorna un resum quantitatiu de les actuacions i/o expedients donat un document identificatiu. En el cas de les actuacions agrupades segons el tipus, entrada o sortida. I en el cas dels expedients, agrupats per estat.  
  
#### Peticio
` GET /consultaAgrupada?{documentIdentificatiu, codiINE10,codiDIR3}`  
  
#### Descripcio camps   
|   Parametre  |  Obligatori  |   
| --- | --- |   
| documentIdentificatiu | SI |   
| codiINE10 | NO |   
| codiDIR3 | NO |   
  
#### Exemple peticio   
` GET /consultaAgrupada?documentIdentificatiu=98906049A&codiINE10=34668342`  
  
#### Exemple resposta 
```json  
 {
   "codiResultat":"codiOK",
   "descripcioResultat":"Descripcio resultat",
   "valorDocumentIdentificacio":"98906049A",
   "tipusDocumentIdentificacio":"NIF",
   "agrupacions":[
      {
         "codiEns":"34668342",
         "tipusCodiEns":"INE10",
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
 
