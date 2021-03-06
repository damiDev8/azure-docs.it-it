---
title: Copertura della mobilità (transito) in Microsoft Azure Maps Mobility Services (anteprima)
description: Informazioni sul livello di copertura fornito dai servizi di mobilità di Azure Maps (anteprima) in quali aree per le funzionalità di transito pubbliche, ad esempio gli avvisi di routing e servizio.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e902f313edf22d75f6b183575c3dc8d0dd94bc1f
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2020
ms.locfileid: "96904755"
---
# <a name="azure-maps-mobility-services-preview-coverage"></a>Copertura dei servizi di mobilità di Azure Maps (anteprima)

> [!IMPORTANT]
> I servizi di mobilità di Azure Maps sono attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


I servizi di [mobilità](/rest/api/maps/mobility) di Azure Maps migliorano il tempo di sviluppo per le applicazioni con funzionalità di transito pubblico, ad esempio il routing di transito e la ricerca di interruzioni di transito pubblico vicine. Gli utenti possono recuperare informazioni dettagliate su fermate dei trasporti pubblici, linee e orari. I servizi Mobility consentono inoltre agli utenti di recuperare le geometrie di interruzione e di linea, gli avvisi per le interruzioni, le linee e le aree di servizio, nonché gli arrivi e gli avvisi del servizio in tempo reale. Inoltre, i servizi Mobility forniscono funzionalità di routing con opzioni di pianificazione dei viaggi multimodale. La pianificazione di itinerari multimodali incorpora le opzioni di itinerario a piedi, in bicicletta e con trasporti pubblici, in un unico itinerario. Gli utenti possono anche accedere a itinerari dettagliati multimodali.

Azure Maps non fornisce lo stesso livello di informazioni e accuratezza per tutte le città e i paesi o le aree geografiche. La possibilità di chiamare i dati di transito pubblici dipende dall'area metro. Inoltre, i dati della mappa potrebbero non includere tutte le opzioni di transito pubblico e le agenzie che svolgono l'area metropolitana.

La tabella seguente contiene informazioni sulla copertura per i servizi di mobilità di Azure maps.

| Simbolo | Significato |
|--------|---------|
| *      |Copertura quasi completa per il paese/area geografica.|

## <a name="americas"></a>Americhe

