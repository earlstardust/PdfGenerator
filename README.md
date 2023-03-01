# PdfGenerator
Ja, jag vet. Readme-fil är helt oformaterad men det kändes inte som det viktiga här med tanke på tiden.
Annars är det så klart viktigt om man sysslar med kod som ska leva vidare att det inte bara är den som skrivit koden som ska förstå den.

En API-lösning med köer i molnet känns väl som en bra lösning som också kan hantera större mängder trafik.

I Document Service ska vi ju ha följande enligt spec
string customerNumber, kanske även kalla ssn även om jag hellre hade haft ett PersonId som Guid eftersom pesonnummer faktiskt kan bytas och intenödvändigtvis är unikt.
Guid DocumentNumber
string documentText, som så klart skulle kunna vara ett mer fancy objekt i form av Document eller så

Ett annat sätt att få fram ett dokument skulle ju kunna vara annars att ha färdiga mallar i xpd-format som sedan populeras med xml-data men det hänger på att alla dokuemnt av samma typ, dvs alla dokument för user sign up är ganska lika. Xml-data skulle då kunna vara i sin enklaste form personnummer och dokumenttext. Denna xml-fil skulle kunna skickas till generatePdf(formId, xmlData) där formId anger vilket dokuemnt, dvs vilken mall. Nackdel är att man får använda sig av Adobes verktyg och det kostar för varje pdf. Fördel är stabilt och snabbt samt kan klaga enorma mängder. Sen kanske man inte vill ha samma setup som myndigheter. :)

Här har vi antagit
