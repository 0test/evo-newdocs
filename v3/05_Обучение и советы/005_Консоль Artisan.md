# Консоль Artisan в EvolutionCMS

План статьи:

1. зачем. упомянуть про создание своей команды и ссылку дать
2. использование и про cd core сказать
3. Часто используемые команды, пара слов о каждой
4. перевод команд

```
Usage:
  command [options] [arguments]

Options:
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  clear-compiled           Remove the compiled class file
  completion               Dump the shell completion script
  extras                   Extras
  help                     Display help for a command
  list                     List commands
  migrate                  Run the database migrations
 cache
  cache:clear              Flush the application cache
  cache:clear-full         Full cache clear blade + Evolution
  cache:forget             Remove an item from the cache
 closuretable
  closuretable:rebuild     Rebuild tree Closure Table
 db
  db:seed                  Seed the database with records
 doc
  doc:list                 Documents from the site_content table
 make
  make:migration           Create a new migration file
  make:site                Update site
 migrate
  migrate:fresh            Drop all tables and re-run all migrations
  migrate:install          Create the migration repository
  migrate:refresh          Reset and re-run all migrations
  migrate:reset            Rollback all database migrations
  migrate:rollback         Rollback the last database migration
  migrate:status           Show the status of each migration
 package
  package:create           Install composer package
  package:discover         Generate ServiceProviders for custom packages
  package:installautoload  Install composer package
  package:installrequire   Install composer package
  package:runconsoles      Run console command from custom packages
 route
  route:list               List all registered routes
 salo
  salo:install             Install Laravel Salo's default Docker Compose file
  salo:publish             Publish the Laravel Salo Docker files
 tpl
  tpl:list                 Templates from the site_template table
 tv
  tv:list                  TV lists
 vendor
  vendor:publish           Publish any publishable assets from vendor packages
 view
  view:clear               Clear all compiled view files
```