| Paese/Area geografica |  Città (area metro) |
|----------------|---------|
| Antigua e Barbuda | Antigua e Barbuda * |
| Argentina       | <p>Azul, Bahía Blanca, Buenos Aires, Caleta Olivia, Catamarca, Chivilcoy, Comodoro Rivadavia, Concordia, Cordova, Corrientes, generale Pico, Gualeguaychu, la Rioja, Mar del Plata, Mendoza, Miramar, Necochea, Neuquén, Oberá, Olavarría, Paraná, Posadas, Rafael, Rio Tercero, Rosario, salta, San Carlos de Bariloche, San Luis, San Miguel de Tucumán, San Pedro, Santa Fe, Tandil, Ushuaia, Victoria, Viedma, Villa María</p>|
| Barbados       |  Barbados |
| Brasile         | <p>Angra dos Reis, Anápolis, Apucarana, Aracaju, Araraquara, Araxa, Araçatuba, Atibaia, beni, Barretos, Bauru, Bebedouro, Belém, Belo Horizonte, Blumenau, Boa Vista, Botucatu, Brasilia, Caldas Novas, Campina Grande, Campinas, Campo Belo, campo grande, Caraguatatuba, Caratinga, Cascavel, Cataguases, Caxias, Leopoldina e Região, Catalão, Caxias, Chapecó, Cianorte, Conselheiro Lafaiete, Corumbá, Criciúma, Cruzeiro do sul, Cuiabá, Curitiba, Curitibanos, Curvelo, Diamantina, Divinópolis, Dourados, Estrela, Feira de Santana, Fernando de Noronha, Florianópolis, Fortaleza, Foz do Iguaçu, Franca, Garanhuns, Goiânia, Governador Valadares, Guarapuava, Imperatriz, Ipatinga, Irati, Itabira, Itabuna, Itajaí, Itajuba, Ituiutaba, Jaguarao, Jaraguá do sul, Joao Pessoa, Joinville, Juazeiro do Norte, Juiz de Fora, Jundiaí, Lages, Lavras e regiao, Lucas do Rio verde, Londrina, Macapa, Macaé, Maceió, Mafra e Rio Nero, Manaus, Manhuacu, Maringá, Marília, Monte Carmelo , Montes Claros, Mossoró, Natal, Osorio, Ourinhos, Ouro Preto, Palmas, Paracatu, Paranaguá, Parnaíba, passo fundo, Passos, Pato Branco, Patos de Minas, Patrocínio, Pelotas, Picos, Piracicaba, Pirapora, Pocos de Caldas, Ponta grosse, Porto Alegre, Porto Ferreira, Porto Seguro, Porto Velho, Praia Grande, Recife, Ribeirão Preto, Rio, Rio Branco, Rio verde, Rondonópolis, Salinas, Salvador, Santa Cruz do sul, Santa Maria, Santa Rita do Sapucaí, Santarem, Santiago del estero, Santos, Sao Gabriel do Oeste, Sao Joao del Rei, Tiradentes e regiao, Sao Jose do Rio Preto, Sao Mateus, Sao Paulo, Sorocaba, São Carlos, São Francisco do sul, São José dos Campos, São Lourenço, São Luís, Taubaté, Telemaco Borba, Teofilo Otoni, Teresina, Toledo, Três Lagoas, Tucurui, Ubatuba, Uberaba, Uberlândia, Ubá, uruguaiana, Varginha, Vicosa, Videira & Fraiburgo , Vitória, Vitória da conquista, volta Redonda, Votuporanga </p>|
| Canada | Banff (AB), Brandon (MB), Calgary (AB), Chatham-Kent (ON), Comox Valley (BC), Cowichan Valley (BC), Cranbrook (BC), Edmonton (AB), Fort St. John, Fredericton (NB), Greater Sudbury (ON), Greater Vancouver (BC), Halifax (NS), Kamloops (BC), Kelowna-Vernon (BC), Kingston (ON), Londra (ON), Moncton (NB), Montreal (QC), Nanaimo (BC), Ottawa (ON), Prince George (BC), Québec City (QC), Red Deer (AB), Regina (SK), Rimouski (QC), Saskatoon (SK), Sherbrooke (QC), Southwest British Columbia (BC), Squamish (BC), St. John ' s (NL), Sunshine Coast, Thunder Bay (ON), Toronto (ON), Victoria (BC), West Kootenay (BC), Whistler (BC), Windsor (ON), Winnipeg (MB), Woodstock</p>|
| Cile  | <p>Antofagasta, Arica, Aysén, Chillán, Concepción, Constitución, Copiapó, Curicó, Iquique, la Serena y Coquimbo, Linares, Los Angeles (Chile), Los Lagos, Punta Arenas, Rancagua, Santiago, Talca, Temuco, Valdivia, Valparaíso, Viña del Mar</p>|
| Colombia | <p>Barranquilla, Bogotá, Bucaramanga, Cali, Cartagena, Ibagué, Medellín, pasto, Popayán, Santa Marta, Sincelejo, Valledupar</p>|  
| Costa Rica | San José|
| Repubblica Dominicana | Santo Domingo |
| Ecuador | Cuenca, Guayaquil, Loja, Manta, Milagro|
| El Salvador | San Salvador |
 | Guatemala | Ciudad de Guatemala (GT) |
