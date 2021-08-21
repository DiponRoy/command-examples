
# npm
<p align="left">
    <a href="https://www.python.org" target="_blank"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/npm/npm-original-wordmark.svg" alt="python" width="40" height="40" /></a>
    <a href="https://www.typescriptlang.org/" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-original.svg" alt="typescript" width="40" height="40" /> </a>
    <a href="https://nodejs.org" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original-wordmark.svg" alt="nodejs" width="40" height="40" /> </a>
</p>

npm is a package manager for the JavaScript programming
 -  **official**: https://docs.npmjs.com/cli/v7/commands
 - https://devhints.io/npm
 - https://www.freecodecamp.org/news/npm-cheat-sheet-most-common-commands-and-nvm/
 - https://www.sitepoint.com/npm-guide/

 
## Most Used Commands
**Version**
```
node -v		#node version
npm -v		#npm version
```
**Initiate**
```
npm init
```
**Install/Update/Uninstall package**
```
npm install package_name	# install latest available package version
npm update package_name		# update package to latest available version
npm uninstall package_name	# remove installed package
```
**Show installed packages**
```
npm list
```
**Install/Update/Uninstall packages**
```
npm install		# install libraries in the file along with their dependencies
npm update		# update libraries in the file along with their dependencies
```   
**Run project**
```
npm start
npm run script_name
```
## npm init
```
npm init	#Initiate a project
npm init -y	#Initiate a project without answering questions
```
## npm install
```
npm install package_name				# install latest available package version
npm install package_name1 package_name2	# install multiple packages to latest available versions		
```
```
npm -i package_name		# install latest available package version
```
## pip install
```
pip install package_name				# install latest available version
pip install package_name==2.3			# install specific version
pip install 'package_name>=2.3'			# any version above specified minimum version
pip install 'package_name>=3.0,<4.0' 	# any version above and below between specified minimum and maximun version
```
```
pip install -r requirements.txt		# installs libraries in the file along with their dependencies
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

## npm list
```
npm list    			# show installed packages in the tabular format
npm list -g --depth 0	# show packages installed in the local virtual environment
npm outdated			# show outdated packages
npm view				# show up-to-date packages
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
pip show package_name	# show information about a specified package
```
## pip check
```
pip check	#verify whether installed packages have compatible dependencies
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
