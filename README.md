
commands

ocp:                 oc get pods | grep -v Completed
ocpw:               oc get pods -w | grep -v Completed
oced 'string':    oc edit deployment (deployment name containing 'string')
ocl 'string':        oc logs (pod name containing 'string')
oclf 'string':       oc logs (pod name containing 'string') -f
ocr 'string'         oc rsh (pod name containing 'string')
ocdp 'string'      oc describe pod (pod name containing string')
ocproj 'string'    oc project (project name containing 'string')

Any command that locates a pod name will have the full pod name stored as $p for future use
Example:
`ocl asd` will pull search for a pod name containing `asd` like `zxcasdqwe`. The command will then be `oc logs `zxcasdqwe` with the name `zxcasdqwe` stored as $p. This allows for fast follow on commands like `oc delete pod $p`


.bashrc
-----------------------------------------------

export HISTSIZE=10000
export HISTFILESIZE=10000

alias ocpw='oc get pods -w | grep -v "Completed"'

function ocp { cmd='oc get pods | grep -v Completed'; echo $cmd; eval $cmd; }
export -f ocp

#-----------------------

function finddeploymentnames { d="$(oc get deployment | awk '/'$1'/ {print $1}')"; t="$(echo $d | gawk 'match($0, /(.*)-.*-.*/, a) {print a[1]}')";}
export -f finddeploymentnames

function oced { finddeploymentnames $1; echo "oc edit deployment $d"; oc edit deployment $d; }
export -f oced

#-----------------------

function findauthpolicynames { ap="$(oc get AuthorizationPolicy | awk '/'$1'/ {print $1}')"; t="$(echo $ap | gawk 'match($0, /(.*)-.*-.*/, a) {print a[1]}')";}
export -f findauthpolicynames

function oceap { findauthpolicynames $1; echo "oc edit AuthorizationPolicy $ap"; oc edit AuthorizationPolicy $ap; }
export -f oceap

#-----------------------

function findpodnames { p="$(ocp | awk '/'$1'/ {print $1}')"; t="$(echo $p | gawk 'match($0, /(.*)-.*-.*/, a) {print a[1]}')";}
export -f findpodnames

function ocl { findpodnames $1; echo "oc logs $p"; oc logs $p --all-containers; }
export -f ocl

function oclf { findpodnames $1; echo "oc logs $p -f"; oc logs $p -f; }
export -f oclf

function ocr { findpodnames $1; echo "oc rsh $p"; oc rsh $p; }
export -f ocr

function ocdp { findpodnames $1; echo "oc describe pod $p"; oc describe pod $p; }
export -f ocdp

#-----------------------

function findprojectnames { p="$(oc projects | awk '/'$1'/ {print $1}')"; t="$(echo $p | gawk 'match($0, /(.*)-.*-.*/, a) {print a[1]}')";}
export -f findprojectnames

function ocproj { findprojectnames $1; echo "oc project $p"; oc project $p; }
export -f ocproj
	
