# PetclinicJenkinsPipeline

This is a Jenkins pipeline that:

- Downloads and builds a vulnerable version of Petclinic
- Runs a very basic functional test
- Uses Contrast Security as a quality gate against the result

Prerequisites:

Version 2.6 of the Contrast Security Jenkins Plugin - see https://github.com/jenkinsci/contrast-continuous-application-security-plugin
