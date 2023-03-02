# PdfGenerator
Ja, jag vet. Readme-fil är helt oformaterad men det kändes inte som det viktiga här med tanke på tiden.
Annars är det så klart viktigt om man sysslar med kod som ska leva vidare att det inte bara är den som skrivit koden som ska förstå den.

Denna lösning är inte körbar även om det finns lite kod som jag tänker kan visa lite hur jag resonerat. Koden är förmodligen inte ens byggbar men kan ses som diskussionsunderlag.

En API-lösning med köer i molnet känns väl som en bra lösning som också kan hantera större mängder trafik.

I Document Service ska vi ju ha följande enligt spec
string customerNumber, kanske även kalla ssn även om jag hellre hade haft ett PersonId som Guid eftersom pesonnummer faktiskt kan bytas och inte nödvändigtvis är unikt. Om jag byter kön så byter jag personnummer. Om jag dör och någon som är född på samma dag kommer till Sverige kan den teoretiskt få "mitt" personnummer. Personnummer ändras från xxxxxx-xxxx till xxxxxx+xxxx den dag du fyller 100. Dessutom inte bra med tanke på GDPR att skriva massa personnummer i loggen. Bättre då med unikt id som gör att man kan leta upp vilken person det handlar om.
Guid DocumentNumber
string documentText, som så klart skulle kunna vara ett mer fancy objekt i form av Document eller så.

Ett annat sätt att få fram ett dokument skulle ju kunna vara annars att ha färdiga mallar i xpd-format som sedan populeras med xml-data men det hänger på att alla dokument av samma typ, dvs alla dokument för user sign up är ganska lika. Xml-data skulle då kunna vara i sin enklaste form personnummer och dokumenttext. Denna xml-fil skulle kunna skickas till en metod som genererar en pdf med parametrar (formId, xmlData) där formId anger vilket formulär, dvs vilken mall och version. Nackdel är att man får använda sig av Adobes verktyg och det medför en kostnad. Fördel är stabilt och snabbt samt kan klaga enorma mängder. Sen kanske man inte vill ha samma setup som myndigheter. :) Dessutom bra för lagring. (se nedan)

Här har vi antagit att det finns en lösning för storage som vi implementerar.
Men när det gäller storage vore det smart att inte lagra alla möjligtvis miljontals Pdf-filer utan bara innehåll samt en referens till vilken mall och vilken version som använts. På så sätt kan vi generera en pdf on the fly när den behövs. Om du har x antal filer med text "Välkommen till SEB Gustav Rylander! Ditt kundnummer är 42." så skulle x antal pdf-filer kunna bli en stor mängd data medan om du bara lagrar vilken xpd-mall du använt samt innehållet som xml blir det en mindre mängd data att lagra, vilket så klart är bra. Du slipper lagra samma data flera gånger. 

Flödet skulle gå som följer
Document Service håller koll på beställningar som jag tolkat det. Där lagrar vi beställningar med vilken kund, vilket dokument och vilket innehåll.
Eftersom vi inte vet belastningen är det klokt att ha meddelandehanterare här, Azure Service Bus eller liknande, och vi lägger ett meddelande på bussen/kön som Pdf Service plockar upp och genererar pdf-fil (Se lite kod i exempel). Den i sin tur skickar ett meddelande till storage service som på samma sätt gör sitt jobb och meddelar response.

Om man har den här lösningen i molnet går det att skala och meddelandehantering gör att större mängder data kan hanteras. Visst kan det hamna på dead letter queue men det kan ju monitoreras.

Loggning och felhantering ska så klart också finnas. Loggning antar jag redan finns så allt ska självklart loggas enligt gällande standard.
Självklart ska vi ha validering också och skicka BadRequest om inte anropet är ok.
När det gäller felhantering och kasta exception finns det också vinster att göra i att returnera Task<Result<bool>> istället för Task<bool> vad jag förstått och hantera exception i Result istället för att kasta. Men det finns säkert mer att justera.

### Personlig kommentar.
Då det är ytterst ovanligt för mig att börja från noll så här så mycket av det ni ser kanske verkar lite udda. Jag är medveten om det. Om det finns någon att diskutera med och befintlig kod att utgå ifrån anser jag att det finns mycket större möjligheter till att göra något som kommer vara bra och hålla i längden.
Docker har jag använt ytterst lite som också meddelat under intervju 1 och då bara kört containers som en del av större lösning. Därav ingen container här.
