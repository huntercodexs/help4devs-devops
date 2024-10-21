
# NPM HELPER

## General Overview

###### Create one NPM module

- Create the account in the NPM (https://www.npmjs.com/)

- Create one node project

- Execute npm init to start the project generation
<pre>
npm init
# Fill up the enrollment
</pre>

- Create one Public GitHUB repository
- 
- Check the current npm user
<pre>
	npm whoami
</pre>

- Entry the project folder
<pre>
	cd /home/user/project
</pre>

- Make the login in the NPM account
<pre>
npm login
Inform username
Inform password
Inform email
</pre>

- Check if the package.json exists

<pre class="code-style">
"repository": {"type": "git", "url": "git://github.com/vendor/project"}
</pre>

- Publish the NPM module

<pre>
npm publish
</pre>

### Remove the project from NPM

<pre>
npm login
npm unpublish --force yourPackage
</pre>

> TIP: If you remove one package from NPM modules, probably it will be needed to hold on for about 24 hours to publish
> another NPM package module that has the same name.
