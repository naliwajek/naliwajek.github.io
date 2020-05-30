---
layout: post
title: Conda create env with -f configuration file
category: Technical Notes
---
With [conda](http://conda.pydata.org/docs/intro.html), which is a package, dependency and environment manager for any language - but I'm using it for Python, you can create entire environments from a configuration file, like this:

~~~ shell
conda env create -f <path_to_configuration_file>
~~~

And your example configuration may look like this:

~~~ yml
name: CarND-TensorFlow-Lab
dependencies:
- openssl=1.0.2j
- pip>=8.1.2
- psutil=4.4.1
- python>=3.4.0
- readline=6.2
- setuptools=27.2.0
- sqlite=3.13.0
- tk=8.5.18
- wheel=0.29.0
- xz=5.2.2
- zlib=1.2.8
- pip:
  - appnope==0.1.0
  - cycler==0.10.0
  - decorator==4.0.10
  - entrypoints==0.2.2
  - ipykernel==4.5.0
  - ipython==5.1.0
  - ipython-genutils==0.1.0
  - ipywidgets==5.2.2
  - jinja2==2.8
  - jsonschema==2.5.1
  - jupyter==1.0.0
  - jupyter-client==4.4.0
  - jupyter-console==5.0.0
  - jupyter-core==4.2.0
  - markupsafe==0.23
  - matplotlib==1.5.3
  - mistune==0.7.3
  - nbconvert==4.2.0
  - nbformat==4.1.0
  - notebook==4.2.3
  - numpy==1.11.2
  - pexpect==4.2.1
  - pickleshare==0.7.4
  - pillow==3.4.2
  - prompt-toolkit==1.0.8
  - protobuf==3.1.0.post1
  - ptyprocess==0.5.1
  - pygments==2.1.3
  - pyparsing==2.1.10
  - python-dateutil==2.5.3
  - pytz==2016.7
  - pyzmq==16.0.0
  - qtconsole==4.2.1
  - scikit-learn==0.18
  - scipy==0.18.1
  - simplegeneric==0.8.1
  - six==1.10.0
  - sklearn==0.0
  - tensorflow>=0.12.0rc1
  - terminado==0.6
  - tornado==4.4.2
  - tqdm==4.8.4
  - traitlets==4.3.1
  - wcwidth==0.1.7
  - widgetsnbextension==1.2.6
~~~