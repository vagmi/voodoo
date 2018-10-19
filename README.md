# Voodoo - An agentless deployment tool

Voodoo is an agentless deployment tool where you write your deployment scripts in Javascript. The tool itself is written in Go.

Here is a script that deploys Docker.

```js
var package = require("package")
var ubuntu = require("distros/ubuntu")

func registerHosts() {
  // it uses ssh-agent to for key negotiation
  registerHost("node", {
    host: "dockerhost"
  });
}

func installDocker() {
  // This ensures that packages like software-properties-common, 
  // tmux, vim, ca-certificates, curl, apt-transport-https, etc
  ubuntu.setupBasePackages()
  
  var versionName = ubuntu.getVersionCode()
  ubuntu.addAptKeyFromURL("https://download.docker.com/linux/ubuntu/gpg")
  ubuntu.addRepository("deb [arch=amd64] https://download.docker.com/linux/ubuntu " + versionName + " stable")
  ubuntu.ensureInstalled("docker-ce")
}

registerHosts()

run(installDocker, {
  onHosts: ["node"]
});

```

