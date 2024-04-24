---
title: 'Useful mxmlc command line options'
date: 2009-12-10T00:00:00-07:00
tags: ["actionscript"]
summary: "Options for mxmlc compiler: file specification, output location, source paths, script limits, incremental builds, and loading configurations."
---

- `-file-spec File1.mxml [File2.mxml ...]`  
  Used to specify the file names which are to be compiled by the mxmlc compiler.

- `-output dir/out.swf`  
  Use this option to specify the output directory and/or file name.

- `-source-path dir1 [dir2 ...]`  
  By default, the mxmlc compiler looks in the current directory only for the required files. If you are using some source files which are located in another directory or multiple directories, use this option to specify those locations.

- `-default-script-limits 150 10`  
  The first parameter (e.g. 150) specifies the maximum level of recursion allowed. The second parameter defines the maximum script execution time (in seconds).

- `-incremental=true`  
  By default, the mxmlc compiler does a clean build each time it is invoked. To use incremental builds, specify this option. In case of incremental builds, the compiler compiles only those files which have changed from the last build.

- `-load-config=config.xml`  
  Instead of specifying the compiler options on the command line, you can put all the options in the config.xml file. By default, the mxmlc compiler reads the parameters from the flex-config.xml file in the SDK/frameworks directory. However, if you use this option, the flex-config.xml file is ignored altogether. Many of the options specified in that file are required for mxmlc to run. Hence, you’ll need to specify all the required options in your custom configuration file.

- `-load-config+=config.xml`  
  Since specifying all the required values in the config.xml file is painful, use this option to load your configuration file in addition to the default flex-config.xml file. The values in your config.xml file override the default values in the flex-config.xml file. The remaining values are read from the flex-config.xml file.