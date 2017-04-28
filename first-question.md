# Angular \(2+, 4+\) - Setting the base tag in index.html for a sub-folder for deploying to hosted server at build time.

E.g. you want to deploy to an external server but not into the root of that server rather to a subfolder, like so:

                NOT to this directory **www.dthuesen.de**

                BUT to this directory **www.dthuesen.de/subdirectory**



**Than you have to do this:**

Running a deployment of Angular in a subdirectory requires to set the base tag in index.html to the desired sub-folder by running the build command with the according option: Usage: **ng build --base-href &lt;base&gt; **or** ng build --base-href /myUrl/ **or** ng build --bh /myUrl/**

so in the example "www.dthuesen.de/subdirectory" the basetag would be written instead

~~&lt;**base href="/"&gt;**~~

it would be replaced with

&lt;**base href="/home"&gt;**

