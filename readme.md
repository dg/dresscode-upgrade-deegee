[Upgrading Dibi and Texy with DressCode](https://dresscode.run/upgrading-deegee)
================================================================================

[![Latest Stable Version](https://poser.pugx.org/dresscode/upgrade-deegee/v/stable)](https://packagist.org/packages/dresscode/upgrade-deegee)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://packagist.org/packages/dresscode/upgrade-deegee)

 <!---->

<h3>

✅ Upgrades code for [Dibi from 4.1 and Texy from 3.0](#versions-covered) to today<br>
✅ Knows [what each variable is](#what-gets-upgraded), so it rewrites only the right calls<br>
✅ Reports [what needs a human](#what-is-left-to-you), with what to write instead<br>
✅ From the author of Dibi and Texy, [written from every commit](#where-the-data-come-from) of both

</h3>

 <!---->

**Upgrade code using Dibi and Texy without hunting for renames.** Install this package, run `dresscode upgrade`, and
code written for older versions of [Dibi](https://github.com/dg/dibi) and [Texy](https://github.com/dg/texy) is
rewritten to the API of the versions your project stands on:

```diff
- $id = $this->db->insertId();
+ $id = $this->db->getInsertId();

- $result->setType('price', Type::FLOAT);
+ $result->setType('price', Type::Float);

- $slug = Texy::webalize($title);
+ $slug = Helpers::webalize($title);

- $el = new \TexyHtml('p');
+ $el = new \Texy\HtmlElement('p');
```

Four changes, the imports rewritten along with them, and not a byte more of the file touched. The package holds the
data, what each version of Dibi and Texy renamed, moved or retired; the rewriting is done by
[DressCode](https://dresscode.run), the PHP coding standard and upgrade tool, which comes with it.

 <!---->

Installation and first run
==========================

**1️⃣ Install it: `composer require --dev dresscode/upgrade-deegee`**<br>
**2️⃣ Upgrade the code: `vendor/bin/dresscode upgrade`**<br>
**3️⃣ Read the diff, run your tests and PHPStan, and commit**

The package brings DressCode along, and `dresscode upgrade` takes it as it finds it, with no configuration.
Most of the rewrites need to know what a variable is, which DressCode asks your PHPStan, so the project needs
`phpstan/phpstan` installed. The style of the code stays as it is: with a `dresscode.neon`, the upgrade keeps of
its coding standard what the code already follows, and without one it changes nothing but the old API. Where it
rewrote something, run it again, as the new code may hold more to upgrade.

To have every `dresscode check` and `dresscode fix` watch for the old API too, in CI or in a Git hook, turn the
upgrade on in `dresscode.neon`; the upgrade is the set `deprecations`, with the types of the code:

```neon
typeAnalysis: phpstan

use:
	- deprecations
```

 <!---->

What gets upgraded
==================

None of it is search and replace. DressCode asks PHPStan what each variable is, so `insertId()` is rewritten where it
is called on a connection of Dibi and nowhere else. What the package rewrites:

- **constants out of upper case**: `Type::FLOAT` to `Type::Float`, `Fluent::REMOVE` to `Fluent::Remove`,
  `HtmlElement::INNER_TEXT` to `HtmlElement::InnerText`,
- **renamed methods** of Dibi: `affectedRows()` to `getAffectedRows()`, `insertId()` to `getInsertId()`, on a
  connection and on the static `dibi` alike,
- **the helpers of Texy** moved to `Texy\Helpers`: `webalize()`, `outdent()`, `normalize()`, and `Utf::strtolower()`
  and `Utf::utf2ascii()` to `Helpers::toLower()` and `Helpers::toAscii()`,
- **the classes of Texy** moved into its namespace, `TexyHtml` to `Texy\HtmlElement`, and a module that moved,
  `$texy->cleaner` to `$texy->htmlOutputModule`,
- **renamed classes and parameters**: `DibiExtension22` to `DibiExtension3`, `new Modifier(mod: ...)` to
  `new Modifier(s: ...)`.

The rewritten code takes the shape of the file around it, or of your coding standard, if the configuration names one.

 <!---->

What is left to you
===================

Where there is no replacement, DressCode does not guess. It reports the place and says what to do:

```
Property `Texy\Texy::$encoding` is forbidden: the input and the output are always UTF-8; drop the setting
Constant `Texy\Texy::XHTML1_STRICT` is forbidden: there is no replacement, XHTML is not generated any more and the output is always HTML5
```

An upgrade is a rewrite plus a list of what remains. Every such sentence is written as an instruction, so the list
can be worked through by you or handed to an AI coding agent as it is.

 <!---->

Where the data come from
========================

The data come from the author of Dibi and Texy. Both libraries first got an upgrading guide, `docs/upgrading.md` in
their repositories, written from their whole history, commit by commit, and every entry was then checked against the
code of the release that made the change. The data are also checked against the installed libraries, so a
replacement that does not exist cannot get in, and each library has a sample of old code that the data must turn
into the expected new code.

 <!---->

Versions covered
================

Dibi from its version 4.1 and Texy from its version 3.0 to the current ones. The data of each library apply only when
your project has it, and only the sections of the versions it stands on: the lowest version the constraint in
`composer.json` allows, or the version the key `targets` of the configuration names. The
[manual](https://dresscode.run/upgrading-deegee) has the details.

 <!---->

Limits
======

- **Most rewrites need PHPStan.** `dresscode upgrade` does not run without it, and `check` and `fix` without
  `typeAnalysis: phpstan` rewrite only classes, functions and annotations.
- **DressCode runs on PHP 8.4 to 8.6.** The code it upgrades may be written for PHP 8.0 and newer, but the package is
  installed into the project, so the project has to install on PHP 8.4 or newer.
- **The data fix the code, not its meaning.** A change of behavior is not in the data; the upgrading guides of the
  libraries describe it. Run your tests after a fix.

 <!---->

Other ecosystems
================

| package | upgrades |
|---|---|
| [`dresscode/upgrade-nette`](https://github.com/dg/dresscode-upgrade-nette) | every Nette library, most from 3.0 |
| [`dresscode/upgrade-symfony`](https://github.com/dg/dresscode-upgrade-symfony) | the components and bridges of Symfony, from 6.0 |
| [`dresscode/upgrade-laravel`](https://github.com/dg/dresscode-upgrade-laravel) | the Laravel framework from 6, and the attributes of Laravel 13 |
| [`dresscode/upgrade-deegee`](https://github.com/dg/dresscode-upgrade-deegee) | Dibi and Texy |

A package like these can be written for any library; the [manual](https://dresscode.run/upgrading-data) says how.

 <!---->

Development
===========

The libraries the data are about are in `require-dev`, so the data are checked against their installed versions:

- `php tests/check.php <library>` lints `upgrading/<library>.neon` and runs its sample,
  `tests/samples/<library>.code`, comparing the result with `.expected` and `.violations`,
- `php tests/check.php <library> --update` writes those two from the run; read the diff, it is what the data do,
- `vendor/bin/tester tests` runs all of it.
