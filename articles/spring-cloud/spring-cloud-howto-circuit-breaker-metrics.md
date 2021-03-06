---
title: Raccolta delle metriche del circuito Resilience4J di Spring cloud
description: Come raccogliere le metriche del circuito Resilience4J di Spring cloud.
author: MikeDodaro
ms.author: brendanm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 12/15/2020
ms.custom: devx-track-java
ms.openlocfilehash: e44e7c5d04695d5bd65d2eedc5474889a707c8bd
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882145"
---
# <a name="collect-spring-cloud-resilience4j-circuit-breaker-metrics-preview"></a>Raccolta delle metriche del circuito Resilience4J di Spring cloud (anteprima)

Questo documento illustra come raccogliere metriche del circuito Resilience4j di Spring cloud con Application Insights agente Java in-process.  Con questa funzionalità è possibile monitorare le metriche dell'interruttore resilience4j da Application Insights.

Per illustrare il funzionamento, viene usato [Spring-cloud-Circuit-Breaker-demo](https://github.com/spring-cloud-samples/spring-cloud-circuitbreaker-demo) .

## <a name="prerequisites"></a>Prerequisiti

* Abilitare Java In-Process Agent dalla [guida Application Insights di java In-Process Agent](./spring-cloud-howto-application-insights.md#enable-java-in-process-agent-for-application-insights). 

* Abilitare la raccolta di dimensioni per le metriche resilience4j dalla [guida Application Insights](../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).

* Installare Git, Maven e Java, se non è già usato dal computer di sviluppo.

## <a name="build-and-deploy-apps"></a>Compilare e distribuire app

La procedura seguente compila e distribuisce le app.

1. Clonare e compilare il repository demo.

```bash
git clone https://github.com/spring-cloud-samples/spring-cloud-circuitbreaker-demo.git
cd spring-cloud-circuitbreaker-demo && mvn clean package -DskipTests
```

2. Creare applicazioni con endpoint

```azurecli
az spring-cloud app create --name resilience4j --is-public \
    -s ${asc-service-name} -g ${asc-resource-group}
az spring-cloud app create --name reactive-resilience4j --is-public \
    -s ${asc-service-name} -g ${asc-resource-group}
```

3. Distribuire le applicazioni.

```azurecli
az spring-cloud app deploy -n resilience4j \
    --jar-path ./spring-cloud-circuitbreaker-demo-resilience4j/target/spring-cloud-circuitbreaker-demo-resilience4j-0.0.1.BUILD-SNAPSHOT.jar \
    -s ${service_name} -g ${resource_group}
az spring-cloud app deploy -n reactive-resilience4j \
    --jar-path ./spring-cloud-circuitbreaker-demo-reactive-resilience4j/target/spring-cloud-circuitbreaker-demo-reactive-resilience4j-0.0.1.BUILD-SNAPSHOT.jar \
    -s ${service_name} -g ${resource_group}
```

> [!Note]
>
> * Includere la dipendenza obbligatoria per Resilience4j:
>
>   ```xml
>   <dependency>
>       <groupId>io.github.resilience4j</groupId>
>       <artifactId>resilience4j-micrometer</artifactId>
>   </dependency>
>   <dependency>
>       <groupId>org.springframework.cloud</groupId>
>       <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
>   </dependency>
>   ```
> * Il codice cliente deve usare l'API di `CircuitBreakerFactory` , che viene implementata come un oggetto `bean` creato automaticamente quando si include uno starter di Spring cloud Circuit Breaker. Per informazioni dettagliate, vedere [Spring cloud Circuit Breaker](https://spring.io/projects/spring-cloud-circuitbreaker#overview).
>
> * Le due dipendenze seguenti presentano conflitti con i pacchetti resilient4j precedenti.  Assicurarsi che il cliente non le includa.
>
>   ```xml
>   <dependency>
>       <groupId>org.springframework.cloud</groupId>
>       <artifactId>spring-cloud-starter-sleuth</artifactId>
>   </dependency>
>   <dependency>
>       <groupId>org.springframework.cloud</groupId>
>       <artifactId>spring-cloud-starter-zipkin</artifactId>
>   </dependency>
>   ```
>
>
> Passare all'URL fornito dalle applicazioni gateway e accedere all'endpoint da [Spring-cloud-Circuit-Breaker-demo](https://github.com/spring-cloud-samples/spring-cloud-circuitbreaker-demo) come indicato di seguito:
>
>   ```console
>   /get
>   /get/delay/{seconds}
>   /get/fluxdelay/{seconds}
>   ```

## <a name="locate-resilence4j-metrics-from-portal"></a>Individuare le metriche di Resilence4j dal portale

1. Selezionare il pannello **Application Insights** dal portale di Azure Spring cloud e fare clic su **Application Insights**.

   [![resilience4J 0](media/spring-cloud-resilience4j/resilience4J-0.png)](media/spring-cloud-resilience4j/resilience4J-0.PNG)

2. Selezionare **metriche** dalla pagina **Application Insights** .  Selezionare **Azure. applicationinsights** dallo **spazio dei nomi Metrics**.  Selezionare anche **resilience4j_circuitbreaker_buffered_calls** metrica con la **media**.

   [![resilience4J 1](media/spring-cloud-resilience4j/resilience4J-1.png)](media/spring-cloud-resilience4j/resilience4J-1.PNG)

3. Selezionare **resilience4j_circuitbreaker_calls** metrica e **media**.

   [![resilience4J 2](media/spring-cloud-resilience4j/resilience4J-2.png)](media/spring-cloud-resilience4j/resilience4J-2.PNG)

4. Selezionare **resilience4j_circuitbreaker_calls**  metrica e **media**.  Fare clic su **Aggiungi filtro** e quindi selezionare nome come **createNewAccount**.

   [![resilience4J 3](media/spring-cloud-resilience4j/resilience4J-3.png)](media/spring-cloud-resilience4j/resilience4J-3.PNG)

5. Selezionare **resilience4j_circuitbreaker_calls**  metrica e **media**.  Quindi fare clic su **applica suddivisione** e selezionare **tipo**.

   [![resilience4J 4](media/spring-cloud-resilience4j/resilience4J-4.png)](media/spring-cloud-resilience4j/resilience4J-4.PNG)

6. Selezionare **resilience4j_circuitbreaker_calls**,'**resilience4j_circuitbreaker_buffered_calls** e **resilience4j_circuitbreaker_slow_calls** metrica con la **media**.

   [![resilience4J 5](media/spring-cloud-resilience4j/resilience4j-5.png)](media/spring-cloud-resilience4j/resilience4j-5.PNG)

## <a name="see-also"></a>Vedi anche

* [Application Insights](./spring-cloud-howto-application-insights.md)
* [Analisi distribuita](spring-cloud-tutorial-distributed-tracing.md)
* [Dashboard interruttore](spring-cloud-tutorial-circuit-breaker.md)