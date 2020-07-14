---
layout:     post
title:      "AWS Machine Learning Foundations Part Two"
date:       2020-05-24 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Machine Learning
    - Udacity
---
This is part two of the notes of the course [AWS Machine Learning Foundations](https://classroom.udacity.com/courses/ud090) on Udacity.
<!--more-->
### Why Object-Oriented Programming?
Object-oriented programming has a few benefits over procedural programming, which is the programming style you most likely first learned. As you'll see in this lesson,

- object-oriented programming allows you to create large, modular programs that can easily expand over time;
- object-oriented programs hide the implementation from the end-user.

#### Characteristics and Actions in English Grammar
Another way to think about characteristics and actions is in terms of English grammar. A characteristic would be a noun. On the other hand, an action would be a verb.

Let's pick something from the real-world: a dog. A few characteristics could be the dog's weight, color, breed, and height. These are all nouns. What actions would a dog take? A dog can bark, run, bite and eat. These are all verbs.

### Object-Oriented Programming (OOP) Vocabulary
- class - a blueprint consisting of methods and attributes
- object - an instance of a class. It can help to think of objects as something in the real world like a yellow pencil, a small dog, a blue shirt, etc. However, as you'll see later in the lesson, objects can be more abstract.
- attribute - a descriptor or characteristic. Examples would be color, length, size, etc. These attributes can take on specific values like blue, 3 inches, large, etc.
- method - an action that a class or object could take
- OOP - a commonly used abbreviation for object-oriented programming
encapsulation - one of the fundamental ideas behind object-oriented programming is called encapsulation: you can combine functions and data all into a single entity. In object-oriented programming, this single entity is called a class. Encapsulation allows you to hide implementation details much like how the scikit-learn package hides the implementation of machine learning algorithms.
In English, you might hear an attribute described as a property, description, feature, quality, trait, or characteristic. All of these are saying the same thing.

Accessing attributes directly would be frowned upon in many other languages but not in Python. Instead, the general object-oriented programming convention is to use methods to access attributes or change attribute values. These methods are called set and get methods or setter and getter methods. Refer to [Properties vs. Getters and Setters](https://www.python-course.eu/python3_properties.php)

### Advanced OOP Topics
Here is a list of resources for advanced Python object-oriented programming topics.

- [class methods, instance methods, and static methods](https://realpython.com/instance-class-and-static-methods-demystified/) - these are different types of methods that can be accessed at the class or object level
- [class attributes vs instance attributes](https://www.python-course.eu/python3_class_and_instance_attributes.php) - you can also define attributes at the class level or at the instance level
- [multiple inheritance, mixins](https://easyaspython.com/mixins-for-fun-and-profit-cb9962760556) - A class can inherit from multiple parent classes
- [Python decorators](https://realpython.com/primer-on-python-decorators/) - Decorators are a short-hand way for using functions inside other functions

### Making a Package
A package is a collection of Python modules. A Python package contained multiple files, a Python package also needs an `__init__.py` file. 

A basic package structure looks like below.
```
package_name/
    README.txt
    setup.py
    package_name/
          __init__.py
          module1.py
          test/
              __init__.py
              test_module1.py
```
To install the local packge, change to the directory with the `setup.py` file. Then type `pip install .` or `pip install -e .` which is the devlop mode.

Pip is a [Python package manager](https://pip.pypa.io/en/stable/) that helps with installing and uninstalling Python packages. You might have used pip to install packages using the command line: `pip install numpy`. When you execute a command like `pip install numpy`, pip will download the package from a Python package repository called [PyPi](https://pypi.org/).

### PyPi vs. Test PyPi
Note that pypi.org and test.pypy.org are two different websites. You'll need to register separately at each website. If you only register at pypi.org, you will not be able to upload to the test.pypy.org repository.

You'll need to create a setup.cfg file, README.md file, and license.txt file. You'll also need to create accounts for the pypi test repository and pypi repository. 

Also, remember that your package name must be unique. If you use a package name that is already taken, you will get an error when trying to upload the package.

Summary of the Terminal Commands Used in the Video
```bash
cd binomial_package_files
python setup.py sdist
pip install twine

# commands to upload to the pypi test repository
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
pip install --index-url https://test.pypi.org/simple/ dsnd-probability

# command to upload to the pypi repository
twine upload dist/*
pip install dsnd-probability
```

#### More PyPi Resources
Tutorial on distributing packages
This link has a good tutorial on distributing Python packages including more configuration options for your setup.py file: [tutorial on distributing packages](https://packaging.python.org/tutorials/packaging-projects/). You'll notice that the python command to run the setup.py is slightly different with

```bash
python3 setup.py sdist bdist_wheel
```
This command will still output a folder called `dist`. The difference is that you will get both a .tar.gz file and a .whl file. The .tar.gz file is called a *source archive* whereas the .whl file is a *built distribution*. The .whl file is a newer type of installation file for Python packages. When you pip install a package, pip will first look for a whl file (wheel file) and if there isn't one, will then look for the tar.gz file.

A tar.gz file, ie an sdist, contains the files needed to compile and install a Python package. A whl file, ie a built distribution, only needs to be copied to the proper place for installation. Behind the scenes, pip installing a whl file has fewer steps than a tar.gz file.

Other than this command, the rest of the steps for uploading to PyPi are the same.

### Reference
- [OOP Github](https://github.com/udacity/DSND_Term2/tree/master/lessons/ObjectOrientedProgramming)
- [Example Google Style Python Docstrings](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
- [Beginner's Guide to Contributing to a Github Project](https://akrabat.com/the-beginners-guide-to-contributing-to-a-github-project/)
- [Contributing to a Github Project](https://github.com/MarcDiethelm/contributing/blob/master/README.md)
- [Overview of PyPi](https://docs.python.org/3/distutils/packageindex.html)
- [MIT License](https://opensource.org/licenses/MIT)
- [Making a Python Package](https://python-packaging-tutorial.readthedocs.io/en/latest/setup_py.html)

