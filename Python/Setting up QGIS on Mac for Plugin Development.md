
I was having some trouble getting plugin development working on my MacBook Pro, so I figured I’d write up a guide on how I’ve gotten it working. I use the plugin builder([QGIS Python Plugins Repository](https://plugins.qgis.org/plugins/pluginbuilder/)) to build plugins and to get to libraries linked and work here’s the steps I took.


## Prerequisites
One issue I found running the installer commands was an error `Error: Too many open files @ rb_sysopen`, which was fixed by running `ulimit -n 1024`.

## Installation
```
brew tap osgeo/osgeo4mac
brew install qgis
```

## Post-Installation
After getting errors from post-installation regarding dal I ran
```
brew unlink osgeo-gdal osgeo-proj osgeo-libspatialite osgeo-gdal-python
brew install https://raw.githubusercontent.com/OSGeo/homebrew-osgeo4mac/f6e7b27c7485afbdc5a8d9844231fd2e64f02659/Formula/osgeo-gdal.rb
brew install https://raw.githubusercontent.com/OSGeo/homebrew-osgeo4mac/b37f82142e347c293cf0b7992f6187d40116b2ca/Formula/osgeo-proj.rb
brew install https://raw.githubusercontent.com/OSGeo/homebrew-osgeo4mac/27f4ac2bc4056750307cee1b7a6abb9e33c846dd/Formula/osgeo-libspatialite.rb
brew install https://raw.githubusercontent.com/OSGeo/homebrew-osgeo4mac/de5db18027bc37cd3bb0d4be07f59fdc8fe55df9/Formula/osgeo-gdal-python.rb
```

Then run

```
brew unlink spatialindex
brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/ff7b5ee0ea31d7af9c4561e20783a1997fb7dbaf/Formula/spatialindex.rb
brew pin spatialindex
```

And 

```
brew reinstall ninja gsl sip six bison flex pkg-config python qt pyqt qscintilla2
$ brew link —overwrite pyqt

```
## Base Plugin Setup
After installing QGIS, install the plugin builder(found here or via the QGIS [QGIS Python Plugins Repository](https://plugins.qgis.org/plugins/pluginbuilder/)) and generate the plugin filling in the information you want. I typically want to use things like Makefiles and Unit Tests, so I have it generate all of that.

## Link Generated Plugin into QGIS
After generating your project, you’ll find(as I did) running `make test` generates a bunch of errors as libraries aren’t linked as expected. In the generated project there seems to be instructions for linking using Linux but not for Mac. Here’s the Mac equivalent I wrote