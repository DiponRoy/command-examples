# pip
pip is a package manager for Python packages


## Most Used Commands
**Install/Update/Uninstall package**
```
pip install package_name		# install latest available package version
pip install -U package_name		# update installed package to latest available version
pip uninstall package_name		# uninstall installed package		
```
**Show installed packages**
```
pip list
```
**Install/Update/Uninstall packages from requirements.txt**
```
pip install -r requirements.txt			# install libraries in the file along with their dependencies
pip install -U -r requirements.txt		# update libraries in the file along with their dependencies
pip uninstall -r requirements.txt 		# uninstall libraries from requirements.txt
```   
**Generate requirements.txt**
```
pip freeze > requirements.txt
```
## pip install
```
pip install package_name				# install latest available version
pip install package_name==2.3			# install specific version
pip install 'package_name>=2.3'			# any version above specified minimum version
pip install 'package_name>=3.0,<4.0'	# any version above and below between specified minimum and maximun version
```
```
pip install -r requirements.txt			# installs libraries in the file along with their dependencies
```

## pip update
```
pip install -U package_name				# upgrade a single installed package to latest available version
```
```
pip install -U -r requirements.txt		# upgrade libraries in the file along with their dependencies
```

## pip uninstall
```
pip uninstall package_name 			# confirm before uninstall
pip uninstall -y package_name		# uninstall without confirmation
```
```
pip uninstall -r requirements.txt 	# Generage requirements.txt
```

## pip list
```
pip list    # show installed packages in the tabular format
pip list -l # show packages installed in the local virtual environment
pip list -o	# show outdated packages
pip list -u	# show up-to-date packages
```
## pip freeze
```
pip freeze						# installs all installed packages in the global environment
pip freeze -l					# only lists packages installed in the local virtual environment 
```
```
pip freeze > requirements.txt	# generaget requirements.txt from installed packages
```
## pip show
```
pip show package_name			# show information about a specified package
```
## pip check
```
pip check						#verify whether installed packages have compatible dependencies
```
## pip search
```
pip search package_name
```
## need to add
```
--force-reinstall
```
```
--no-cache-dir
```
```
pip install pip-review
pip-review --interactive
https://stackoverflow.com/a/56798606/2948523
```

## References

 - https://dev.to/dev0928/commonly-used-python-pip-commands-255d
 - https://dzone.com/articles/most-important-quotpipquot-commands-for-a-python-d
 -  **master**: https://pip.pypa.io/en/stable/cli/
