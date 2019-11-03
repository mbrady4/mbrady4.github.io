+++
title = "Creating a Python Package"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
1. Navigate to correct directory
2. Create a virtual environment

    pipenv --python 3

3. Launch virtual environment

    pipenv shell

4. Install dependencies

    pipenv install numpy pandas
    pipenv install -d twine

5. Create [setup.py](http://setup.py) file

    """ lambdata - a collection of Data Science helper functions
    """
    
    import setuptools
    
    REQUIRED = [
        "numpy",
        "pandas"
    ]
    
    with open("README.md", "r") as fh:
        LONG_DESCRIPTION = fh.read()
    
    setuptools.setup(
        name="data-explorer",
        version="0.0.1",
        author="mwbrady",
        description="A collection of Data Science helper functions",
        long_description=LONG_DESCRIPTION,
        long_description_content_type="text/markdown",
        url="https://github.com/mbrady4/lambdata",
        packages=setuptools.find_packages(),
        python_requires=">=3",
        install_requires=REQUIRED,
        classifiers=[
            "Programming Language :: Python :: 3",
            "License :: OSI Approved :: MIT License",
            "Operating System :: OS Independent",
        ],
    )

5. Build local distribution (all files will automatically be added to .gitignore)

    # Build the package locally to check that it is working 
    # (all build files added to .gitignore)
    python setup.py sdist bdist_wheel

5. Upload to pypi (test). Need to enter username and password

    twine upload --repository-url https://test.pypi.org/legacy/ dist/*

7. Package should now be accessible on the pypi (test) site at the following: [https://test.pypi.org/project/data-explorer/](https://test.pypi.org/project/data-explorer/) ... change package name in URL as needed.

8. Installing the package

    # Replace data-explorer with package name
    pip install -i https://test.pypi.org/simple/ data-explorer

9. Import package sub-directory

    # This is a sub-directory within my package folder
    import lambdata_mbrady4

10. Run Methods

    from lambdata_mbrady4 import feature_explorer
    
    # Data_Dict is a method within the feature_explorer file
    feature_explorer.data_dict(king, king['price'])

### Other helpful commands

    # Used to install pipenv to make it accesible globally
    sudo -H pip install -U pipenv