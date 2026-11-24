# GrumPHP Magento Convention

This package is to be required on all PHPro's Magento projects.

## Configuration 
The project `grumphp.yml` should contain at least:
```
imports:
    - { resource: './vendor/phpro/magento-conventions/grumphp-convention.yml' }
    
parameters:
  git.project: (FOO)
```

### Configurable parameters

#### GENERAL
| Parameter                | Default                                       | Info |
|--------------------------|-----------------------------------------------| --- |
| **root_conventions_dir** | vendor/phpro/magento-conventions/configs/     | Relative path to the config directory of the convention installation |

#### PHPCS
The excluded sniffs, ignored paths and the `app/code` whitelist live in `configs/phpcs.xml`, so a direct `vendor/bin/phpcs` run uses the same rules as GrumPHP.

The convention no longer sets `phpcs.exclude`, `phpcs.ignore_patterns` or `phpcs.whitelist_patterns`. Configure project specific sniff exclusions and ignored paths in your own ruleset. Create a `phpcs.xml.dist` in the project root that extends the convention ruleset:
```xml
<?xml version="1.0"?>
<ruleset name="Project">
    <rule ref="./vendor/phpro/magento-conventions/configs/phpcs.xml">
        <exclude name="Magento2.Some.Sniff"/>
    </rule>
    <exclude-pattern>*/Legacy/*</exclude-pattern>
</ruleset>
```
Then point GrumPHP to it in `grumphp.yml`:
```yaml
parameters:
  phpcs.standard: [phpcs.xml.dist]
```
An exclude pattern from the convention ruleset cannot be removed in a ruleset that extends it. A project that needs to check code outside `app/code` needs its own ruleset with the rules copied from `configs/phpcs.xml`.

| Parameter | Default | Info |
| --- | --- | --- |
| **phpcs_ruleset_file** | phpcs.xml | Relative path to the phpcs ruleset |
| **phpcs.standard** | [ '%root_conventions_dir%%phpcs_ruleset_file%' ] | The standards to check code styling |
| **phpcs.severity** | ~ | Global severity level |
| **phpcs.warning_severity** | ~ | Warning severity level |
| **phpcs.error_severity** | ~ | Error severity level |
| **phpcs.triggered_by** | [ php ] | Triggered by |

#### PHP CS FIXER
The rules, risky setting, cache setting and checked paths are defined in `configs/.php-cs-fixer.php`. If you want to change them, create your own `.php-cs-fixer.php` file in your project and point `phpcsfixer.config` to it.

| Parameter | Default | Info |
| --- | --- | --- |
| **phpcs_file** | .php-cs-fixer.php | Relative path to CS config file |
| **phpcsfixer.cache_file** | ~ | Create a .php_cs cache file |
| **phpcsfixer.config** | '%root_conventions_dir%%phpcs_file%' | Path to php CS config file |
| **phpcsfixer.config_contains_finder** | true | Configuration file uses a finder |
| **phpcsfixer.verbose** | true | Verbose |
| **phpcsfixer.diff** | false | Diff |
| **phpcsfixer.triggered_by** | [ 'php' ] | Php triggered by files |

#### PHPLINT
| Parameter | Default | Info |
| --- | --- | --- |
| **phplint.exclude** | [ 'vendor' ] | Excluded directories |
| **phplint.jobs** | ~ | Lint jobs |
| **phplint.short_open_tag** | false | Short open tags |
| **phplint.ignore_patterns** | [ ] | ignore patterns |
| **phplint.triggered_by** | [ 'php', 'phtml' ] | Triggered by files |

#### PHPSTAN
Existing Magento 2 projects should run at least level `5` and work up to the standard level `9` step by step. Set the level in your own `phpstan.neon`.

The ignored paths are defined as `excludePaths` in `configs/phpstan.neon`.

Configure project specific PHPStan settings (level, excluded paths, ignored errors, stubs) in your own `phpstan.neon` that includes the convention config:
```neon
includes:
    - vendor/phpro/magento-conventions/configs/phpstan.neon

parameters:
    level: 9
    excludePaths:
        analyse:
            - */Legacy/*
    ignoreErrors:
        - identifier: missingType.iterableValue
```
Then point GrumPHP to it in `grumphp.yml`. The convention no longer sets `phpstan.level` or `phpstan.ignore_patterns`; the level and excluded paths come from the neon file:
```yaml
parameters:
  phpstan.configuration: ./phpstan.neon
```

