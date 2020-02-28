TODO
====

Currently the `FeatureManager` class is a very shallow interpretation of Piotr Miazga's proposed
API and associated scaffolding classes (see https://phabricator.wikimedia.org/T244481 and
https://gerrit.wikimedia.org/r/#/c/mediawiki/skins/Vector/+/572323/). This document aims to list
the steps required to get from this system to something as powerful as Piotr's.

1. Decide whether "set" is the correct name
2. Add support for sets that utilise contextual information that isn't available at boot time, e.g.

```php
use Vector\Constants;
use IContextSource;

$featureManager->registerSet( Constants::LOGGED_IN_SET, function( IContextSource $context ) {
	$user = $context->getUser();

	return $user
		&& $user->isSafeToLoad()
		&& $user->isLoggedIn();
} );

$featureManager->registerSet( Constants::MAINSPACE_SET, function ( IContextSource $context ) {
	$title = $context->getTitle();

	return $title && $title->inNamespace( NS_MAIN );
} );
```

3. Consider supporing memoization of those sets (see https://gerrit.wikimedia.org/r/#/c/mediawiki/skins/Vector/+/573626/7/includes/FeatureManagement/FeatureManager.php@68)
4. Add support for getting all sets
5. Add support for getting all features enabled when a set is enabled/disabled