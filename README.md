# AOC Hub de carpetes ciutadanes
Projecte de documentació per als integradors del Hub de Carpetes Ciutadanes (HCC) del Consorci AOC. El HCC consultarà a cada ens integrat informació sobre actuacions (accions de relació entre la ciutadania i l'Administració entesos des d'un punt de vista ampli i tipificats per entrada o sortida) i expedients que hauran d'exposar a través d'una interfície REST que compleixi el model i les consultes descrites a continuació.

Com a primers passos, abans de les tasques d'integració, es recomana la lectura del [document de presentació](/presentacio_myg_20201105.pdf).

Per integrar-se al servei, els passos a fer són els següents:

A) Preproducció
1. Dur a terme el desenvolupament explicat en aquest repositori documental
2. Omplir el formulari de contacte del portal de Suport: https://suport-integradors.aoc.cat/hc/ca/requests/new indicant a l'assumpte "Integració en proves de El meu espai - Hub de Carpetes Ciutadanes". Afegint al mateix les **URLs** dels endpoints amb exemples de les consultes.
3. Un cop validades a preproducció les funcionalitats dels endpoints per part de l'AOC, fixar la data de sortida a producció i seguir els passos de B) Producció.

El HCC revisara la integració a l'entorn de proves (preproducció) per poder per validar la correcta integració al servei. Per fer correctament les comprovacions dels vostres endpoints a preproducció, podeu fer servir HTTPS però no afegir cap filtratge d'IP, ni autenticar el certificat client durant la fase de preproducció.

Per a les proves, si us plau **no** fer servir NIFs de persones reals. En concret, cal fer servir:
|       NIF      | Nom o Raó social | Cognom1 | Cognom2 | Casos d'ús |  
| ---------------| --- | --- | --- | --- |
| 99999972C | Ciutadania | Fictícia | PF | Persona física |
| 99999974E | Ciutadania | Fictícia | PFRPJ | Persona física representant de persona jurídica |
| A01111114 | PJurídica Ficticia PJ |  |  | Persona jurídica representada per 99999974E |

Per a validar els endpoints a preproducció cal:

A) 3 registres o més de NIF 99999972C + Tres reg. o més de NIF 99999974E representant de la persona jurídica A01111114.

B) 2 expedients o més de NIF 99999972C + Tres exp. o més de NIF 99999974E representant A01111114.

Per assegurar la qualitat de les proves a preproducció, és necessari que tant els assentaments registrals o actuacions com els expedients siguin els més reals possibles però sense contenir dades personals.

Un cop l'AOC tingui la informació necessària per consultar les dades, validarà que la integració funcioni correctament a preproducció.

