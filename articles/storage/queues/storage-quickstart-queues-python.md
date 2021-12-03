---
title: 'Schnellstart: Azure Queue Storage-Clientbibliothek v12 – Python'
description: Hier erfahren Sie, wie Sie mithilfe der Azure Queue Storage-Clientbibliothek v12 für Python eine Warteschlange erstellen und ihr Nachrichten hinzufügen. Dann lernen Sie, wie Sie Nachrichten in der Warteschlange lesen und löschen. Zudem erfahren Sie, wie Sie eine Warteschlange löschen.
author: normesta
ms.author: normesta
ms.date: 07/23/2021
ms.topic: quickstart
ms.service: storage
ms.subservice: queues
ms.custom: devx-track-python, mode-api
ms.openlocfilehash: 9da9cfc756eb9abf082b105a9b9f690093a62c0a
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131083325"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-python"></a>Schnellstart: Azure Queue Storage-Clientbibliothek v12 für Python

Beginnen Sie mit dem Einsatz der Azure Queue Storage-Clientbibliothek v12 für Python. Azure Queue Storage ist ein Dienst zum Speichern einer großen Anzahl von Nachrichten, die später abgerufen und verarbeitet werden. Führen Sie die nachfolgenden Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben zu testen.

Mit der Azure Queue Storage-Clientbibliothek v12 für Python können Sie Folgendes ausführen:

- Erstellen einer Warteschlange
- Hinzufügen von Nachrichten zu einer Warteschlange
- Einsehen von Nachrichten in einer Warteschlange
- Aktualisieren einer Nachricht in einer Warteschlange
- Empfangen von Nachrichten aus einer Warteschlange
- Löschen von Nachrichten aus einer Warteschlange
- Löschen einer Warteschlange

Zusätzliche Ressourcen:

