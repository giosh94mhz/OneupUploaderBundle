Use Gaufrette as Storage layer
==============================

Gaufrette is an abstract storage layer you can use to store your uploaded files. From the _Why use Gaufrette_ section on [their Repository](https://github.com/KnpLabs/Gaufrette):

> The filesystem abstraction layer permits you to develop your application without the need to know were all those medias will be stored and how.
>
> Another advantage of this is the possibility to update the files location without any impact on the code apart from the definition of your filesystem. In example, if your project grows up very fast and if your server reaches its limits, you can easily move your medias in an Amazon S3 server or any other solution.

In order to use Gaufrette with OneupUploaderBundle, be sure to follow these steps:

## Install KnpGaufretteBundle

Add the KnpGaufretteBundle to your composer.json file.

```js
{
    "require": {
        "knplabs/knp-gaufrette-bundle": "0.1.*"
    }
}
```

And update your dependencies through composer.

    $> php composer.phar update knplabs/knp-gaufrette-bundle

After installing, enable the bundle in your AppKernel:

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Knp\Bundle\GaufretteBundle\KnpGaufretteBundle(),
    );
}
```

## Configure your Filesystems

The following is a sample configuration for the KnpGaufretteBundle. It will create a filystem service called `gaufrette.gallery_filesystem` which can be used in the OneupUploaderBundle. For a complete list of features refer to the [official documentation](https://github.com/KnpLabs/KnpGaufretteBundle).

```yml
knp_gaufrette:
    adapters:
        gallery:
            local:
                directory: %kernel.root_dir%/../web/uploads
                create: true
    
    filesystems:
        gallery:
            adapter: gallery
    
    stream_wrapper: ~
```

## Configure your mappings

Activate Gaufrette by switching the `type` property to `gaufrette` and pass the Gaufrette filesystem configured in the previous step.

```yml
oneup_uploader:
    mappings:
        gallery:
            storage:
                type: gaufrette
                filesystem: gaufrette.gallery_filesystem 
```
