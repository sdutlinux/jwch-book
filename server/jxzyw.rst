教学资源网迁移记录
=====================

项目主页: https://github.com/jwch/teaching

原因:

#. 由原先asp程序，迁移到ruby程序。这样就可以不用2003, sql server,减少维护成本，新同学的学习成本
#. 框架打算用ruby的 padrina,这个框架是基于sinatra 框架的包装，添加常用的工具。轻量级的

padrina:

1. http://www.padrinorb.com/
2. https://github.com/padrino/padrino-framework


笔记
-----------------------

命令行工具－Project Generator

项目生成::
  
  $ padrino g project <the_app_name> </path/to/create/app> --<component-name> <value>

详细: http://www.padrinorb.com/guides/generators 





开发记录
-------------------------

创建项目::
    
  padrino g project teaching  -t rspec -d activerecord 

generators controller example::
  
  padrino g controller welcome get:index 

查看所有的rake task::
  
  $ padrino rake -T

  => Executing Rake -T ...
  rake ar:abort_if_pending_migrations  # Raises an error if there are pending migrations
  rake ar:charset                      # Retrieves the charset for the current environment's database
  rake ar:collation                    # Retrieves the collation for the current environment's database
  rake ar:create                       # Create the database defined in config/database.yml for the current Padrino.env
  rake ar:create:all                   # Create all the local databases defined in config/database.yml
  rake ar:drop                         # Drops the database for the current Padrino.env
  rake ar:drop:all                     # Drops all the local databases defined in config/database.yml
  rake ar:forward                      # Pushes the schema to the next version.
  rake ar:migrate                      # Migrate the database through scripts in db/migrate and update db/schema.rb by invoking ar:schema:dump. Target specific version with VERSION=x. Turn ...
  rake ar:migrate:down                 # Runs the "down" for a given migration VERSION.
  rake ar:migrate:redo                 # Rollbacks the database one migration and re migrate up.
  rake ar:migrate:reset                # Resets your database using your migrations for the current environment
  rake ar:migrate:up                   # Runs the "up" for a given migration VERSION.
  rake ar:reset                        # Drops and recreates the database from db/schema.rb for the current environment and loads the seeds.
  rake ar:rollback                     # Rolls the schema back to the previous version.
  rake ar:schema:dump                  # Create a db/schema.rb file that can be portably used against any DB supported by AR
  rake ar:schema:load                  # Load a schema.rb file into the database
  rake ar:setup                        # Create the database, load the schema, and initialize with the seed data
  rake ar:structure:dump               # Dump the database structure to a SQL file
  rake ar:translate                    # Generates .yml files for I18n translations
  rake ar:version                      # Retrieves the current schema version number
  rake gen                             # Generate the Rakefile
  rake routes[query]                   # Displays a listing of the named routes within a project, optionally only those matched by [query]
  rake routes:app[app]                 # Displays a listing of the named routes a given app [app]
  rake secret                          # Generate a secret key
  rake seed                            # Load the seed data from db/seeds.rb
  rake spec                            # Run complete application spec suite
  rake spec:app                        # Run RSpec code examples


查看路由::
  
  padrino rake routes 
   


创建model, 创建section model 用于存放栏目::

  padrino g model section name:string  
  
db/migrate/001_create_sections.rb::

  class CreateSections < ActiveRecord::Migration
    def self.up
      create_table :sections do |t|
        t.string :name
        t.timestamps
      end
    end
  
    def self.down
      drop_table :sections
    end
  end
 
  

然后执行,建表::
  
  padrino rake ar:migrate

输出::
  
  => Executing Rake ar:migrate ...
  DEBUG -  (0.2ms)  select sqlite_version(*)
  DEBUG -  (22.3ms)  CREATE TABLE "schema_migrations" ("version" varchar(255) NOT NULL)
  DEBUG -  (0.1ms)  PRAGMA index_list("schema_migrations")
  DEBUG -  (17.1ms)  CREATE UNIQUE INDEX "unique_schema_migrations" ON "schema_migrations" ("version")
  DEBUG -  (0.2ms)  SELECT "schema_migrations"."version" FROM "schema_migrations" 
   INFO - Migrating to CreateSections (1)
  DEBUG -  (0.1ms)  begin transaction
  ==  CreateSections: migrating =================================================
  -- create_table(:sections)
  DEBUG -  (0.5ms)  CREATE TABLE "sections" ("id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, "name" varchar(255), "created_at" datetime NOT NULL, "updated_at" datetime NOT NULL) 
   -> 0.0022s
  ==  CreateSections: migrated (0.0023s) ========================================

  DEBUG -  (0.1ms)  INSERT INTO "schema_migrations" ("version") VALUES ('1')
  DEBUG -  (15.4ms)  commit transaction
  DEBUG -  (0.2ms)  SELECT "schema_migrations"."version" FROM "schema_migrations"
  DEBUG -  (0.1ms)  PRAGMA index_list("sections")

创建 section controller, 需要inex,列出所有的section 还需要new, create 方法创建 section::
  
    $ padrino g controller section get:index  get:new post:create 
    create  app/controllers/section.rb
    create  app/helpers/section_helper.rb
    create  app/views/section
    apply  tests/rspec
    create  spec/app/controllers/section_controller_spec.rb

get:index 的意思是 以http 的get 方式访问index, post:create 的意思是访问crate 的时候是以post 方式访问的

