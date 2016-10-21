
### Formatting the code

There is a cmake "format" task available in every build system. It requires
having [Astyle](http://astyle.sourceforge.net/) on your path.

### Formatting the license

There is a maven POM with a plugin to do this. From the root directory:

```
> mvn -f formatHeaders.xml license:format
```

### Getting code coverage

You can build the libraries and exes with gcov support

```sh
> cmake -DSTATICLIBS=ON -DCOVERAGE=ON -DCMAKE_BUILD_TYPE=Debug
> make
```

Then run the test suites to output coverage information.

```sh
> ./testopendnp3
> ./testopenpal
```
Then generate an info file with lcov

```sh
> lcov -c -d ./ -b ./ -o coverage.info
```

and then process this into html

```
> genhtml coverage.info -o test_html
```
