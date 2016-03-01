---
layout: post
title:  "Improving UX in OpenStack's Freezer"
date:   2016-02-29 16:30:00
categories: openstack freezer python ux
---


Pending stuff
-------------

  - create an index for log files

Overview
--------

[Freezer](https://wiki.openstack.org/wiki/Freezer) is a backup and restore tool part of the OpenStack infrastructure which has been part of the big tent since liberty release.

Freezer team is planning to grown this project into a complete [disaster recovery](https://review.openstack.org/#/c/278242/) solution, but in order to do that a big refactoring needs to happen, which is to create plugin layers for all the components inside freezer, the idea is to be as much flexible and lean as possible but being modular implies having common interfaces between components and for the user as well. This post will cover that, how to make freezer usable for end users.


Components
----------

freezer is composed of several components:

  - [freezer-agent](https://github.com/openstack/freezer)
  - [freezer-scheduler](https://github.com/openstack/freezer)
  - [freezer-api](https://github.com/openstack/freezer-api)
  - [python-freezerclient](https://github.com/memogarcia/python-freezerclient)
  - [freezer-web-ui](https://github.com/openstack/freezer-web-ui)


Architecture
------------

User Experience
---------------
 - ideas:
    - job shortcuts
    - job templates (is this the same as shortcuts?)
    - python freezer client should install the agent and scheduler as a dependency
    - freezer-agent should only accept --config argument
    - freezer-scheduler should only accept: start, stop, abort jobs
    - all the interaction should be done by the ui or freezerclient
    - Improve the README for all projects
      - but let's start with the ui and freezerclient
