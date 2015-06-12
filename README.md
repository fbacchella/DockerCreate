This tool is provided to supersed Dockerfile, with easier to write command, less intermediate images and smart content 
detection.

Given a yaml template file, it create a script, and generate a docker file that will execute it.

The template is made of parts, each one generating a specific kind of action: installing packages, extracting tar,
installing files, or custom script.

The template can contain variable that will be resolved at execution time.

A template must contains the following items:
 - tag, map to `--tag=` in docker build command
 - from, map to `FROM` in the Dockerfile
 - maintainer, map to `MAINTAINER` in the Dockerfile
 - environment, map to `ENV` in the Dockerfile, it must be an mapping
 - command, map to `CMD` in the Dockerfile
 - entrypoint, map to `ENTRYPOINT` in the Dockerfile
 - execute, a sequence of step to execute.

Step types
==========

A step is made of a mapping that define it.

The most common key is `type` that define the kind of action that this step is.

There is 3 possible way to define the content of a step:
- the key `content`, it will be used as is.
- the key `source`, it's the path to a file, it will be copied
- the key `url`, the content will be downloaded

Content can be given with multi line, in the yaml way:

    content: |
        line 1
        line 2
        
Initial space will be ignored
        
packages
--------

A set of package to install

script
------

A shell script to execute, it will be executed with bash

script
------

A command to execute.


file
----
A file to install

Additionnal keys can be given:

- file_name, the final file name, inside the image
- mode, a octal set of permission
- owner, the final owner of the file, in the format user:group ; it can be symbolic or numeral, but it will be resolved
within the image.

Variables
=========

The template can use variable, they are defined in the format ${{variable_name}}.

They are given as `-v name value` in the command line