Katana ORM
==========

Katana ORM is a fork of the glory Propel ORM but with some fundamental changes.

 * UnitOfWork - allows mass insertion/update and separation of repository and entity
 * Optional ActiveRecord
 * JIT Compiler - instead of having a build time it can create the needed stuff on the fly
 * JIT Migration - instead of manually migrate your database we do it for you if needed automatically
 * NoSQL support - in the first version some basic support for some noSQL databases
 * Better/Easier “behavior” development (plugins)
 * Entity definition through xml, yml, php, database schema, annotations

That all with Propel’s initial **blazing fast approach** of compiling instead of doing always a
database/object introspection, means it will be incredibly fast.