| Messico | Acapulco, Aguascalientes, Cancun, Durango, città del Messico, Guadalajara, Lion, Merida, Monterrey, Puebla, Puerto Vallarta, Querétaro, San Luis Potosi, Tijuana, Torreon|
| Nicaragua | Managua |
| Panama | Panama|
| Perù | Cusco, Lima |
| Portorico | San Juan |
| Suriname | Paramaribo |
| Uruguay | Montevideo, Paysandu, punta del Este, salto |
| Stati Uniti d'America | <p>Albany (GA), Albany (NY), Albuquerque (NM), Anchorage (AK), Ann Arbor (MI) Appleton-Oshkosh-Neenah (WI), Asheville (NC), Atene (GA), Atene (OH), Atlanta (GA), Austin (TX), Bakersfield (CA), Baltimore (MD), Bend-Redmond (o), Berkshire County (MA), Birmingham (AL), Bloomington (IN), Boise (ID), Boston (MA), Boulder (CO), Bowling Green (KY), Brevard County (FL), Buffalo (NY), Butte (MT), Cap Cod (MA), County Center (PA), Champaign-Urbana (IL), Charleston (SC), Charleston (WV), Charlotte (NC), Charlottesville (VA), Chattanooga (TN), Cheyenne (WY), Chicago (IL), Cincinnati (OH), Citrus County (FL), Cleveland (OH), Coachella Valley (CA), Colorado Springs (CO), Columbia (TN), Columbia (SC), Columbus (OH), Corpus Christi (TX), Dallas/Forth Worth (TX), Dayton (OH), Delaware, Denver (CO), Des Moines (IA), Detroit (MI), Duluth (MN), El Paso (TX), Eugene (o), Fairbanks (AK), Fargo (ND) , Fayetteville (NC), Flagstaff (AZ), Flint (MI) Fort Collins (CO), Fort Wayne (IN), Fresno (CA), Gainesville (FL), Grand fork (ND), Grand Rapids (MI), Green Bay (WI), Greensboro (NC), Greenville (SC), Gunnison (CO), Hampton Roads (VA), Hanford (CA), Hartford (CT), Hernando County (FL), Hinesville (GA), Honolulu (HI), Houston (TX), Humboldt County (CA), Huntsville (AL), Indianapolis (IN), Itaca (NY), Jackson (MS), Jackson (TN), Jacksonville-St. John ' s County (FL), Johnson City (TN), Jonesboro (AR), Joplin (MO), Juneau (AK), Kalamazoo (MI), Kalispell (MT), Kansas City (MO), Kauai (HI), Ketchum (ID), Knoxville (TN), Lafayette (IN), Lancaster (PA), Lansing (MI), Laredo (TX), Las Vegas (NV), Lawrence (KS), Lee County (FL), Lexington (KY), Lincoln County (o), Little Rock (AR), Los Angeles (CA), Louisville (KY), Lubbock (TX), Madison (WI), Manchester (NH) , McAllen (TX), Memphis (TN), Miami (FL), Milwaukee-Waukesha (WI), Minneapolis-St. Paul (MN), Missoula (MT), modesto (USA), Moline (IL), Monroe County (PA), Montgomery (AL), Morgantown (WV), Nashville (TN), Navajo Nation), New Haven (CT), New Orleans (LA), NYC-NJ area (NY), Ocala (FL), Okaloosa County (FL), Oklahoma City (OK), Omaha (NE), Orlando (FL), Palm Desert (CA), Panama City (FL), Pensacola (FL), Peoria (IL), Philadelphia (PA), Phoenix (AZ), Pittsburgh (PA), Portland (ME), Portland (OR), Racine (WI), Raleigh (NC), Redding (CA), Reno & Lake Tahoe (NV), Richmond (VA), Roanoke Valley (VA-Lynchburg), Rochester (NY), Rockford (IL), Rocky Mount (NC), Rocky Mountain National Park (CO), Valle inaffidabile (o), Roseburg (o), Roseville (CA), Sacramento (CA), Salem (OR), Salt Lake City (UT), San Antonio (TX), San Diego (CA), San Luis Obispo (CA), Santa Barbara (CA), Santa Fe (NM), Sarasota (FL) , Savannah (GA), litorale Region (NH), Seattle-Tacoma-Bellevue (WA), SF Bay Area (CA), SF-San Jose Area (CA), Sioux City (IA), Sioux Falls (SD), Sitka (AK), Spokane (WA), Springfield (MA), South Bend (IN), Springfield (IL), Springfield (Mass), St. George (UT), St. Louis (MO), Stockton (CA), Syracuse-Utica (NY), Tallahassee (FL), Tampa-St. Pietroburgo (FL), Terre Haute (IN), Toledo (OH), Topeka (KS), attraversamento città (MI), Tucson (AZ), Tulsa (OK), Vermont, Victorville (CA), Volusia County (FL), Waco (TX), Washington (DC), Waterbury (CT), Wichita (KS), Wichita Falls (TX) Wilmington (NC), Yakima (WA), Youngstown (OH), York County (PA), Yuma County (AZ)</p>|
| + Isole Vergini americane | Isole Vergini statunitensi * |
| Venezuela | Caracas |

