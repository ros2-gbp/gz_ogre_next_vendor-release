^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package gz_ogre_next_vendor
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

0.2.0 (2025-11-24)
------------------
* Bump minor for Rolling
* Mark gz_cmake_vendor as a build dependency #10
* Merge pull request `#9 <https://github.com/gazebo-release/gz_ogre_next_vendor/issues/9>`_ from gazebo-release/jrivero/missing_atomic
  * Missing atomic depends. 0.2.0
* Fix use of gz-cmake vendor package (`#6 <https://github.com/gazebo-release/gz_ogre_next_vendor/issues/6>`_)
  This ensures that this works with gz-cmake4 as well
* Add check for system installed ogre-next (`#5 <https://github.com/gazebo-release/gz_ogre_next_vendor/issues/5>`_)
  * Check for system installed ogre-next first.
  * Add exact/relaxed version matching
  * Revert change to github url
  ---------
  Co-authored-by: Rhys Mainwaring <rhys.mainwaring@me.com>
* Contributors: Addisu Z. Taddese, Jose Luis Rivero, Øystein Sture

0.1.0 (2024-05-07)
------------------
* Fix build on arm64  (`#3 <https://github.com/gazebo-release/gazebo_ogre_next_vendor/issues/3>`_)
  Co-authored-by: Jose Luis Rivero <jrivero@osrfoundation.org>
* Contributors: Addisu Z. Taddese

0.0.4 (2024-04-22)
------------------
* Fix build error due to glslang
* Contributors: Addisu Z. Taddese

0.0.3 (2024-04-19)
------------------
* Add additional dependencies, fix vulkan build
* Sort dependencies
* Contributors: Addisu Z. Taddese

0.0.2 (2024-04-08)
------------------
* Change version to 0.0.1
* Fix linter (`#2 <https://github.com/gazebo-release/gazebo_ogre_next_vendor/issues/2>`_)
* Add README (`#1 <https://github.com/gazebo-release/gazebo_ogre_next_vendor/issues/1>`_)
* Rename package to gz_ogre_next_vendor
* Remove unused flag
* Add license and contributing files
* Fix rosdep keys in package.xml
* Fix patch to work on Jammy
* Aligned with  build flags from our fork
* Ogre Next Vendor package
* Contributors: Addisu Z. Taddese, Michael Carroll