| Parameter | Default | Info |  
| --- | --- | --- |
| **phpstan_file** | phpstan.neon | Relative path to PHPStan neon file |
| **phpstan.autoload_file** | ~ | Autoload files |
| **phpstan.configuration** | '%root_conventions_dir%%phpstan_file%' | Path to configuration file |
| **phpstan.triggered_by** | [ 'php' ] | PHPStan triggered files |
| **phpstan.memory_limit** | "-1" | Memory limit PHPStan uses |
| **phpstan.use_grumphp_paths** | true | Use GrumPHP paths |

#### PHPUNIT
Make sure you create a `phpunit.xml` file in your project root.

| Parameter | Default | Info |  
| --- | --- | --- |
| **phpunit.config_file** | ~ | Path to phpunit.xml  |
| **phpunit.testsuite** | ~ | Run tests for certain suite |
| **phpunit.group** | [ ] | Run tests for certain group |
| **phpunit.always_execute** | true | Always run the whole test suite, even if no PHP files were changed |
| **phpunit.order** | null | Run tests in a specific order |

#### COMPOSER
| Parameter | Default | Info |
| --- | --- | --- |
| **composer.file** | ./composer.json | File location |
| **composer.no_check_all** | false | Check all |
| **composer.no_check_lock** | true | Check lock file |
| **composer.no_check_publish** | true | Check publish |
| **composer.no_local_repository** | true | Check for local repo's |
| **composer.with_dependencies** | true | Check dependencies |
| **composer.strict** | false | Set composer to strict |

#### GIT
| Parameter                                | Default | Info                                                                         |
|------------------------------------------| --- |------------------------------------------------------------------------------|
| **git.project**                          | PROJECT| Project abbreviation code used in Jira. Use (FOO\|BAR) for multiple projects |
| **git_commit_message.matchers**          | ['/%git.project%-([0-9]*)/'] | Match commit message to PROJECT-{number}...                                  |
| **git_commit_message.max_subject_width** | 70 | Git commit subject message length                                            |
| **git_commit_message.max_body_width**    | 80 | Git commit body message length                                               |
| **git_commit_message.case_insensitive**  | true | Message is case sensitive                                                    |
| **git_commit_message.multiline**         | true | Message can contain multiple lines                                           |
| **git_branch_name.blacklist**            | [ "development", "production", "staging", "master" ]          | List of non-wanted branch names                                              |
| **git_branch_name.whitelist**            | Check grumphp-convention.yml   | Allowed branch names                                                         |
| **git_blacklist.keywords**               | Check grumphp-convention.yml   | Disallowed words                                                             |
| **git_blacklist.triggered_by**           | ['php', 'js', 'html', 'phtml']| Triggered by files                                                           |
| **git_branch_name.allow_detached_head**  | true | Allow detached head                                                          |

#### FILE SIZE
| Parameter | Default | Info |
| --- | --- | --- |
| **file_size.size** | 5M | The maximum file size |
| **file_size.ignore_patterns** | [ ] | Patterns that will be ignored |

#### CLOVER
| Parameter | Default       | Info                                                     |
| --- |---------------|----------------------------------------------------------|
| **clover_coverage.clover_file** | ./.clover.xml | Path to the clover coverage report                       |
| **clover_coverage.minimum_level** | 0             | Clover minimum level, this will hard fail if not reached |
| **clover_coverage.target_level** | 100           | Clover target level, this will soft fail if not reached  |

#### SECURITYCHECKER COMPOSERAUDIT
| Parameter                                     | Default  | Info                                         |
|-----------------------------------------------| --- |----------------------------------------------|
| **securitychecker_composeraudit.format**      | null     |                                              |
| **securitychecker_composeraudit.locked**      | true     |                                              |
| **securitychecker_composeraudit.no_dev**      | false    |                                              |
| **securitychecker_composeraudit.run_always**  | true     |                                              |
| **securitychecker_composeraudit.working_dir** | null     |                                              |
| **securitychecker_composeraudit.abandoned**   | ignore   | Ignore abandoned packages, Magento uses some |