- [API-Referenzdokumentation](/python/api/azure-storage-queue/index)
- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue)
- [Paket (Python-Paketindex)](https://pypi.org/project/azure-storage-queue/)
- [Beispiele](../common/storage-samples-python.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- Azure Storage-Konto – [Erstellen eines Speicherkontos](../common/storage-account-create.md)
- [Python](https://www.python.org/downloads/) für Ihr Betriebssystem – 2.7 bzw. 3.6 oder höher

## <a name="setting-up"></a>Einrichten

In diesem Abschnitt wird beschrieben, wie Sie ein Projekt zum Arbeiten mit der Azure Queue Storage-Clientbibliothek v12 für Python vorbereiten.

### <a name="create-the-project"></a>Erstellen des Projekts

Erstellen Sie eine Python-Anwendung mit dem Namen `queues-quickstart-v12`.

1. Erstellen Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) ein neues Verzeichnis für das Projekt.

    ```console
    mkdir queues-quickstart-v12
    ```

1. Wechseln Sie zum neu erstellten Verzeichnis `queues-quickstart-v12`.

    ```console
    cd queues-quickstart-v12
    ```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie das Paket mit der Azure Queue Storage-Clientbibliothek für Python mithilfe des Befehls `pip install`.

```console
pip install azure-storage-queue
```

Dieser Befehl installiert das Paket mit der Azure Queue Storage-Clientbibliothek für Python sowie alle Bibliotheken, von denen die Bibliothek abhängig ist. In diesem Fall ist das nur die Azure-Kernbibliothek für Python.

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

1. Öffnen einer neuen Textdatei im Code-Editor
1. Hinzufügen von `import`-Anweisungen
1. Erstellen der Struktur für das Programm, einschließlich einer sehr einfachen Ausnahmebehandlung

    Der Code lautet wie folgt:

    ```python
    import os, uuid
    from azure.storage.queue import QueueServiceClient, QueueClient, QueueMessage

    try:
        print("Azure Queue storage v12 - Python quickstart sample")
        # Quick start code goes here
    except Exception as ex:
        print('Exception:')
        print(ex)

    ```

1. Speichern Sie die neue Datei als `queues-quickstart-v12.py` im Verzeichnis `queues-quickstart-v12`.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Objektmodell

Azure Queue Storage ist ein Dienst für die Speicherung großer Nachrichtenmengen. Eine Warteschlangennachricht kann bis zu 64 KB groß sein. Eine Warteschlange kann Millionen Nachrichten enthalten, bis die maximale Kapazität eines Speicherkontos erreicht ist. Warteschlangen werden häufig verwendet, um ein Arbeits-Backlog zur asynchronen Verarbeitung zu erstellen. Queue Storage bietet drei Arten von Ressourcen:

- Das Speicherkonto
- Eine Warteschlange im Speicherkonto
- Nachrichten in der Warteschlange

Im folgenden Diagramm ist die Beziehung zwischen diesen Ressourcen dargestellt.

![Diagramm der Queue Storage-Architektur](./media/storage-queues-introduction/queue1.png)

Verwenden Sie die folgenden Python-Klassen zur Interaktion mit diesen Ressourcen:

- [`QueueServiceClient`](/python/api/azure-storage-queue/azure.storage.queue.queueserviceclient): Mit dem `QueueServiceClient` können Sie alle Warteschlangen in Ihrem Speicherkonto verwalten.
- [`QueueClient`](/python/api/azure-storage-queue/azure.storage.queue.queueclient): Mit der `QueueClient`-Klasse können Sie eine einzelne Warteschlange und die darin enthaltenen Nachrichten verwalten und bearbeiten.
- [`QueueMessage`](/python/api/azure-storage-queue/azure.storage.queue.queuemessage): Die `QueueMessage`-Klasse repräsentiert die einzelnen Objekte, die beim Aufrufen von [`receive_messages`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) in einer Warteschlange zurückgegeben werden.

## <a name="code-examples"></a>Codebeispiele

Diese Beispielcodeausschnitte zeigen Ihnen, wie folgende Aktionen mit der Azure Queue Storage-Clientbibliothek für Python ausgeführt werden:

- [Abrufen der Verbindungszeichenfolge](#get-the-connection-string)
- [Erstellen einer Warteschlange](#create-a-queue)
- [Hinzufügen von Nachrichten zu einer Warteschlange](#add-messages-to-a-queue)
- [Einsehen von Nachrichten in einer Warteschlange](#peek-at-messages-in-a-queue)
- [Aktualisieren einer Nachricht in einer Warteschlange](#update-a-message-in-a-queue)
- [Empfangen von Nachrichten aus einer Warteschlange](#receive-messages-from-a-queue)
- [Löschen von Nachrichten aus einer Warteschlange](#delete-messages-from-a-queue)
- [Löschen einer Warteschlange](#delete-a-queue)

### <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Der folgende Code ruft die Verbindungszeichenfolge für das Speicherkonto ab. Die Verbindungszeichenfolge ist in der Umgebungsvariable gespeichert, die Sie im Abschnitt [Konfigurieren der Speicherverbindungszeichenfolge](#configure-your-storage-connection-string) erstellt haben.

Fügen Sie diesen Code im `try`-Block hinzu:

```python
    # Retrieve the connection string for use with the application. The storage
    # connection string is stored in an environment variable on the machine
    # running the application called AZURE_STORAGE_CONNECTION_STRING. If the
    # environment variable is created after the application is launched in a
    # console or with Visual Studio, the shell or application needs to be
    # closed and reloaded to take the environment variable into account.
    connect_str = os.getenv('AZURE_STORAGE_CONNECTION_STRING')
```

### <a name="create-a-queue"></a>Erstellen einer Warteschlange

Legen Sie einen Namen für die neue Warteschlange fest. Der folgende Code fügt einen UUID-Wert an den Warteschlangennnamen an, um sicherzustellen, dass er eindeutig ist.

> [!IMPORTANT]
> Warteschlangennamen dürfen nur Kleinbuchstaben, Ziffern und Bindestriche enthalten und müssen mit einem Buchstaben oder einer Ziffer beginnen. Vor und nach jedem Bindestrich muss ein Zeichen stehen, das kein Bindestrich ist. Der Name muss außerdem zwischen 3 und 63 Zeichen lang sein. Weitere Informationen finden Sie unter [Benennen von Warteschlangen und Metadaten](/rest/api/storageservices/naming-queues-and-metadata).

Erstellen Sie eine Instanz der [`QueueClient`](/python/api/azure-storage-queue/azure.storage.queue.queueclient)-Klasse. Rufen Sie dann die [`create_queue`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#create-queue---kwargs-)-Methode auf, um die Warteschlange in Ihrem Speicherkonto zu erstellen.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu:

```python
    # Create a unique name for the queue
    queue_name = "quickstartqueues-" + str(uuid.uuid4())

    print("Creating queue: " + queue_name)

    # Instantiate a QueueClient which will be
    # used to create and manipulate the queue
    queue_client = QueueClient.from_connection_string(connect_str, queue_name)

    # Create the queue
    queue_client.create_queue()
```

### <a name="add-messages-to-a-queue"></a>Hinzufügen von Nachrichten zu einer Warteschlange

Der folgende Codeausschnitt fügt der Warteschlange durch Aufrufen der [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-)-Methode Nachrichten hinzu. Außerdem speichert er die [`QueueMessage`](/python/api/azure-storage-queue/azure.storage.queue.queuemessage), die vom dritten `send_message`-Aufruf zurückgegeben wird. `saved_message` wird später im Programm zum Aktualisieren des Nachrichteninhalts verwendet.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu:

```python
    print("\nAdding messages to the queue...")

    # Send several messages to the queue
    queue_client.send_message(u"First message")
    queue_client.send_message(u"Second message")
    saved_message = queue_client.send_message(u"Third message")
```

### <a name="peek-at-messages-in-a-queue"></a>Einsehen von Nachrichten in einer Warteschlange

Durch Aufrufen der [`peek_messages`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#peek-messages-max-messages-none----kwargs-)-Methode können Sie die Nachrichten in der Warteschlange einsehen. Diese Methode ruft mindestens eine Nachricht vom Anfang der Warteschlange ab, ändert aber nicht die Sichtbarkeit der Nachricht.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu:

```python
    print("\nPeek at the messages in the queue...")

    # Peek at messages in the queue
    peeked_messages = queue_client.peek_messages(max_messages=5)

    for peeked_message in peeked_messages:
        # Display the message
        print("Message: " + peeked_message.content)
```

### <a name="update-a-message-in-a-queue"></a>Aktualisieren einer Nachricht in einer Warteschlange

Aktualisieren Sie den Inhalt einer Nachricht durch Aufrufen der [`update_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#update-message-message--pop-receipt-none--content-none----kwargs-)-Methode. Diese Methode kann das Sichtbarkeitstimeout und den Inhalt einer Nachricht ändern. Beim Nachrichteninhalt muss es sich um eine UTF-8-codierte Zeichenfolge handeln, die bis zu 64 KB groß sein darf. Übergeben Sie zusammen mit dem neuen Inhalt Werte aus der Nachricht, die weiter oben im Code gespeichert wurde. Die `saved_message`-Werte identifizieren die Nachricht, die aktualisiert werden soll.

```python
    print("\nUpdating the third message in the queue...")

    # Update a message using the message saved when calling send_message earlier
    queue_client.update_message(saved_message, pop_receipt=saved_message.pop_receipt, \
        content="Third message has been updated")
```

### <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Laden Sie zuvor hinzugefügte Nachrichten durch Aufrufen der [`receive_messages`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-)-Methode herunter.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu:

```python
    print("\nReceiving messages from the queue...")

    # Get messages from the queue
    messages = queue_client.receive_messages(messages_per_page=5)
```

### <a name="delete-messages-from-a-queue"></a>Löschen von Nachrichten aus einer Warteschlange

Löschen Sie Nachrichten aus einer Warteschlange, nachdem sie empfangen und verarbeitet wurden. In diesem Fall besteht die Verarbeitung nur darin, dass die Nachricht in der Konsole angezeigt wird.

Die App wird angehalten und wartet auf Benutzereingaben, indem `input` aufgerufen wird, bevor die Nachrichten verarbeitet und gelöscht werden. Überprüfen Sie im [Azure-Portal](https://portal.azure.com), ob die Ressourcen ordnungsgemäß erstellt wurden, bevor sie gelöscht werden. Alle Nachrichten, die nicht explizit gelöscht werden, werden schlussendlich wieder in der Warteschlange angezeigt und ggf. erneut verarbeitet.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu:

```python
    print("\nPress Enter key to 'process' messages and delete them from the queue...")
    input()

    for msg_batch in messages.by_page():
            for msg in msg_batch:
                # "Process" the message
                print(msg.content)
                # Let the service know we're finished with
                # the message and it can be safely deleted.
                queue_client.delete_message(msg)
```

### <a name="delete-a-queue"></a>Löschen einer Warteschlange

Der folgende Code bereinigt die von der App erstellten Ressourcen, indem die Warteschlange mithilfe der [`delete_queue`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#delete-queue---kwargs-)-Methode gelöscht wird.

Fügen Sie diesen Code am Ende des `try`-Blocks hinzu, und speichern Sie die Datei:

```python
    print("\nPress Enter key to delete the queue...")
    input()

    # Clean up
    print("Deleting queue...")
    queue_client.delete_queue()

    print("Done")
```

## <a name="run-the-code"></a>Ausführen des Codes

Diese App erstellt drei Nachrichten und fügt sie einer Azure-Warteschlange hinzu. Der Code listet die Nachrichten in der Warteschlange auf, ruft sie ab und löscht sie und löscht letztendlich die Warteschlange.

Navigieren Sie in Ihrem Konsolenfenster zu dem Verzeichnis, das die Datei `queues-quickstart-v12.py` enthält, und führen Sie dann die App mit dem folgenden `python`-Befehl aus.

```console
python queues-quickstart-v12.py
```

Die Ausgabe der App sieht etwa wie das folgende Beispiel aus:

```output
Azure Queue Storage client library v12 - Python quickstart sample
Creating queue: quickstartqueues-cac365be-7ce6-4065-bd65-3756ea052cb8

Adding messages to the queue...

Peek at the messages in the queue...
Message: First message
Message: Second message
Message: Third message

Updating the third message in the queue...

Receiving messages from the queue...

Press Enter key to 'process' messages and delete them from the queue...

First message
Second message
Third message has been updated

Press Enter key to delete the queue...

Deleting queue...
Done
```

Wenn die App vor dem Empfangen von Nachrichten angehalten wird, überprüfen Sie Ihr Speicherkonto im [Azure-Portal](https://portal.azure.com). Überprüfen Sie, ob sich in der Warteschlange Nachrichten befinden.

Drücken Sie die `Enter`, um Nachrichten zu empfangen und zu löschen. Wenn Sie dazu aufgefordert werden, drücken Sie erneut die `Enter`, um die Warteschlange zu löschen und die Demo zu beenden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mithilfe von Python-Code eine Warteschlange erstellen und dieser Nachrichten hinzufügen. Danach haben Sie erfahren, wie Sie Nachrichten einsehen, abrufen und löschen. Zum Schluss haben Sie gelernt, wie Sie eine Nachrichtenwarteschlange löschen.

Tutorials, Beispiele, Schnellstartanleitungen und weiteres Dokumentationsmaterial finden Sie hier:

> [!div class="nextstepaction"]
> [Azure für Python-Entwickler](/azure/python/)

- Weitere Informationen finden Sie unter [Azure Storage-Bibliotheken für Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage).
- Weitere Informationen zu Azure Queue Storage-Beispiel-Apps finden Sie unter [Azure Queue Storage-Clientbibliothek v12 für Python – Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue/samples).
