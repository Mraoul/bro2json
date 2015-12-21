bro2json
========

bro2json is a simple python utility that converts Bro logs in standard bro format into json by using the commented header fields to learn about the formatting. This attemps to create json logs which are a close approximation to using the use_json option as part of the AsciiWriter.

This utility is ideally meant for logs that have already been created and I would recommend you configure bro to output directly into json if you are planning on feeding the data to an upstream location.

Usage
-----
<pre>
usage: bro2json [-h] [-e EXCLUDE [EXCLUDE ...]] [-o OUTPUT]
                   [-g {always,source,never}] [-p] [-v]
                   source [source ...]

Convert bro formatted logs to json logs.

positional arguments:
  source                Source[s] to convert, could be a file or a directory
                        of files, or multiple files or directories

optional arguments:
  -h, --help            show this help message and exit
  -e EXCLUDE [EXCLUDE ...], --exclude EXCLUDE [EXCLUDE ...]
                        If processing a directory exclude files that begin
                        with these strings
  -o OUTPUT, --output OUTPUT
                        An output destination directory to write the data,
                        defaults to cwd, use "-" to write to stdout
  -g {always,source,never}, --gzip {always,source,never}
                        Determine whether output files will be gzip
                        compressed, "source" will gzip compress if the source
                        was -- option ignored if output is stdout
  -p, --path            Store the bro 'path' field as '@path' in the json
                        output helpful if processing output in an automated
                        fashion
  -v, --verbose         Be Verbose
</pre>

###Converting a file

```bash
$ ./bro2json /opt/logs/2015-11-11/conn.00\:00\:00-01\:00\:00.log.gz
```

The above command will create a file called conn.00:00:00-01:00:00.log.gz in the current working directory that is gzip compressed and in json format

###Converting multiple files

To convert multiple files simply list all of them at the command line

```bash
$ ./bro2json /opt/logs/2015-11-11/conn.00\:00\:00-01\:00\:00.log.gz /opt/logs/2015-11-11/conn.01\:00\:00-02\:00\:00.log.gz
```

The above command will create two files corresponding to the two files you passed in.


###Converting a directory

To convert an entire directory, simply pass the path to the directory

```bash
$ ./bro2json /opt/logs/2015-11-11/
```

The above command will create a directory called '2015-11-11' in your current working directory and place all converted files into that directory. Further, note that since not all files within a logging directory are bro-formatted, the 'exclude' flag (-e) is honored and defaults to some common logs found in bro's logging directories that are generally not as useful as others (e.g., stdout, stderr, etc). Check the variable at the top of the file to see which files are excluded by default.