## <a name="asia-pacific"></a>Asia Pacifico

| Paese/Area geografica |  Città (area metro) |
|--------|---------|
| Australia | <p>Adelaide, Alice Springs, Bowen, Brisbane, Bundaberg QLD, Burnie, Cairns, Canberra, Darwin, Gladstone, Hobart, Innisfail, Launceston, Mackay, Magnetic Island, Maryborough-Hervey Bay, Melbourne, New South Wales, Perth, RockHampton, South East Queensland, Sydney, Toowoomba, Townsville, Victoria, Warwick, Yeppoon</p> |
| Brunei | Bandar Seri Begawan |
| Cina | <p> Changchun, Changsha, Chengdu, Chongqing, Dalian, Datong, Dongguan, Hangzhou, Harbin, Jiangyin, Jinan, Nanjing, Nantong, Ningbo, Pingdingshan, Qingdao, Shenyang, Suzhou, Tangshan, Tianjin, Weifang, Wuhan, Wuxi, Yantai, Yixing, Zhuhai, Shanghai, Beijing, Guangzhou, Shenzhen, Zhengzhou</P>|
| RAS di Hong Kong | RAS di Hong Kong *|
| RAS di Macao | RAS di Macao *|
| Maldive | Male |
| India | Ahmedabad, Bengaluru, Delhi, Hyderabad, Mumbai, Mysuru, Pune|
| Indonesia | Bandung, Banjarmasin, Banyuwangi, Batam, Denpasar, Jakarta, Kediri, Malang, Palembang, Semarang, Surabaya, Surakarta, Yogyakarta |
| Giappone | Hokkaido, Prefettura di Shizuoka, Tokyo, Wakkanai, Prefettura di Yamanashi |
| Malaysia | Ipoh, Johar Bahru, Kuala Lumpur, Kuantan, Penang |
| Nuova Zelanda | Auckland, Christchurch, Dunedin, Queenstown, Timaru, Wellington|
| Filippine | Manila |
| Singapore | Singapore |
| Corea del Sud | Busan, Seoul |
| Taiwan | Changhua County, Taipei |
| Thailandia | Bangkok, Chiang mai |
| Uzbekistan | Samarcanda |
| Vietnam | Hanoi, città di Ho Chi Minh |

## <a name="europe"></a>Europa

