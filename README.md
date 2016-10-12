GraspIt! User Manual Documentation
==============================================

This is the source for the GraspIt! User Manual,
which is  hosted at http://graspit-simulator.github.io/

This Documentation borrows heavily from:
https://github.com/fetchrobotics/docs

Building New Releases
---------------------

Install prerequisites:

```
sudo pip install sphinx
```

Build new release:

```
git clone git@github.com:graspit-simulator/graspit-simulator.github.io_src.git
make whatever changes to the source that you would like
make html
git remote add build git@github.com:graspit-simulator/graspit-simulator.github.io.git
rm -rf source 
git rm -r source 
git add build
git commit -m "new build"
git push build master --force
```

Troubleshooting:

If you want to view changes locally, run:
```
python -m SimpleHTTPServer 8080
```

If you get complaints about .sty files install the following

```
sudo apt-get install texlive-full
```
