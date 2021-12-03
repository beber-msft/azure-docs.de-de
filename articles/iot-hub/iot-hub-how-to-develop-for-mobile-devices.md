---
title: Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Erfahren Sie mehr über die Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs.'
author: eross-msft
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: lizross
ms.openlocfilehash: c0c7d58284528f33edfcf0a8e497bd8a99f73b53
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132554558"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs | Microsoft-Dokumentation

„Dinge“ im Internet der Dinge (Internet of Things, IoT) können sich auf eine breite Palette von Geräten mit unterschiedlichen Fähigkeiten beziehen: Sensoren, Mikrocontroller, intelligente Geräte, industrielle Gateways und sogar mobile Geräte.  Ein mobiles Gerät kann ein IoT-Gerät sein, das Gerät-zu-Cloud-Telemetriedaten sendet und von der Cloud verwaltet wird.  Es kann auch das Gerät sein, auf dem eine Back-End-Dienstanwendung ausgeführt wird, die andere IoT-Geräte verwaltet.  In beiden Fällen können [Azure IoT Hub SDKs](./iot-hub-devguide-sdks.md) verwendet werden, um Anwendungen zu entwickeln, die auf mobilen Geräten funktionieren.  

## <a name="develop-for-native-ios-platform"></a>Entwicklung für die native iOS-Plattform

Azure IoT Hub SDKs unterstützen über das IoT Azure Hub C SDK die native iOS-Plattform.  Sie können es sich als ein iOS SDK vorstellen, das Sie in Ihr XCode-Projekt des Typs Swift oder Objective C einbinden können.  Es gibt zwei Möglichkeiten, das C SDK unter iOS zu verwenden:

* Die CocoaPod-Bibliotheken direkt im XCode-Projekt verwenden.  
* Den Quellcode für das C SDK herunterladen und entsprechend der [Buildanweisung](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) für macOS Builds für die iOS-Plattform erstellen.  

Das Azure IoT Hub C SDK ist zur maximalen Portabilität auf verschiedene Plattformen in C99 geschrieben.  Der Portierungsprozess umfasst das Schreiben einer schlanken Adaptionsschicht für die plattformspezifischen Komponenten, die hier für [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx) zu finden ist.  Die Funktionen des C SDK können auf der iOS-Plattform genutzt werden, einschließlich der unterstützten Azure IoT Hub-Primitiven und SDK-spezifischen Funktionen, wie z.B. Wiederholungsrichtlinien für die Netzwerkstabilität.  Die Schnittstelle für das iOS SDK ähnelt auch der Schnittstelle für das Azure IoT Hub C SDK.  

In diesen Dokumentationen erfahren Sie, wie Sie eine Geräte- oder Dienstanwendung auf einem iOS-Gerät entwickeln:

* [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz](../iot-develop/quickstart-send-telemetry-iot-hub.md)  
* [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub](iot-hub-ios-swift-c2d.md) 

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Entwickeln mit CocoaPod-Bibliotheken für Azure IoT Hub

Die Azure IoT Hub SDKs bieten für die iOS-Entwicklung eine Gruppe von Objective-C CocoaPod-Bibliotheken.  Die aktuelle Liste der CocoaPod-Bibliotheken finden Sie unter [CocoaPods für Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md).  Sobald die entsprechenden Bibliotheken in Ihr XCode-Projekt eingebunden sind, gibt es zwei Möglichkeiten, IoT Hub-bezogenen Code zu schreiben:

* Objective-C-Funktion: Wenn Ihr Projekt in Objective-C geschrieben ist, können Sie APIs direkt aus dem Azure IoT Hub C SDK aufrufen.  Wenn Ihr Projekt in Swift geschrieben ist, können Sie `@objc func` aufrufen, bevor Sie Ihre Funktion erstellen, und mit dem Schreiben sämtlicher Logiken in Azure IoT Hub unter Verwendung von C- oder Objective-C-Code fortfahren.  Eine Reihe von Beispielen, die beide Verfahren veranschaulichen, finden Sie im [Beispielrepository](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* Einbinden von C-Beispielen: Wenn Sie eine C-Geräteanwendung geschrieben haben, können Sie in Ihrem XCode-Projekt direkt auf diese verweisen:
    * Fügen Sie in XCode die Datei „sample.c“ Ihrem XCode-Projekt hinzu.  
    * Fügen Sie Ihrer Abhängigkeit die Headerdatei hinzu.  Ein Beispiel einer Headerdatei ist im [Beispielrepository](https://github.com/Azure-Samples/azure-iot-samples-ios) enthalten. Weitere Informationen finden Sie auf der Apple-Dokumentationsseite für [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="develop-for-android-platform"></a>Entwickeln für die Android-Plattform
Das Azure IoT Hub Java SDK unterstützt die Android-Plattform.

In diesen Dokumentationen erfahren Sie, wie Sie eine Geräte- oder Dienstanwendung auf einem Android-Gerät mit Gradle und Android Studio entwickeln:

* [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz](../iot-develop/quickstart-send-telemetry-iot-hub.md)  
* [Schnellstart: Steuern eines mit einem IoT-Hub verbundenen Geräts (Android)](quickstart-control-device-android.md) 

## <a name="next-steps"></a>Nächste Schritte

* [REST-API-Referenz zu IoT Hub](/rest/api/iothub/)
* [Azure IoT C SDK-Quellcode](https://github.com/Azure/azure-iot-sdk-c)