| Paese/Area geografica |  Città (area metro) |
|----------------|---------|
| Andorra        | Andorra la Vella |
| Austria        | Vienna |
| Bielorussia        | Gomel, Grodno, Polotsk & Novopolotsk, Zhlobin, Vileyka, Maladziečna, Minsk, Rechytsa |
| Belgio        | Belgio |
| Bolivia        | La Paz, Santa Cruz de la Sierra |
| Bosnia ed Erzegovina | Sarajevo |
| Bulgaria       | <p>Balchik, Blagoevgrad, Burgas, Dobrich, Gabrovo, Haskovo, Kardzhali, Lovech, Nessebar, Pazardzhik, Pernik, Pleven, Plovdiv, Ruse, Shumen, Sliven, Stara Zagora, Vratsa, Yambol, Varna, Veliko, Sofia</P> |
| Croazia | Crikvenica, Dubrovnik, Rijeka, Slovanski Brod, Zagabria |
| Cipro | Larnaca, Limassol, Nicosia |
| Repubblica Ceca | Brno, Jablonec, Karlovy Vary, Liberec, Ostrava, Praga |
| Danimarca   | Danimarca |
| Estonia   | Estonia |
| Finlandia   | Hämeenlinna, Helsinki, Joensuu, Jyväskylä, Kajaani, Kouvola-Kotka, Kuopio, Lappeenranta, Mikkeli, Oulu, pori, Rovaniemi, Seinäjoki, Tampere, Turku, Vaasa|
| Francia    | <p>Amberieu-en-Bugey, Amiens, Angers, Annecy, Annonay, Arras, Aubenas, Bayonne, Besançon, Blois, Bordeaux, Boulogne sur Mer, Brest, Briançon, Cannes, Châlons-en-Champagne, Chartres, Clermont-Ferrand, Colmar, Côte Azzurra, Dax, Dijon, Grenoble, Haguenau, la Rochelle, le Mans, Lens, Lille, Lorient, Lione, MACS, Marsiglia & Provenza, Metz, Millau, Mont-de-Marsan, Montpellier, Mulhouse, Nancy, Nantes, Nice, Nice Côte Azzurra, Nimes, Normandia, Nyons, Parigi, Poitiers, privi di Quimper, Rennes, Saint Malo, Saint-Étienne, Saint-Nazaire, Saintes, Sarrebourg, sete, Strasburgo, Tarbes, Tolosa, Tours</P> |
| + Guyana francese | Cayenne |
| + Nuova Caledonia | Nouméa  |
| Georgia | Tbilisi |
| Germania | <p>Berlino, Brandenburg, Brema & Niedersachsen, Colonia, Eisenach, Francoforte, Amburgo, Karlsruhe, Mainz, München-Munich, area Rhein-Neckar, Rhein-Ruhr Region, Stoccarda, Titisee-Neustadt, Ulm</P> |
| Grecia | <p>Agrinio, Aigio, Athens, Arta, Amorgos, Chania, Corfù, Chios Kos, Heraklion, Ioannina, Kavala, Kalamata, Khios, Komotini, Kos, Larissa, Meganisi, Milos, Mykonos, Patra, Rethimno, Rhodes, Santorini, Serres, Syros, Tinos, Salonicco, Veria, Volos, Xanthi </P> |
| Ungheria | Budapest, Gyor, Miskolc, Nograd County, PESC, Szeged, Székesfehérvár |
| Islanda | Ísland-Islanda * |
| Irlanda | Irlanda |
| Italia   | <p>Agrigento, Alessandria, Ancona, Bari, Bologna-Bologna, Cagliari-Sardegna, Campobasso, Catania e Messina, Cosenza, crema, Cremona, Crotone, Cuneo, Firenze, Firenze, Foggia, Genova, Genova, Iglesias, la Spezia, Lecce, Matera, Milano, Milano, Napoli-Napoli, Padova, Palermo, Parma, Perugia, Pescara, Pisa, potenza, Roma-Roma, Siena e Grosseto, Siracusa-Siracusa, Taranto, Torino-Torino, Trento, Trieste, Udine, Venezia-Venezia </p> |
| Lettonia | Rīga |
| Liechtenstein | Liechtenstein |
| Lituania | Druskininkai, Kauno, Klaipėda, Panevėžys, Vilnius |
| Lussemburgo | Lussemburgo |
| Moldova | Chişinău |
| Montenegro | Podgorica |
| Paesi Bassi | Paesi Bassi |
| Norvegia | Norvegia |
| Polonia | <p>Breslavia, Białystok, Bydgoszcz, Elbląg, Elk, Gorzow, Kętrzyna, Cracovia, Leszno, Lodz, Lublino, Mrągowo, Olsztyn, Poznań, Rzeszów, Sanok, Starachowice, Świonujście, Szczecin, Tricity, Varsavia, Wodzisław Śląski, Breslavia, Zakopane</p> |
| Portogallo | Bragança, Coimbra, Funchal, Leiria, Lisboa, Portimao, Porto|
| Malta | Malta |
| Romania | <p>Alba Iulia, Arad, Bistrița, Brăila, Braşov, Bucarest, Buzau, Cluj Napoca, Costanza, Craiova, Deva, Focșani, Galati, Iaşi, Miercurea Ciuc, Oradea, Piatra Neamt, Pitești, Ploieşti, Reșița, Satu Mare, Sibiu, Suceava, Targu Mures, Timisoara, Tulcea, Zala</p> | 
| Russia  | Kaliningrad, Rostov-on-Don, Volgograd, Ekaterinburg, Kazan, Kirov, Krasnodar, Mosca, Nalchik, Nizhny Novgorod, Novosibirsk, Nojabr ' sk, Omsk, Oryol, Perm, St Petersburg, Pyatigorsk, Tver, Tomsk |
| Serbia  | Beograd, Kragujevac, NIS, Novi Sad, Valjevo, Subotica | 
| Slovacchia | Banská Bystrica, Bratislava, Kosice, Presov, Prievidza, Ruzomberok a Liptovsky Mikulas, Stará Ľubovňa, Trencin |
| Slovenia | Koper, Lubiana |
| Spagna    | <p>Albacete, cocolaa, Alicante, Almería, Asturias, Avila, Badajoz, Bay of Cadice, Barcellona, Bilbao, Burgos, Caceres, Campo de Gibilterra, Castellon de la Plana, Ceuta, Ciudad Real, Cordova, Cuenca, El Hierro, Ferrol, Fuerteventura, Gran Canaria, Granada, Huelva, Huesca, Ibiza, Jaén-Úbeda, la Gomera, la Palma, Lanzarote, León, Lleida, Logroño, Lugo, Madrid, Malaga, Mallorca-Maiorca, Melilla, Minorca, Merida, Murcia, Ourense, Palencia, Pamplona, Salamanca, San Sebastian , Santander, Santiago de Compostela, Segovia, Siviglia, Soria, Tarragona-Reus, Tenerife, Toledo, Valencia, Valladolid, Vigo, Vitoria-Gasteiz, Zaragoza-Saragozza</p> |
| Svezia | Goteborg/Gothenburg/Jonkoping, Malmö kommun-Malmö, Norrköping och Linköping, Stoccolma, Sundsvall |
| Svizzera | Basilea, Ginevra, Yverdon-les-Bains, Zurigo | 
| Turchia | Adana-Mersin, Ankara, Antalya, Balıkesir, Bilecik, Çorum, Bursa,, Denizli, Duzce, Edirne, Elazig, Eskisehir, Istanbul, Izmir-Aydin, Kahramanmaras, Kayseri, Konya, Malatya, Muğla, Samsun, Şanlıurfa, Trabzon |
| Regno Unito | East Anglia, East Midlands, Londra e sud-est, nord-orientale, nord-ovest, Irlanda del Nord, Scozia, sud-ovest, Galles, Midlands occidentali, Yorkshire |
| Ucraina | Kharkiv, Zhytomyr, Kiev, Lviv, Chernivtsi |

