## Repository to showcase WordPressCS 3.0 custom config issue

Steps to reporoduce:

1. Clone the repository
2. Go to the project folder
3. Run `composer install`
4. Run `vendor/bin/phpcs src`

Current result:

```
➜  phpcs3.0-custom-rule-repro git:(main) ✗ vendor/bin/phpcs src

Fatal error: Cannot declare class WordPressCS\WordPress\Sniffs\Files\FileNameSniff, because the name is already in use in /Users/xxx/Local Sites/xxx/app/public/wp-content/plugins/phpcs3.0-custom-rule-repro/vendor/wp-coding-standards/wpcs/WordPress/Sniffs/Files/FilenameSniff.php on line 36
```

Expected result:

```
➜  phpcs3.0-custom-rule-repro git:(main) ✗ vendor/bin/phpcs src

FILE: /Users/xxx/Local Sites/xxx/app/public/wp-content/plugins/phpcs3.0-custom-rule-repro/src/index.php
--------------------------------------------------------------------------------------------------------------------
FOUND 2 ERRORS AFFECTING 1 LINE
--------------------------------------------------------------------------------------------------------------------
 2 | ERROR | You must use "/**" style comments for a file comment
 2 | ERROR | Inline comments must end in full-stops, exclamation marks, or question marks
--------------------------------------------------------------------------------------------------------------------

Time: 115ms; Memory: 6MB
```

### Issue cause

If we go to the `.phpcs.xml` file and comment out the `Filename` rule, everything works fine:

```xml
<?xml version="1.0"?>
<ruleset name="Test Config">
	<file>.</file>
	<arg name="extensions" value="php"/>
	<rule ref="WordPress"/>
	<rule ref="WordPress-Extra"/>
	<rule ref="WordPress-Docs"/>

	<!-- <rule ref="WordPress.Files.Filename">
		<exclude-pattern>/src/*\.php</exclude-pattern>
	</rule> -->
</ruleset>
```

### Using default WordPress standard

If we specify standard from the cli (bypassing custom phpcs config), everything works fine:

```
➜  phpcs3.0-custom-rule-repro git:(main) ✗ vendor/bin/phpcs src --standard="WordPress"
```
