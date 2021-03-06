---
title: Batch logging
description: Hente ut feilmeldinger fra behandling av batch jobber
weight: 800
---

### BatchLoggingAgencyExternal

Denne tjenesten gir tjenesteeier tilgang til feil som har forekommet under behandling av batch jobber. Som en del av denne tjeneste-beskrivelsen vil følgende stikkord bli brukt:

 - DataBatch - referer til en batch fil levert til altinn av tjenesteeier.
 - DataItem - en Xml entitet i DataBatch. I en Correspondence fil vil for eksempel en enkel Correspondence være et DataItem.
 - Issue - en feil, warning eller informasjons melding som forekom under behandling av DataBatch og DataItem.

##### GetStatusOverview

Denne operasjonen gir en oversikt over DataBatcher som er blitt behandlet, og antall Issues som er opplevd under behandling.

|**Input**|**Beskrivelse**|
|--------|--------|
|BatchLoggingRequest|Angir søke-kriterier for DataBatcher og DataItems|
|**Returverdi**|**Beskrivelse**|
|BatchLoggingResult|Resultat for søke-kriterier|
|**Property**|**Beskrivelse**|
||**BatchLoggingRequest**|
|SystemUserCode|Angir hvilken tjenesteeier requesten gjelder for. SystemUserCode er spesifikk for en gitt ShipmentDefinition, og er vanligvis angitt i filen som er blitt levert til Altinn|
|DataBatchType?|Angir hva slags type Batch som ønskes returnert. Mulige verdier er: ActivateSubscription – Aktivering av en eksisterende subscription, Notification – Utsending av varsel, Correspondence – Innlesing av correspondence, Prefill – Innlesing av prefill, RgisterDSFProperty – Innlesing av DSF property, RegisterDSFPropertyAdd – Innlesing av DSF property husnumre, RegisterDSFStreet – Innlesing av DSF gate, RegisterDSFStreetAdd – Innlesing av DSF Gate husnumre, RegisterDSFUser – Innlesing av DSF person, RegisterER – Innlesing av organisasjons-data, Subscription – Innlesing av Subscription data|
|BatchLoggingDateTimeRequest|Angir en Til og Fra dato hvor fil-innlesing blir gjort|
|FileName|Angir filnavn på DataBatch. Må være eksakt fullt filnavn. (Correspondence.xml må altså angis som «Correspondence.xml»|
|DataBatchId?|Angir en spesifikk DataBatchId å returnere data for|
|Sequence?|Angir et spesifikt sekvensnummer å returnere data for. Sequence blir satt i hver fil som leveres til Altinn|
|FromIssueId?|Begynn å returnere Issues fra denne IssueId. Det leveres aldri mer enn 10.000 issues fra et enkelt kall, så i tilfelle hvor mer enn 10.000 issues eksisterer vil resterende issues kunne returneres ved bruk av denne verdien|
||**BatchLoggingStatusOverview**|
|DataBatches (List<DataBatch>)|Liste av DataBatcher funnet med søke-kriterier|
|NumberOfIssues|Totalt antall Issues som er blitt returnert|
||**DataBatch**|
|StartDate|Når behandling av DataBatch startet|
|EndDate|Når DataBatch var ferdig behandlet|
|DataBatchId|Unik identifikator for DataBatch|
|Sequence|Sekvensnummer angitt i DataBatch|
|ShipmentId|Identifikator for Shipment opprettet for DataBatch|
|FileName|Filnavn for DataBatch|
|NumberOfIssues|Antall feil opplevd for DataBatch|

Tabellen under angir feilmeldinger.

|**Feilkode**|**Beskrivelse**|
|--------|--------|
|31033|BatchLoggingRequest var ikke riktig angitt|

##### GetDetailedStatus

Denne operasjonen gir en oversikt over DataBatcher som er blitt behandlet, med tilhørende Issue og metadata for DataItem.

|**Input**|**Beskrivelse**|
|--------|--------|
|BatchLoggingRequest|Angir søke-kriterier for DataBatcher og DataItems|
|**Returverdi**|**Beskrivelse**|
|BatchLoggingResult|Resultat for søke-kriterier|
|**Property**|**Beskrivelse**|
||**BatchLoggingDetailedStatus**|
|DataBatches (List DataBatch)|Liste av DataBatcher funnet med søke-kriterier|
|DataItems (List DataItem)|Liste med metadata for DataItems som har opplevd Issues under behandling|
|Issues (List Issue)|Liste med feil opplevt under DataBatch behandling|
|NumberOfIssues|Totalt antall Issues som er blitt returnert|
||**DataItem**|
|DataItemId|Unik identifikator for DataItem|
|DataBatchId|Unik identifikator for DataBatch som DataItem hører til.|
|Line|Linjenummer for DataItem i fil|
|Position|Posisjon for DataItem i fil|
|Data|Rå Xml data for DataItem. Returneres kun i GetDataItem (9.18.3)|
|State|Status på DataItem|
|ServiceCode|Tjenestekode for DataItem|
|ServiceEditionCode|Tjenesteversjonskode for DataItem|
||**Issue**|
|DataItemId?|DataItem for Issue|
|DataBatchId|DataBach for Issue|
|ErrorDateTime|Når Issue oppstod|
|IssueType|Typen Issue som er opplevd|
||**IssueType**|
|Code|ErrorCode for IssueType|
|Description|Feilmelding for IssueType|
|Level|Hvor alvorlig Issuet var. Verdier som kan avgis er: Other, Information, Warning, Error, Critical|

##### GetDataItem

Denne operasjonen returnerer metadata og rå Xml data for en spesifikk DataItem (Xml-entitet) som har feilet under behandling.

|**Input**|**Beskrivelse**|
|--------|--------|
|dataItemId|Spesifikk identifikator for et DataItem|
|DataItem|Inneholder metadata og rå Xml data for et DataItem|