B) Producció
1. Cal securitzar els endpoints exposats.
2. Presentar un nou formulari de contacte del portal de Suport: [https://suport-integradors.aoc.cat/hc/ca/requests/new](https://suport-integradors.aoc.cat/hc/ca/requests/new) annexant [el document de sol·licitud d’integració](https://github.com/ConsorciAOC/Integracio-Serveis/raw/main/documentAlta/formulari_sol-licitud_integracio_serveis_caoc.pdf) signat. Indicant a l'assumpte "Integració en producció de El meu espai - Hub de Carpetes Ciutadanes" i determinant al cos del formulari de suport les URLS definitives de les consultes.

Per a la sortida a producció, cal securitzar els endpoints exposats:
- Comunicacions via HTTPS.
- Habilitar les IPs per les quals arribarà el CAOC als serveis web oferts: `157.97.64.126` i `157.97.65.88`.
- Validar el [certificat client](https://www.aoc.cat/wp-content/uploads/2021/11/Serveis_Administracio_Electronica_CAOC.zip) que l'AOC presentarà al establir la connexió. El número de sèrie del certificat és 3256b8ec6a5b7db071a9ff174fc83ffb. En el cas que el vostre servidor d’aplicacions o similar, no admeti els certificats sha256 (corresponent a l’arrel de Sector Públic), llavors necessiteu afegir la clau pública del certificat final enlloc de les arrels, per a aquest cas, el podeu descarregar per a que l’afegiu al vostre truststore.

Un cop l'AOC tingui la informació necessària per consultar les dades, validarà que la integració funcioni correctament a producció.

El model de desenvolupament es concretarà en fases i podria variar durant el temps:

1. Primera fase de desenvolupament (Prioritzada)
    1. [Consulta d'actuacions](#1-consulta-dactuacions) (Prioritat alta)
    2. [Consulta d'expedients](#2-consulta-dexpedients) (Prioritat mitja)

2. Segona fase de desenvolupament (No prioritzada)
    1. Consulta agrupada
    2. Consulta detallada d'una actuació
    3. Consulta detallada d'un expedient

    Aquesta segona fase està pendent de ser concretada i es troba en estat d'esborrany.


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

Al construir les respostes JSON, cal no informar aquells camps opcionals on no es retorni cap valor o valor buit.

Incorrecte (s'informa el camp _cognomRepresentant_ com una cadena buida de caràcters):
```json
"actuacions": [
    {
        "idActuacio": "1",
        "tipusPersona": "INTERESSAT",
        "nomRepresentant": "Joan",
        "cognomRepresentant": "",
        "nomInteressat": "Pere",     
        
    }
```

Correcte:
```json
"actuacions": [
    {
        "idActuacio": "1",
        "tipusPersona": "INTERESSAT",
        "nomRepresentant": "Joan",
        "nomInteressat": "Pere",
        
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
  "fue":"false"
}
```  

#### Descripcio camps  

També trobareu la totalitat de paràmetres previstos actualment en el [model de dades de la resposta](/model_dades_mygov.xslx).
Els paràmetres segueixen una estructura determinada pel nom del mateix en minúscules i, opcionalment, l'àmbit: "parametre" i "parametreAmbit".

|       Paràmetre      | Descripció | Obligatori |  
| ----------------------| --- | --- |  
| idActuacio          | Identificador de l'actuació | SI |  
| tipusPersona           | Tipus de persona | SI (INTERESSAT, REPRESENTANT) |  
| codiINE10              | Codi INE10 de l'Ens emissor de la informació i que s'integra amb el hub de Carpetes ciutadanes de MyGov | SI |  
| codiDIR3Organisme      | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub de Carpetes ciutadanes de MyGov | NO |
| tipusActuacio          | Tipus d'actuació | SI (ENTRADA, SORTIDA)|  
| dataActuacio           | Data de l'actuació. Format ISO_8601 YYYY-MM-DDThh:mm:ss | SI |  
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
| codiDIR3Organisme | Codi DIR3 de l'Ens emissor de la informació i que s'integra amb el hub MyGov | NO|    
| procediment | Procediment |  SI |   
| dataIniciExpedient | Data inici. Format ISO_8601 YYYY-MM-DDThh:mm:ss| SI |   
| assumpteExpedient | Assumpte |  SI  |   
| urlExpedient | URL de la carpeta ciutadana de l'Ens. Quan el ciutadà vulgui consultar el detall de l'expedient farà clic en aquest enllaç | SI |   
| familia |  Família o materia sobre el qual tracta el procediment. | NO |   
| dataPrevistaResolucio | Data prevista resolució. Format ISO_8601 YYYY-MM-DD | NO |   
| dataFinalitzacioExpedient | Data finalització. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |   
| codiFase |  Codificació de la fase en que es troba la tramitació de l'expedient. Pendent de definir el model tancat de les fases de tramitació de l'expedient per part de l'AOC. | NO |
| descripcioFase | Descripció de la FASE de tramitació de l'expedient, d'acord a la terminologia interna que faci servir l'Ens | NO |   
| estatExpedient | Estat | NO (OBERT,TANCAT) |   
| observacionsExpedient | Observacions sobre l'expedient que l'Ens vulgui informar | NO |   
| idActuacio | Identificador de l'actuació  | NO |   
| dataActuacio | Data i hora del número de registre d'entrada o sortida que ha donat origen a l'expedient. Format ISO_8601 YYYY-MM-DDThh:mm:ss | NO |   
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
| codiResultat | Retorna el codi resultant de l'operació | SI (200, 301, 302, 400, 401, 403 ,404, 500, 504, 509) |   
| descripcioResultat | Retorna un string de detall de l'operació | SI |
| actuacions | Array de resposta de les actuacions | SI |

##### Taula de significats dels codis d'error

| Codi | Significat |
| --- | --- |
| 200 | OK. |
| 301 | L'URI del recurs sol·licitat ha estat canviat. Una nova URI serà rebuda a la resposta. |
| 302 | El recurs de la URI sol·licitada ha estat canviat temporalment. Nous canvis a la URI seran agregats en el futur. Per tant, la mateixa URI ha de ser utilitzada pel client en futures sol·licituds. |
| 400 | Sintaxi invàlida, no s'ha pogut interpretar la sol·licitud. | 
| 401 | Cal autenticació. Aquesta és similar a 403, però en aquest cas l'autenticació és possible. | 
| 403 | No té els permisos necessaris per a cert contingut, per la qual cosa el servidor rebutja atorgar una resposta apropiada. | 
| 404 | No s'ha pogut trobar el contingut sol·licitat. | 
| 500 | El servidor ha trobat una situació que no sap com gestionar-la. | 
| 504 | Timeout. No es pot obtenir una resposta a temps. | 
| 509 | Límit d'ample de banda excedit. | 

##### Taula de descriptors dels codis d'error

| Codi | Descriptors |
| --- | --- | 
| 200 | OK | 
| 301 | Nova URI: https://github.com/ConsorciAOC/HubCarpetes/edit/main/README.js (aquest és un exemple, caldria retornar la URI)| 
| 302 | El recurs de la URI sol·licitada ha estat canviat temporalment | 
| 400 | Sintaxi invàlida, no s'ha pogut interpretar la sol·licitud | 
| 401 | Cal autenticació | 
| 403 | No té els permisos necessaris | 
| 404 | No s'ha trobat cap informació | 
| 500 | El servidor ha trobat una situació que no sap com gestionar-la | 
| 504 | Temps d'espera excedit | 
| 509 | Ample de banda excedit | 

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
| codiResultat | Retorna el codi resultant de l'operació | SI (200, 301, 302, 400, 401, 403 ,404, 500, 504, 509) |   
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
### Fase 2

Donada la naturalesa de la metodologia àgil escollida pel desenvolupament del projecte, la fase 2 és troba pendent de definició.
Per tant, s'aconsella contactar amb la persona responsable del servei si es vol continuar amb les tasques d'integració de la fase 2.
