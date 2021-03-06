Mr Bones
    by Tim Pease
    http://codeforpeople.rubyforge.org/bones

== DESCRIPTION:

Mr Bones is a handy tool that builds a skeleton for your new Ruby projects.
The skeleton contains some starter code and a collection of rake tasks to
ease the management and deployment of your source code. Mr Bones is not
viral -- all the code your project needs is included in the skeleton (no
gem dependency required).

== VERSION 2.0.0 CHANGES:

Version 2.0.0 of Mr Bones introduces backwards incompatibilities. Here is
what you need to update in order for your current projects to work with
this latest release.

Perform an update the tasks in your project directory:

  bones -u your/project/directory

The following .rake files have been renamed in Mr Bones 2.0.0 (the old
name is on the left and the new name is on the right). You will need to
delete the old version and use only the new version.

  doc.rake          => rdoc.rake
  annotations.rake  => notes.rake

The PROJ openstruct has been amended to contain a collection of nested
openstructs. This will affect any settings you might have in the
top-level Rakefile for your project. Here is the translation key (old
names on the left and new names on the right).

  rubyforge_name        => rubyforge.name

  specs                 => spec.files
  spec_opts             => spec.opts

  tests                 => test.files
  test_file             => test.file
  test_opts             => test.opts

  rcov_dir              => rcov.dir
  rcov_opts             => rcov.opts
  rcov_threshold        => rcov.threshold
  rcov_threshold_exact  => rcov.threshold_exact

  rdoc_opts             => rdoc.opts
  rdoc_include          => rdoc.include
  rdoc_exclude          => rdoc.exclude
  rdoc_main             => rdoc.main
  rdoc_dir              => rdoc.dir
  rdoc_remote_dir       => rdoc.remote_dir

  dependencies          => gem.dependencies
  executables           => gem.executables
  extensions            => gem.extensions
  files                 => gem.files
  need_tar              => gem.need_tar
  need_zip              => gem.need_zip
  post_install_message  => gem.extras['post_install_message']

  annotation_exclude    => notes.exclude
  annotation_extensions => notes.extensions
  annotation_tags       => notes.tags      

  svn                   => svn.path
  svn_root              => svn.root
  svn_trunk             => svn.trunk
  svn_tags              => svn.tags
  svn_branches          => svn.branches

  ann_file              => ann.file
  ann_text              => ann.text
  ann_paragraphs        => ann.paragraphs
  ann_email             => ann.email     

And of course, each name should be prepended with PROJ in your Rakefile.

== FEATURES:

Mr Bones provides the following rake tasks:

  ann               # Alias to ann:announcement
  ann:announcement  # Create an announcement file
  ann:email         # Send an email announcement
  bones:debug       # Show the PROJ open struct
  clean             # Remove any temporary products.
  clobber           # Remove any generated file.
  doc               # Alias to doc:rdoc
  doc:rdoc          # Build the rdoc HTML Files
  doc:release       # Publish RDoc to RubyForge
  doc:rerdoc        # Force a rebuild of the RDOC files
  doc:ri            # Generate ri locally for testing
  gem               # Alias to gem:package
  gem:debug         # Show information about the gem
  gem:install       # Install the gem
  gem:package       # Build the gem file bones-1.3.4.gem
  gem:release       # Package and upload to RubyForge
  gem:repackage     # Force a rebuild of the package files
  gem:uninstall     # Uninstall the gem
  git:create_tag    # Create a new tag in the Git repository
  git:show_tags     # Show tags from the Git repository
  manifest          # Alias to manifest:check
  manifest:check    # Verify the manifest
  manifest:create   # Create a new manifest
  notes             # Enumerate all annotations
  notes:fixme       # Enumerate all FIXME annotations
  notes:optimize    # Enumerate all OPTIMIZE annotations
  notes:todo        # Enumerate all TODO annotations
  spec              # Alias to spec:run
  spec:rcov         # Run all specs with RCov
  spec:run          # Run all specs with basic output
  spec:specdoc      # Run all specs with text output
  spec:verify       # Verify that rcov coverage is at least 90.0%
  svn:create_tag    # Create a new tag in the SVN repository
  svn:show_tags     # Show tags from the SVN repository
  test              # Alias to test:run
  test:rcov         # Run rcov on the unit tests
  test:run          # Run tests for run

The rake tasks in the Mr Bones framework can be found in the "tasks"
directory. Add your own tasks there when you need more functionality.

== SYNOPSIS:

To create a new "Get Fuzzy" project:

  bones get_fuzzy

If a new release of Mr Bones comes out with better features the "Get Fuzzy"
project will need to be updated:

  bones --update get_fuzzy

And if you ever get confused about what Mr Bones can do:

  bones --help

== REQUIREMENTS:

Mr Bones does not have any "requirements", but if you do not have the
following gems installed you will not get all that Mr Bones has to offer.

* rubyforge - for easy gem publishing to rubyforge.org
* rcov - for code coverage testing
* rspec - if that's the way you roll
* facets - for pretty colors

== INSTALL:

* sudo gem install bones

