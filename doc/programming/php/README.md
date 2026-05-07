---
title: "PHP"
keywords: [php, composer, laravel, symfony, jit, mago, modern-php]
status: reviewed
review_date: 2026/05/03
---

# PHP

A small landing for PHP. Modern PHP — 8.x with native types, attributes, enums, readonly classes, and the JIT — bears very little resemblance to the PHP 5 reputation that still follows the language around.

## Why PHP is still worth knowing

- **The web's installed base.** Every WordPress, Drupal, Magento, Moodle, MediaWiki, Joomla, and most of Wikipedia's stack is PHP. Even if you don't write new PHP, you'll probably read some.
- **Modern frameworks.** [Laravel](https://laravel.com/) and [Symfony](https://symfony.com/) are mature, well-typed, and ship out-of-the-box with everything from queues to scheduled tasks to admin panels. Laravel especially has a developer-experience story that rivals Rails.
- **Deployment simplicity.** PHP's shared-nothing request model means scaling is trivial: drop more PHP-FPM workers behind a load balancer. No long-running process state, no async/await coloring, no GC tuning.
- **Performance.** PHP 8's JIT plus OPcache gets it within reasonable distance of Node for most web workloads. For raw throughput, [FrankenPHP](https://frankenphp.dev/) (PHP embedded in a Caddy server) and [RoadRunner](https://roadrunner.dev/) close the gap further.

## Tooling worth knowing

- **[Composer](https://getcomposer.org/)** — the package manager. Universally adopted.
- **[Mago](https://mago.carthage.software/)** — see [[mago]]. Rust-based linter / formatter / static analyzer; the modern alternative to the historical PHPStan / Psalm / PHP_CodeSniffer / PHP-CS-Fixer stack.
- **[PHPStan](https://phpstan.org/)** and **[Psalm](https://psalm.dev/)** — the two long-standing static analyzers; both still maintained and very capable.
- **[Pest](https://pestphp.com/)** — modern testing framework; the Laravel-ecosystem default.
- **[FrankenPHP](https://frankenphp.dev/)** — PHP-as-a-Caddy-module; eliminates per-request bootstrapping overhead.

## Articles in this section

- [[mago]] — Mago, the Rust-based all-in-one PHP toolchain.

## Canonical external resources

- [PHP: The Right Way](https://phptherightway.com/) — the de-facto modern style guide.
- [Official PHP docs](https://www.php.net/manual/en/) — the manual itself is good.
- [Stitcher.io blog (Brent Roose)](https://stitcher.io/) — modern-PHP deep dives.

## See also

- [[programming/package_managers/README|package managers]] — Composer context.
- [[programming/rust/README|Rust]] — Mago is one of several Rust-rewrites of PHP-ecosystem tooling.
- [[programming/README|programming]] — parent.

## Keywords

`#php` `#composer` `#laravel` `#symfony` `#mago` `#modern-php` `#programming`
