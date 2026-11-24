# Changelog
## [0.4.2]
### Added
- Added `treatPhpDocTypesAsCertain: false` to the PHPStan config
- Added `configs/phpcs.xml` ruleset with the excluded sniffs and ignored paths (now also `*/Test/*`)
- Added `excludePaths` to the PHPStan config

### Changed
- Raised the default PHPStan level from 5 to 9 and moved it to `configs/phpstan.neon`; `phpstan.level` now defaults to `~`
- `phpcs.standard` now points to `configs/phpcs.xml`; `phpcs.exclude`, `phpcs.ignore_patterns` and `phpcs.whitelist_patterns` default to empty
- Moved the PHPStan ignored paths to `excludePaths` in `configs/phpstan.neon` (now also `*/app/bootstrap.php` and `*/.php-cs-fixer.php`); `phpstan.ignore_patterns` defaults to empty

## [0.4.1]
### Fixed
- Fixed php-cs-fixer config to only search in existing directories

## [0.4.0]
### Added
- Updated php-cs-fixer finder directories to use both `./app/code` and `./src`

## [0.3.0]
### Added
- Added ParallelConfig to php-cs-fixer

## [0.2.0]
### Added
- Fixed some dependencies

## [0.1.0]
### Added
- Added GrumPHP conventions used on Magento projects 