== MANUAL:

The +bones+ command line tool is used to create a skeleton for a Ruby
project. In that skeleton is a "tasks" directory that contains the Mr Bones
rake files. These files are quite generic, and their functionality is
controlled by options configured in the top-level Rakefile. Take a look at
the Rakefile for the Mr Bones gem itself:

  load 'tasks/setup.rb'

  ensure_in_path 'lib'
  require 'bones'

  task :default => 'spec:run'

  PROJ.name = 'bones'
  PROJ.authors = 'Tim Pease'
  PROJ.email = 'not.real@fake.com'
  PROJ.url = 'http://codeforpeople.rubyforge.org/bones'
  PROJ.version = Bones::VERSION
  PROJ.rubyforge.name = 'codeforpeople'

  PROJ.rdoc.remote_dir = 'bones'
  PROJ.rdoc.exclude << '^data'

  PROJ.notes.exclude = %w(^README\.txt$ ^data/)

  PROJ.svn.path = 'bones'

  PROJ.spec_opts << '--color'

  # EOF

The +PROJ+ constant is an open struct that contains all the configuration
options for the project. The open struct is created in the "tasks/setup.rb"
file, and that is also where all the configurable options are defined. Take
a look in the "setup.rb" file to see what options are available, but always
make your changes in the Rakefile itself.

The Mr Bones rake system is based on a "Manifest.txt" file that contains a
list of all the files that will be used in the project. If a file does not
appear in the manifest, it will not be included in the gem. Use the
+manifest+ rake tasks to create and update the manifest as needed.

You can exclude files from being seen by the manifest -- the files are
invisible to the Mr Bones rake tasks. You would do this for any subversion
directories, backup files, or anything else you don't want gumming up the
works. The files to exclude are given as an array of regular expression
patterns; a file is excluded if it matches any of the patterns.

  PROJ.exclude = %w(tmp$ bak$ ~$ CVS \.svn ^pkg ^doc)
  PROJ.exclude << '^tags$'

If your project depends on other gems, use the +depend_on+ command in your
Rakefile to declare the dependency. If you do not specify a version, the
most current version number for the installed gem is used.

  depend_on 'logging'
  depend_on 'rake', '0.7.3'

=== Freezing a Skeleton

Freezing allows you to define your own project skeleton to instantiate with Mr
Bones when you create a new project. The default Mr Bones project skeleton is
copied to the ".mrbones" directory in your home directory. From there, the
skeleton can be modified however you like (add new files and tasks, add new
directories, etc).

  bones --freeze

Typical uses of this feature would be to fill in user specific data like the
author, e-mail address, etc. You can also add more default code to the skeleton
project or your own ".rake" task files.

==== Updating a Frozen Skeleton

Just like a regular project, a frozen skeleton can be updated if a newer
version of Mr Bones comes out with better features. Updating a frozen skeleton
will only modify the tasks. All other files in the skeleton remain untouched.

  bones --freeze

The freeze command is used to update the skeleton tasks to the latest Mr Bones
tasks. A copy of the current tasks are made in an "archive" directory in the
".mrbones" directory. To revert back to the archive version, you will need to
manually rename "archive" to "data" in the ".mrbones" directory.

==== Unfreezing a Skeleton

Unfreezing a skeleton will remove your customized project skeleton from the
".mrbones" directory. The default Mr Bones project skeleton will be used
instead. A copy of your custom skeleton is made in the "archive" directory of
the ".mrbones" directory before it is removed.

  bones --unfreeze

==== Custom Modifications

Mr Bones will perform a limited set of substitutions on the files in the
skeleton when it generates a new project. ERB is used to insert the project
name and the derived class name into template files. The file must end in an
".erb" suffix for ERB substitutions to take place. The ".erb" suffix is
stripped from the file during project generation. Therefore, your ERB files
should have two suffixes -- the ".erb" suffix that will be stripped, and the
suffix you want the resulting project file to have.

Only two values can be substituted into files using ERB -- the project name and
the derived class name.

  <%= name %>
  <%= classname %>

The project +name+ is passed in from the command line and used "as is" (with
the exception that spaces are converted to underscores). The +classname+ is
generated by camel casing the project name where the underscores occur.

  get fuzzy  =>  GetFuzzy
  foo_bar    =>  FooBar

Finally, file names can be modified by substituting in the project name. If a
file name contains the string "NAME" it is replaced with the project name. If
our project is call "get fuzzy" then the following transformations will take
place.

  NAME.rb       =>  get_fuzzy.rb
  test_NAME.rb  =>  test_get_fuzzy.rb
  NAME_spec.rb  =>  get_fuzzy_spec.rb

== ACKNOWLEDGEMENTS:

Ryan Davis and Eric Hodel and their Hoe gem (from which much of the Mr Bones
rake tasks have been stolen). The rails team and their source annotation
extractor. Bruce Williams for help in coming up with the project name. Ara T.
Howard for letting me squat in the codeforpeople rubyforge project.

== LICENSE:

MIT License
Copyright (c) 2007 - 2008

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sub-license, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
