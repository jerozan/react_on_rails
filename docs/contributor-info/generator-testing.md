# Generator Testing
We create several applications that are examples of running the generator (see lib/generators/react_on_rails/install_generator.rb) with various different options. We can then run tests with these apps just like we would any Rails app and thus ensure that our generator makes apps that actually function properly.

Special rake tasks in rakelib/examples.rake handle creating the example apps and running a special hidden generator (lib/generators/react_on_rails/dev_tests_generator.rb) that installs the tests we want to run for each app. These tests can be run manually like any Rails app, or they can be run via the `rake run_rspec:examples` command. There are also commands for running each app individually, i.e., `rake run_rspec:example_basic`.

## Travis and Gemfiles
We are currently using Travis for CI. Because of the way Travis works, it is not possible to `bundle install` multiple Gemfiles. Therefore, we have placed all dependencies for generated apps in the gem's main Gemfile. If you generate an app that has a new gem dependency in its Gemfile, you need to add that dependency to the main Gemfile or it will not work in CI.

## Configuring what Apps are Generated
You can specify additional apps to generate and test by adding to the rakelib/examples_config.yml file. The necessary build and test tasks will automatically be created for you dynamically at runtime.

## More on the Rake Tasks
In order to maximize efficiency, we took several steps to improve the performance of the rake tasks that utilize somewhat advanced rake functionality such as task dependencies, `file` tasks, task synthesizing, and concurrent tasks with `multitask`.

For example, re-generating the app, running `yarn`, and re-generating the webpack bundles are all only done when they need to be done. Rake will also run certain tasks, including those that generate multiple applications, concurrently. 

For more insight, see:

- [Avdi Grimm's series of articles on Rake](http://devblog.avdi.org/2014/04/30/learn-advanced-rake-in-7-episodes/)
- [Martin Fowler's rake article](http://martinfowler.com/articles/rake.html)