## <a name="middle-east-and-africa"></a>Medio Oriente e Africa

| Paese/Area geografica |  Città (area metro) |
|---------|---------|
| Bahrein | Bahrain |
| Burkina Faso | Ouagadougou |
| Congo | Kinshasa |
| Egitto | Cairo    |
| Israele| Israele  |
| Kenya | Nairobi  |
| Madagascar | Antananarivo |
| Marocco | Casablanca, Essaouira, Khouribga, Tétouan|
| Qatar| Doha|
| Arabia Saudita | Thuwal |
| Senegal | Dakar |
| Sudafrica | Città del Capo |
| Tunisia | Kairouan |
| Emirati Arabi Uniti  | Abu Dhabi, Dubai |


## <a name="next-steps"></a>Passaggi successivi

Informazioni su come richiedere i dati di transito usando i servizi Mobility (anteprima):

> [!div class="nextstepaction"]
> [Come richiedere dati di transito](how-to-request-transit-data.md)

Informazioni su come richiedere dati in tempo reale usando i servizi Mobility (anteprima):

> [!div class="nextstepaction"]
> [Come richiedere dati in tempo reale](how-to-request-real-time-data.md)

Esplorare la documentazione dell'API servizi di mobilità di Azure Maps (anteprima)

> [!div class="nextstepaction"]
> [Documentazione dell'API dei servizi Mobility](/rest/api/maps/mobility)
