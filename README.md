# PdfGenerator
Ja, jag vet. Readme-fil är helt oformaterad men det kändes inte som det viktiga här med tanke på tiden.
Annars är det så klart viktigt om man sysslar med kod som ska leva vidare att det inte bara är den som skrivit koden som ska förstå den.

Denna lösning är inte körbar även om det finns lite kod som jag tänker kan visa lite hur jag resonerat.

En API-lösning med köer i molnet känns väl som en bra lösning som också kan hantera större mängder trafik.

I Document Service ska vi ju ha följande enligt spec
string customerNumber, kanske även kalla ssn även om jag hellre hade haft ett PersonId som Guid eftersom pesonnummer faktiskt kan bytas och inte nödvändigtvis är unikt.
Guid DocumentNumber
string documentText, som så klart skulle kunna vara ett mer fancy objekt i form av Document eller så

Ett annat sätt att få fram ett dokument skulle ju kunna vara annars att ha färdiga mallar i xpd-format som sedan populeras med xml-data men det hänger på att alla dokuemnt av samma typ, dvs alla dokument för user sign up är ganska lika. Xml-data skulle då kunna vara i sin enklaste form personnummer och dokumenttext. Denna xml-fil skulle kunna skickas till generatePdf(formId, xmlData) där formId anger vilket dokuemnt, dvs vilken mall. Nackdel är att man får använda sig av Adobes verktyg och det kostar för varje pdf. Fördel är stabilt och snabbt samt kan klaga enorma mängder. Sen kanske man inte vill ha samma setup som myndigheter. :)

Här har vi antagit

Flödet skulle gå som följer
Document Service håller koll på beställningar som jag tolkat det. Där lagrar vi beställningar med vilken kund, vilket dokuemnt och vilket innehåll.
Eftersom vi inte vet belastningen är det klokt att ha meddelandehanterare här, Azure Service Bus eller liknande, och vi lägger ett meddelande på bussen/kön som Pdf Service plockar upp och genererar pdf-fil (Se lite kod i exempel). Den i sin tur skickar ett meddelande till storage service som på samma sätt gör sitt jobb och meddelar response.

Om man har den här lösningen i molnet går det att skala och meddelandehantering gör att större mängder data kan hanteras. Visst kan det hamna på dead letter queue men det kan ju monitoreras.

### Personlig kommentar.
Det är ytterst ovanligt att börja från noll så här så mycket av det ni ser kanske verkar lite udda. Jag är medveten och det och kommer aldrig utge mig själv för att vara den bästa utvecklaren. Om det finns någon att diskutera med och befintlig kod att utgå ifrån anser jag att det finns mycket större möjligheter till att göra något som kommer vara bra och hålla i längden.
Docker har jag använt ytterst lite som också meddelat under intervju 1 och då bara kört containers som en del av större lösning. Därav ingen container här.
