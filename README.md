# sonarqube
SonarQube demo for ios projects

# how to get it work locally(community edition)
We assume that you have installed java, xcode and homebrew

## Command line tools

Optional

rbenv, manage ruby versions per project, globally, etc

   1. `brew install rbenv ruby-build`
   2. add to .bash_profile
      * `if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi`
   3. install your ruby version(at the time of writing this is 2.7.0)
      * `rbenv install 2.7.0`
   4. create a `.ruby-version` file inside the root of your project with the version of your selected ruby version
      * `2.7.0`

Mandatory

1. swiftlint, A tool to enforce Swift style and conventions
   * `brew install swiftlint`
   
2. Install fastlane and slather gem, two options:

   A. Create Gemfile in your root project and run `bundle install`
      ```ruby
      source "https://rubygems.org"
      gem "fastlane"
      gem "slather"
      ```
      
   B. Install directly from command line
   
      Slather, Generate test coverage reports for Xcode `gem install slather`
      
      Fastlane, Automate building and releasing your iOS and Android apps `sudo gem install fastlane -NV`

## Tools

1. SonarQube
    * [download](https://www.sonarqube.org/downloads/)
    * Move unzipped to `/Applications/` folder.
    * Optional, delete version suffix. `SonarQube`
  
2. SonarScanner
   * [download](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)
   * Move unzipped to `/Applications/` folder.
   * Optional, delete version suffix. `SonarScanner`
   * add to bash path
     * open `.bash_profile`
     * add(be carefull with the naming of the directories, here it has deleted the suffix)

    ```bash
        # Sonar Setting
        export PATH=$PATH:/Applications/SonarScanner/bin
        export PATH=$PATH:/Applications/SonarQube/bin
    ```

3. SonarSwift
   * [download jar](https://github.com/Idean/sonar-swift/releases)
   * Move to `/Applications/SonarQube/extensions/plugins/`(be carefull with the naming again)

## Configure

1. Have your xcode project opened to check names for configuration

2. Create sonar configuration file in the root of your project, `sonar-project.properties`
   * REPLACE keyword will show you which items you have to change

    ```bash
    #
    # Swift SonarQube Plugin - Enables analysis of Swift and Objective-C projects into SonarQube.
    # Copyright © 2015 Backelite (${email})
    #
    # This program is free software: you can redistribute it and/or modify
    # it under the terms of the GNU Lesser General Public License as published by
    # the Free Software Foundation, either version 3 of the License, or
    # (at your option) any later version.
    #
    # This program is distributed in the hope that it will be useful,
    # but WITHOUT ANY WARRANTY; without even the implied warranty of
    # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    # GNU Lesser General Public License for more details.
    #
    # You should have received a copy of the GNU Lesser General Public License
    # along with this program.  If not, see <http://www.gnu.org/licenses/>.
    #

    sonar.host.url=http://localhost:9000
    sonar.login=admin
    sonar.password=admin

    sonar.projectKey=com.github.albertopeam.SonarQubeDemo # REPLACE for your project key, I will use bundle identifier to avoid collisions in SonarQube
    sonar.projectName=SonarQubeDemo # REPLACE for your project name, I will use the project name. Both will be used later to configure in the SonarQube panel

    # Number version (can be found automatically in plist, just comment this line)
    sonar.projectVersion=1.0    # REPLACE for your vers
    # Comment if you have a project with mixed ObjC / Swift
    sonar.language=swift
    # Project description
    sonar.projectDescription=Project demo for SonarQube  # REPLACE for your description
    # Path to source directories
    sonar.sources=SonarQubeDemo # REPLACE for your sources directory/ies
    # Path to test directories (comment if no test)
    sonar.tests=SonarQubeDemoTests  # REPLACE for your unit and/or ui tests
    sonar.test.inclusions=**/*Test*/**
    sonar.test.inclusions=*.swift
    sonar.exclusions=**/*.xml,Pods/**/*,Reports/**/*

    # Destination Simulator to run surefire
    # As string expected in destination argument of xcodebuild command
    # Example = sonar.swift.simulator=platform=iOS Simulator,name=iPhone 6,OS=9.2
    sonar.swift.simulator=platform=iOS Simulator,name=iPhone 11,OS=13.3 # REPLACE for your simulator
    # Xcode project configuration (.xcodeproj)
    # and use the later to specify which project(s) to include in the analysis (comma separated list)
    # Specify either xcodeproj or xcodeproj + xcworkspace
    sonar.swift.project=SonarQubeDemo.xcodeproj # REPLACE for your xcodeproj
    #sonar.swift.workspace=testSonar.xcworkspace
    # Specify your appname.
    # This will be something like "myApp"
    # Use when basename is different from targeted scheme. 
    # Or when slather fails with 'No product binary found'
    sonar.swift.appName=SonarQubeDemo # REPLACE for your app name
    # Scheme to build your application
    sonar.swift.appScheme=SonarQubeDemo # REPLACE for your app scheme
    # Configuration to use for your scheme. if you do not specify that the default will be Debug
    sonar.swift.appConfiguration=Debug
    ##########################
    # Optional configuration #
    ##########################
    # Encoding of the source code
    sonar.sourceEncoding=UTF-8
    # SCM
    # sonar.scm.enabled=true
    # sonar.scm.url=scm:git:http://xxx
    # JUnit report generated by run-sonar.sh is stored in sonar-reports/TEST-report.xml
    # Change it only if you generate the file on your own
    # The XML files have to be prefixed by TEST- otherwise they are not processed
    sonar.junit.reportsPath=sonar-reports/
    # Lizard report generated by run-sonar.sh is stored in sonar-reports/lizard-report.xml
    # Change it only if you generate the file on your own
    sonar.swift.lizard.report=sonar-reports/lizard-report.xml
    # Cobertura report generated by run-sonar.sh is stored in sonar-reports/coverage-swift.xml
    # Change it only if you generate the file on your own
    sonar.swift.coverage.reportPattern=sonar-reports/cobertura.xml
    # OCLint report generated by run-sonar.sh is stored in sonar-reports/oclint.xml
    # Change it only if you generate the file on your own
    sonar.swift.swiftlint.report=sonar-reports/swiftlint.txt
    # Change it only if you generate the file on your own
    #sonar.swift.tailor.report=sonar-reports/*tailor.txt
    # Paths to exclude from coverage report (surefire, 3rd party libraries etc.)
    sonar.swift.excludedPathsFromCoverage=build,DerivedData,fastlane,Pods,reports,testSonarTests,testSonarUITests,xcov_output
    sonar.swift.excludedPathsFromCoverage=.*Tests.*
    ##########################
    # Tailor configuration #
    ##########################
    # Tailor configuration
    # -l,--max-line-length=<0-999>                  maximum Line length (in characters)
    #    --list-files                               display Swift source files to be analyzed
    #    --max-class-length=<0-999>                 maximum Class length (in lines)
    #    --max-closure-length=<0-999>               maximum Closure length (in lines)
    #    --max-file-length=<0-999>                  maximum File length (in lines)
    #    --max-function-length=<0-999>              maximum Function length (in lines)
    #    --max-name-length=<0-999>                  maximum Identifier name length (in characters)
    #    --max-severity=<error|warning (default)>   maximum severity
    #    --max-struct-length=<0-999>                maximum Struct length (in lines)
    #    --min-name-length=<1-999>                  minimum Identifier name length (in characters)
    sonar.swift.tailor.config=--no-color --max-line-length=100 --max-file-length=500 --max-name-length=40 --max-name-length=40 --min-name-length=4
    ```

3. Fastlane: it will build, test and generate all needed reports
    * `fastlane init`
    * Edit `Fastfile` or provide your own
    * REPLACE keyword will show you which items you have to change
  
    ```bash
    # This file contains the fastlane.tools configuration
    # You can find the documentation at https://docs.fastlane.tools

    default_platform(:ios)

    platform :ios do
    desc "Description of what the lane does"
    lane :metrics do

        scan(scheme: "SonarQubeDemo",  # REPLACE
        code_coverage: true, 
        derived_data_path: "./DerivedData", 
        output_directory: "./sonar-reports",
        devices: "iPhone 11") # REPLACE
        
        slather(cobertura_xml: true, 
        jenkins: true, 
        scheme: "SonarQubeDemo", # REPLACE
        build_directory: "./DerivedData", 
        output_directory: "./sonar-reports", 
        proj: "./SonarQubeDemo.xcodeproj") # REPLACE

        swiftlint(output_file: "./sonar-reports/swiftlint.txt", 
        ignore_exit_status: true)

        sonar
    end
    end
    ```

## Run

1. Start SonarQube
   * `sh /Applications/SonarQube/bin/macosx-universal-64/sonar.sh console`

2. Enter SonarQube & create a project
   * Wait until sonar is up
   * Access `http://localhost:9000/` in the browser
     * credentials admin, admin
   * Create new project(+ button)
     * use the project key and name that you have used on your `sonar-project.properties`
     * create
     * next screen can be ignored

3. Analyze
   * run `fastlane metrics` in the project root
   * wait until it finishes
   * reload `SonarQube` dashboard in browser to see reports

## .gitignore

1. adding some extra stuff to `.gitignore`to avoid uploading to the repository

    ```bash
    sonar-reports/*
    .scannerwork/*
    ```

# Bibliography

1. https://medium.com/@nishabe/how-to-setup-up-sonarqube-for-swift-project-732002684cbb
2. https://medium.com/@pranay.urkude/sonarqube-integration-with-ios-b76df8405014
3. https://medium.com/@aamir.ali/sonarqube-integration-with-fastlane-in-ios-3cd33e5abdc8
4. https://hackernoon.com/the-only-sane-way-to-setup-fastlane-on-a-mac-4a14cb8549c8
5. https://github.com/fastlane/fastlane
6. https://github.com/rbenv/rbenv
