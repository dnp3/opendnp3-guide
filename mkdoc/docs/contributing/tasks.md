
### Formatting the code

There is an astyle configuration in the config folder

```sh
astyle -R ./cpp/*.h --options=./config/astyle.cfg
astyle -R ./cpp/*.cpp --options=./config/astyle.cfg
```

### Formatting the license

There is a maven POM with a plugin to do this. From the root directory:

```
> mvn -f formatHeaders.xml license:format
```

### Getting code coverage

You can build the libraries and exes with gcov support

```sh
> cmake -DSTATICLIBS=ON -DCOVERAGE=ON
> make
```

Then run the test suites to output coverage information.

```sh
> ./testopendnp3
> ./testopenpal
```
Then generage an info file with lcov

```sh
> lcov -c -d ./ -b ./ -o coverage.info
```

and then process this into html

```
> genhtml coverage.info -o test_html
```

