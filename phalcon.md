[Home](README.md)

# Phalcon

## Volt
The most basic, but standalone, way to invoke the Volt template engine is as follows.
Assume the file `/var/www/templates/test/multi/directory/hierarchy/view.phtml` exists and its contents are `{{ test }}`.
```
$oDi = new \Phalcon\DI();
$oView = new \Phalcon\Mvc\View();
$oView->setDi( $oDi );
$oView->setViewsDir( '/var/www/templates/' ); # note the trailing slash
$oView->registerEngines( [ '.phtml' => 'Phalcon\Mvc\View\Engine\Volt' ] );

$oView->test = "This works";
echo $oView->getRender( 'test/multi/directory/hierarchy', 'view' );
```
