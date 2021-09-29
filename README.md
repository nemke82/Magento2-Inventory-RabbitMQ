# Magento2-Inventory-RabbitMQ
Patch to convert inventory.source.items.cleanup inventory.mass.update inventory.reservations.cleanup inventory.reservations.update inventory.reservations.updateSalabilityStatus inventory.indexer.sourceItem inventory.indexer.stock consumers from MySQL to RabbitMQ Broker

-- To apply patch --

Add the cweagans/composer-patches plugin to the composer.json file.
```
composer require cweagans/composer-patches
```

Edit the composer.json file and add the following section to specify:
```
    "extra": {
        "magento-force": "override",
        "patches": {
            "magento/module-media-gallery-synchronization": {
                "Media-RabbitMQ: media.gallery.synchronization conversion to amqp": "https://raw.githubusercontent.com/nemke82/Magento2-Media-RabbitMQ/main/module-media-gallery-synchronization.patch"
            },
            "magento/module-media-gallery-renditions": {
                "Media-RabbitMQ: media.gallery.renditions.update conversion to amqp": "https://raw.githubusercontent.com/nemke82/Magento2-Media-RabbitMQ/main/module-media-gallery-renditions.patch"
            },
            "magento/module-media-content-synchronization": {
                "Media-RabbitMQ: media.content.synchronization conversion to amqp": "https://raw.githubusercontent.com/nemke82/Magento2-Media-RabbitMQ/main/module-media-content-synchronization.patch"
            },
            "magento/module-media-storage": {
                "Media-RabbitMQ: media.storage.catalog.image.resize conversion to amqp": "https://raw.githubusercontent.com/nemke82/Magento2-Media-RabbitMQ/main/module-media-storage.patch"
            }
          }    
```
Text is added in the "extra": { section, with following content:

Explanations: <BR>
Module: "magento/module-name"   ← That’s the Magento 2 Core module we are patching <BR>
Title: We link this with internal subject name. <BR>
URL to patch: That’s link from above and our demo repository. <BR>
<BR>
Apply the patch. Use the -v option only if you want to see debugging information.
```
composer -v install
```

Update the composer.lock file. The lock file tracks which patches have been applied to each  Composer package in an object.
```
composer update --lock
```

IMPORTANT INFO: <BR>
Please verify that env.php contains correct rabbit creds & above listed consumers are added to consumer_runners if you run consumers using Cron, otherwise it will not run. Also, after patch applied make sure that you execute setup:upgrade and verify if queue is created within RabbitMQ service.
