# OpenEMR Custom Modules

## INTRODUCTION
Why use a module
- Package of functionality independent of the core app
- Administrators can manage functionality
- SOLID
Geared toward small scale projects. For larger, more complex modules you may want to use the Laminas framework.

## CUSTOM SQL
During installation of the module, OpenEMR will run
table.sql 

## FORK THE SKELETON FRAMEWORK
1. Go to adunsulag/oe-module-custom-skeleton in GitHub
2. Click the Fork button to make a fork in your GitHub

## INSTALL SKELETON FRAMEWORK
Take the time to read the instructions provided in the GitHub repo
1. Launch your development IDE and open OpenEMR
2. Change to the /interface/modules/custom_modules directory
3. Clone your fork

```git
git clone git@github.com:KenSchae/oe-module-cutom-skeleton.git
```

## UPDATE composer.json
```
"psr-4": {
    "OpenEMR\\": "src",
    "OpenEMR\\Modules\\CustomModuleSkeleton\\": "interface/modules/custom_modules/oe-module-custom-skeleton/src/"
}
```

Run from the docker folder /docker/development-easy
`composer dump-autoload`


## REFERENCES
- [OpenEMR Modules YouTube](https://youtu.be/LYA8MosIWF0)