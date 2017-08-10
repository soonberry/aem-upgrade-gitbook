# AEM 6.1 to 6.3 Upgrade

AEM Upgrade not only includes new fancy features and bug fixing, but also includes a great quantity of dependencies version upgrading.

The dependencies upgrading may cause function changing, class renaming, unit test failure, etc. The main work for upgrade is to ensure the compilation of customize code all goes well.

As AEM is using maven to manage all java dependencies, it's necessary to update the pom file with latest versions (or proper version due to customize code).

Meanwhile, for the purpose of same code base both working for AEM 6.1 and AEM 6.3, using profile to manage dependencies in pom file is quite significant. Same code base will reduce most of dev's work like code conflict or environment inconsistence. 

For platform strategy, we create a seperate sandbox environment to do testing for the first trial. Regression test for both 6.1 and 6.3 envrionment is also needed if profile is applyed in pom.

***

Here I will introduce how to upgrade with recompiling code, the main issues I fixed and some basic platform solution.

- [Re-compile Code](re-compile-code.md)

- [Troubleshooting](troubleshoot.md)

  ​