# Dev-Project
![image](https://github.com/user-attachments/assets/278c0a45-ec16-4150-b0fe-60bcad4265c5)

Stage-1 Build & Unit Test
  Build Artifacts
  Unit test
Tool: Maven & Gradle

Stage-2 Code Coverage (build & unit Test Required)
  How many lines of code you tested? Means- How many lines we have in our code and how many lines unit test covered? We get the result in some percentage
  Unused Code
Tool: Jacoco

Stage-3 Software Composition analysis (Need just raw code, No build required)
  Identify vulnerabilities introduced by open-source or 3rd party libraries used in code
Tool: OWASP Dependency-check

Stage-4 Static Application Security Testing(SAST) - No need to run application or build but we can test without running the app. that's what static means.
  Identify vulnerabilities in proprietary code(is it hackable, any memory leaking, ducplicate code, any vulnerabilities)
  Insecure Coding practices
Tool: SonarQube

Stage-5 Quality Gates
  Check if application meets the quality standards
  you will get the result of code coverage, sca and sast
  If my code coverage above to 85 % then will qay our code is good
  If I get a vulnerabilities list and if its not above medium or high then it will pass 
Tool: Sonar Quality profile

Stage-7 Scan Docker Image
  Identify vulnerabilities in image layers
  In image we have OS layer, Dependency layer and application layer and so far we have done testing at application layer so to check other layers we need to scan docker image
Tool: Trivy


https://gitlab.com/learndevopseasy/devsecops/springboot-build-pipeline
