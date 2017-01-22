# QtCreator for Emacs Users

QtCreator has **tragic** support for Emacs users. I used the following
steps to make QtCreator Emacs friendly:

* Did checkout of QtCreator source code.
* Removed `Alt-...` shortcuts/quick keys in the menu as they **CANNOT** 
  be disabled using configuration.
* Built QtCreator.
* Customized key binding to match Emacs as close as possible.



### How to checkout and build QtCreator

* Presumptions:
    * Installed QtCreator
	* Ubuntu 14.04+
* Checkout QtCreator:
```
git clone --recursive https://code.qt.io/qt-creator/qt-creator.git

# note --recursive above (checkout QBS) sub-module, if you don't need
# it (I had problem to checkout module corresponding to trunk) simply
# skip --recursive (I suggest it):

git clone https://code.qt.io/qt-creator/qt-creator.git
```
* You just checkout the **latest** QtCreator source code, but
  typically you don't have the latest Qt installed in your distro
  (I also don't want to have >1 Qt versions in the system) and actually
  you don't want it - you want the version you use. Lets rebuild
  the version of QtCreator you have in the system as you will have
  required most dependencies:
    * Determine QtCreator version: 
	  `qtcreator -version` 
	  e.g.
	  `Qt Creator 3.0.1 based on Qt 5.2.1`
* Install dev packages for your qt:
  `sudo apt-get install qtbase5-dev`
* Install **private** build packages:
  `sudo apt-get build-dep qtcreator`
* List tags in Git checkout:
  `git tag -l | grep 3.0.1`
* Checkout tag corresponding to QtCreator you have:
  `git checkout tags/v3.0.1`
* If you did `--recursive` checkout, you also have to checkout 
  corresponding QBS sub-module (I failed to find the right one, therefore 
  I skipped QBS above as a workaround i.e. I don't have any suggestion for QBS).
* Check the status w/:
  `git status`
* Build QtCreator as described in:
  http://wiki.qt.io/Building_Qt_Creator_from_Git
    * Check QMake version: `qmake -v` (must be on path and no error)
	* Prepare Makefile:
```	
	  # cd to directory containing qt-creator/ directory with Git repository
	  mkdir qt-creator-build
	  # now qt-creator/ and qt-creator-build/ are on the same level
	  cd qt-creator-build
	  qmake -r ../qt-creator/qtcreator.pro
```
* Configure and make:
```
      # if you are not in build directory, then cd to it
      cd qt-creator-build
      qmake -r ../qt-creator/qtcreator.pro
      make -j <number-of-cpu-cores+1>
	  # I used `make -j 3` (having four cores to avoid machine trashing) 
	  # if `make -j ...` fails, try to run it w/ one core just by running `make`
```

Troubleshooting:

* Project ERROR: Unknown module(s) in QT: script
    * ... check reference below... or `sudo apt-cache search qt | grep script`
	* `sudo apt-get install qtscript5-dev`
* Project ERROR: Unknown module(s) in QT: help
    * ... check reference below...
	* `sudo apt-get install qttools5-dev`
* Package to lib reference:
```
Qt module           dev package                         lib package
-------------------------------------------------------------------------
bluetooth           qtconnectivity5-dev                 libqt5bluetooth5
concurrent          qtbase5-dev                         libqt5concurrent5
connectivity        qtmobility-dev                      libqtconnectivity1
contacts            qtmobility-dev                      libqt5contacts5
core                qtbase5-dev                         libqt5core5a
dbus                qtbase5-dev                         libqt5dbus5
designer            qttools5-dev                        libqt5designer5
designercomponents  qttools5-dev                        libqt5designercomponents5
feedback            qtmobility-dev                      libqt5feedback5
gallery             qtmobility-dev                      libqtgallery1
gui                 qtbase5-dev                         libqt5gui5
help                qttools5-dev                        libqt5help5
location            qtmobility-dev                      libqt5location5
multimedia          qtmultimedia5-dev                   libqt5multimedia5
multimediakit       qtmobility-dev                      libqtmultimediakit1
network             qtbase5-dev                         libqt5network5
nfc                 qtconnectivity5-dev                 libqt5nfc5
opengl              libqt5opengl5-dev                   libqt5opengl5
organizer           qtmobility-dev                      libqt5organizer5
positioning         qtpositioning5-dev                  libqt5positioning5
printsupport        qtbase5-dev                         libqt5printsupport5
publishsubscribe    qtmobility-dev                      libqt5publishsubscribe5
qml                 qtdeclarative5-dev                  libqt5qml5
quick               qtdeclarative5-dev                  libqt5quick5
quickparticles      qtdeclarative5-dev                  libqt5quickparticles5
quicktest           qtdeclarative5-dev                  libqt5quicktest5
quickwidgets        qtdeclarative5-dev                  libqt5quickwidgets5
script              qtscript5-dev                       libqt5script5
scripttools         qtscript5-dev                       libqt5scripttools5
sensors             qtmobility-dev, libqt5sensors5-dev  libqt5sensors5
serialport          libqt5serialport5-dev               libqt5serialport5
serviceframework    qtmobility-dev                      libqt5serviceframework5
sql                 qtbase5-dev                         libqt5sql5
svg                 libqt5svg5-dev                      libqt5svg5
systeminfo          qtmobility-dev                      libqt5systeminfo5
test                qtbase5-dev                         libqt5test5
uitools             qttools5-dev                        ?
versit              qtmobility-dev                      libqtversit1
webchannel          ?                                   ?
webkit              libqt5webkit5-dev                   libqt5webkit5
websockets          libqt5websockets5-dev               libqt5websockets5
widgets             qtbase5-dev                         libqt5widgets5
x11extras           libqt5x11extras5-dev                libqt5x11extras5
xml                 qtbase5-dev                         libqt5xml5
xmlpatterns         libqt5xmlpatterns5-dev              libqt5xmlpatterns5
```
