===============================
stack
===============================

User focused CLI for OpenStack

Stack is a CLI for OpenStack designed to make it easy for users who are not
deeply familiar with OpenStack to perform common tasks.

* Free software: Apache license
* Documentation: http://docs.openstack.org/developer/stack
* Source: http://git.openstack.org/cgit/openstack/stack
* Bugs: http://bugs.launchpad.net/stack

Proposal
--------

OpenStack has a problem with user experience. It's fantastic that we provide
people with the power to customize their deployments and extend OpenStack in
many different ways however it current comes at the cost of a common, sane
"Getting Started" guide that can make use of the cross-cloud standards that
have been promised.

There is still so much of using OpenStack that is dependent upon the particular
cloud deployment you use, and whilst this will always be the case there should
be an easy way to do the basic things that all OpenStack deployments do.

Features
--------

* Only the operations that a user commonly performs.

  Stack CLI will specifically not cover what are considered administrative
  operations. For example everything about keystone beyond initial
  authentication, or Ironic, or setting up ACLs, or anything that isn't the
  basic operations a user could perform on ANY cloud.

* Focus on OpenStack as a whole.

  From a developer's perspective there are very good reasons that all the
  separate OpenStack projects exist that a user simply should not care about.
  Operations should be performed on OpenStack not on nova or neutron or other
  project.

* Hides the complexity of multiple APIs.

  With all existing CLIs you must directly deal with a specific API version or
  (god forbid) a microversion.

* Excessive errors messaging.

  OpenStack can be hard and there are lots of cryptic messages. The stack CLI
  should provide full paragraph explanations of problems and common remedies to
  fix issues by default.

Goals
-----

* An easy way to get started with OpenStack.

  For most people their interaction with OpenStack will be here is a file
  containing common authentication configuration for a cloud, now go and create
  virtual machines and networks in them.

  They may or may not ever need the more advanced or management features. We
  want to optimize for this common user.

* Drive the API independent "next layer" of the OpenStack SDK.

  The OpenStack SDK project is nearly at a 1.0 release with the first layer of
  API version specific features for services. This should become the building
  block for producing more common functionality that doesn't require specific
  API versions or understanding microversioning.

  Stack CLI would be a consumer of the SDK and provide a feedback channel as to
  its requirements for version independent and cross service functions..

* Determine the discoverability issues in OpenStack.

  We have accepted that there are differences between OpenStack clouds however
  to successfully work with multiple clouds you need to determine what those
  differences are and respond appropriately. We need to sure up

* Not the latest and greatest.

  In general features that are only available in the most recent API release
  are not a candidate. Internally stack CLI can use the most appropriate APIs
  for that cloud however the project is about getting the basics right, not
  supporting the latest cool thing.

* Opinionated.

  Similar to the last point stack should have the flexibility to make educated
  decisions on what a user wants to achieve. It will be configurable but not
  provide every possibly option to a user so as to be overwhelming. If a user
  needs more power than what stack provides then they will want to use
  OpenStack CLI for that task.

FAQ
---

* Why not use the OpenStack CLI?

  This is not intended as a replacement or even a competitor to the OpenStack
  CLI project. The problem with OpenStack CLI is that there is still an
  expectation that users have a good understanding of how OpenStack works.
  Depending on the way you set different environment variables different
  options are available, similarly different options become available depending
  on what packages are installed on the machine.

  This is always going to be needed to do more advanced tasks and perform
  administrative actions. However for the basic user doing `openstack --help`
  is an overwhelming list of options that they should not need to use.

  Whilst we could always extend OpenStack CLI with more options that perform
  these simpler tasks I don't believe we would ever successfully manage the
  experience to target both novice and advanced users in the same application.

* Why now?

  There are a few projects that have really needed to come together to make
  this feasible.

  - Keystoneauth has now had it's initial 1.0 release providing a
    standard way to extend a variety of authentication mechanisms to clients.

  - OpenStack SDK will be releasing a stable 1.0 version that depends on
    keystoneauth's authentication mechanisms.

  - os-client-config has provided a cleaner mechanism for managing a user's
    authentication such that you can use the same config file for OpenStack
    CLI, stack CLI and different service specific CLIs.

  We are quickly arriving at the point where there are some compelling client
  side standards that we can create a new project without reinventing the wheel
  yet again.
