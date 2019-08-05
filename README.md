# PetclinicJenkinsPipeline

This is a Jenkins pipeline that:

- Downloads and builds a vulnerable version of Petclinic
- Runs a very basic functional test
- Uses Contrast Security as a quality gate against the result

Prerequisites:

1. Jenkins - tested with version 2.180
2. A Contrast Security account
3. Version 2.6 of the Contrast Security Jenkins Plugin - see https://github.com/jenkinsci/contrast-continuous-application-security-plugin
