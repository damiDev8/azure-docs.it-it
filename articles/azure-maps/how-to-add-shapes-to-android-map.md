---
title: Aggiungere un livello poligono a Maps Android | Mappe Microsoft Azure
description: Informazioni su come aggiungere poligoni o cerchi a maps. Scopri come usare le mappe di Azure Android SDK per personalizzare le forme geometriche e semplificarne l'aggiornamento e la gestione.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1712cedab9cef23108fcc48b8e09bdc3e33065c4
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679472"
---
# <a name="add-a-polygon-layer-to-the-map-android-sdk"></a>Aggiungere un livello poligono alla mappa (Android SDK)

Questo articolo illustra come eseguire il rendering delle aree delle geometrie delle caratteristiche `Polygon` e `MultiPolygon` sulla mappa usando un livello poligono.

## <a name="prerequisites"></a>Prerequisiti

Assicurarsi di completare la procedura descritta nella [Guida introduttiva: creare un documento dell'app Android](quick-android-map.md) . I blocchi di codice in questo articolo possono essere inseriti nel `onReady` gestore eventi maps.

## <a name="use-a-polygon-layer"></a>Aggiungere un livello poligono

Quando un livello poligono viene connesso a un'origine dati e caricato sulla mappa, esegue il rendering dell'area con le caratteristiche `Polygon` e `MultiPolygon`. Per creare un poligono, aggiungerlo a un'origine dati ed eseguirne il rendering con un livello poligono usando la `PolygonLayer` classe.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source, 
    fillColor("red"),
    fillOpacity(0.7f)
), "labels");
```

Lo screenshot seguente mostra il codice precedente che esegue il rendering dell'area di un poligono usando un livello poligono.

![Poligono con area di riempimento sottoposta a rendering](media/how-to-add-shapes-to-android-map/android-polygon-layer.png)

## <a name="use-a-polygon-and-line-layer-together"></a>Usare insieme un poligono e un livello linea

Per eseguire il rendering del contorno dei poligoni viene usato un livello linea. L'esempio di codice seguente esegue il rendering di un poligono come nell'esempio precedente, ma con l'aggiunta di un livello linea. Questo livello linea è un secondo livello connesso all'origine dati.  

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
    fillColor("rgba(0, 200, 200, 0.5)")
), "labels");

//Create and add a line layer to render the outline of the polygon.
map.layers.add(new LineLayer(source,
    strokeColor("red"),
    strokeWidth(2f)
));
```

Lo screenshot seguente mostra il codice precedente che esegue il rendering di un poligono con il relativo contorno usando un livello linea.

![Poligono con il rendering dell'area di riempimento e del contorno](media/how-to-add-shapes-to-android-map/android-polygon-and-line-layer.png)

> [!TIP]
> Quando si delinea un poligono con un livello linea, assicurarsi di chiudere tutti gli anelli nei poligoni in modo che ogni matrice di punti abbia lo stesso punto di inizio e di fine. Se questa operazione non viene eseguita, il livello linea potrebbe non connettere l'ultimo punto del poligono al primo punto.

## <a name="next-steps"></a>Passaggi successivi

Per altri esempi di codice da aggiungere alle mappe, vedere gli articoli seguenti:

> [!div class="nextstepaction"]
> [Creare un'origine dati](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Usare espressioni di stile basate sui dati](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Aggiungere un livello per le linee](android-map-add-line-layer.md)
