Katana ORM
==========

Katana ORM is a fork of the glory Propel ORM but with some fundamental changes.

 * [ ] UnitOfWork - allows mass insertion/update and separation of repository and entity
 * [ ] Optional ActiveRecord
 * [ ] JIT Compiler - instead of having a build time it can create the needed stuff on the fly
 * [ ] JIT Migration - instead of manually migrate your database we do it for you if needed automatically
 * [ ] NoSQL adapter - in the first version some basic support for some noSQL databases
 * [ ] Better/Easier “behavior” development (plugins)
 * [ ] Entity definition through xml, yml, php, database schema, annotations
 * [ ] Removed hard dependencies on SQL databases
 * [ ] With a REST API Adapter

That all with Propel’s initial **blazing fast approach** of compiling instead of doing always a
database/object introspection, means it will be incredibly fast.


Insight
-------

All following steps can be done without having a external build-time (console command executions).

### Quick-Start:

```php
# bootstrap
Katana::setDefaultRepository('mysql', 'localhost', 'root', 'myHeavyPassword');
Katana::setCompileDirectory('app/cache/');
Katana::getDefaultRepository()->addObject('Author');

# example
class Author {
    use Author\ObjectEncapsulation;
    use Author\ActiveRecord;

    /**
     * @Field(type="string")
     */
    protected $name;
}

$author = new Author();
$author->setName('Hans Zimmer');
$author->save();
```

Nothing else to do for you.
What we do in the background: migrate database' schema and build all necessary traits.

### Reverse

Working with already existing table.

```php
class Author {
    use Author\Reverse;
    use Author\ObjectEncapsulation;
    use Author\ActiveRecord;

    protected $repositoryContainer = 'authors';
}

$author = AuthorQuery::create()->findOneByName('Hans Zi');
$author->setName('Hans Zimmer');
$author->save();
```

### Centralized definition

repositories.yml
```yml
repositories:
    default:
        adapter: mysql
        host: localhost
        user: root
        password: myHeavyPassword
```

objects.yml
```yml
Namespaced\Author:
    repository: default
    active_record: false
    fields:
        name: string
        points: integer
```

```php
Katana::readRepositoryDefinition('path/to/repositories.yml');
Katana::readObjectDefinition('path/to/objects.yml');

$authors = Namespaced\AuthorQuery::create()->find();
foreach ($authors as $author) {
    $author->setPoints(0);
}

Katana::getRepository()->flush(); // writes all changes at once
...
```
