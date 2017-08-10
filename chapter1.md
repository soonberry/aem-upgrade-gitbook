# Introduction

AEM Upgrade not only includes new fancy features and bug fixing, but also includes a great quantity of dependencies version upgrade.

The dependencies upgrade may cause function change, class rename, unit test failure, etc. The main work for upgrade is ensure compiling of customize code all go through.

As AEM is using maven to manage all java dependencies, just need to update the pom file with latest versions of dependecies.

Meanwhile, for the purpose of same code base both working for AEM 6.1 and AEM 6.3, using profile to manage dependencies in pom file is quite needed. Same code base will reduce most of dev's work like code conflict or environment inconsistence. 

For platform strategy, we create a seperate sandbox environment to do testing first. Regression test for 6.1 and 6.3 envrionment is also needed if profile is applyed in pom.