Running a custom/latest Node[.js] version on Red Hat's OpenShift PaaS
====================================================================
This git repository is a sample Node application along with the
"orchestration" bits to help you run the latest or a custom version
of Node on Red Hat's OpenShift PaaS.


Selecting a Node version to install/use
---------------------------------------

To select the version of Node.js that you want to run, just edit or add
a version to the .openshift/markers/NODEJS_VERSION file.

    Example: To install Node.js version 0.12.5, you can run:
       $ echo 0.12.5 >> .openshift/markers/NODEJS_VERSION

    Or alternatively, edit the ```.openshift/markers/NODEJS_VERSION``` file
    in your favorite editor aka vi ;^)


The action_hooks in this application will use that NODEJS_VERSION marker
file to download and extract that Node version if it is available on
nodejs.org and will automatically set the paths up to use the node/npm
binaries from that install directory.

     See: .openshift/action_hooks/ for more details.

    Note: The last non-blank line in the .openshift/markers/NODEJS_VERSION
          file.determines the version it will install.


Okay, now onto how can you get a custom Node.js version running
on OpenShift.


Steps to get a custom Node.js version running on OpenShift
----------------------------------------------------------

Create an account at http://openshift.redhat.com/

Create a namespace, if you haven't already do so

    rhc domain create <yournamespace>

Create a nodejs application (you can name it anything via -a)

    rhc app create -a palinode  -t nodejs-0.10

Add this `github nodejs-custom-version-openshift` repository

    cd palinode
    git remote add upstream -m master git://github.com/ramr/nodejs-custom-version-openshift.git
    git pull -s recursive -X theirs upstream master

Optionally, specify the custom version of Node.js you want to run with
(Default is v0.12.5).
If you want to more later version of Node (example v0.12.42), you can change
to that by just writing it to the end of the NODEJS_VERSION file and
committing that change.

    echo 0.12.42 >> .openshift/markers/NODEJS_VERSION
    #
    # Or alternatively, edit the .openshift/markers/NODEJS_VERSION file
    # in your favorite editor aka vi ;^)
    #
    # Note: 0.12.42 doesn't exist (as yet) and is a fictitious version
    #       mentioned here solely for demonstrative purposes.
    #
    git commit . -m 'use Node version 0.12.42'

Then push the repo to OpenShift

    git push

That's it, you can now checkout your application at:

    http://palinode-$yournamespace.rhcloud.com
    ( See env @ http://palinode-$yournamespace.rhcloud.com/env )

