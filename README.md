---
hide:
  - toc
---

# devtools

This repository is used to share practices, workflows and decisions affecting projects maintained by Ansible DevTools team.

## Python DevTools project dependencies

It should be noted that our vscode extension would either depend on `ansible-dev-tools` python package or directly use our execution environments.

```mermaid
%%{init: {'theme':'neutral', 'themeVariables': { 'edgeLabelBackground': 'transparent'}}}%%
graph LR

  lint(ansible-lint):::pyclass
  compat(ansible-compat):::pyclass
  navigator(ansible-navigator):::pyclass
  adt(ansible-dev-tools):::pyclass;
  ade(ansible-dev-environment):::pyclass
  creator(ansible-creator):::pyclass;
  pytest-ansible(pytest-ansible):::pyclass
  tox-ansible(tox-ansible):::pyclass
  molecule(molecule):::pyclass
  community.molecule(community.molecule):::collectionclass
  builder(ansible-builder):::pyclass
  runner(ansible-runner):::pyclass
  image(community-ansible-dev-tools-image):::containerclass
  sign(ansible-sign):::pyclass

  classDef tsclass fill:#f90,stroke:#f90,color:#333
  classDef containerclass fill:#060,stroke:#060,color:#fff
  classDef collectionclass fill:#5bbdbf,stroke-width:0px
  classDef pyclass fill:#09f5,stroke:#09f0,color:#fff
  style external color:#0FF5,fill:#fff0,stroke:#0FF5
  linkStyle default stroke:grey,text-decoration:none

subgraph external
  builder
  runner
  sign
end

  adt ==> lint
  adt ==> navigator
  adt ==> molecule
  adt ==> ade
  adt ==> creator
  adt ==> sign

  lint ==> compat
  compat ==. test .==> community.molecule
  molecule ==> compat
  molecule ==. test .==> community.molecule:::collectionclass

  navigator ==.==> lint
  navigator ==.==> image
  navigator ==> runner
  navigator ==..==> builder

  adt ==> ade;
  adt ==> creator
  adt ==> pytest-ansible
  adt ==> tox-ansible;

  ade ==> builder;

  click adt "https://github.com/ansible/ansible-dev-tools"
  click ade "https://github.com/ansible/ansible-dev-environment"
  click runner "https://github.com/ansible/ansible-runner"
  click builder "https://github.com/ansible/ansible-builder"
  click community.molecule "https://github.com/ansible-collections/community.molecule"
  click molecule href "https://github.com/ansible/molecule"
  click image href "#container-image"
  click ws "https://github.com/ansible/ansible-workspace-env-reference-image"
  click lint href "https://github.com/ansible/ansible-lint"
  click compat href "https://github.com/ansible/ansible-compat"
  click navigator href "https://github.com/ansible/ansible-navigator"
  click creator href "https://github.com/ansible/ansible-creator"
  click tox-ansible href "https://github.com/ansible/tox-ansible"
  click pytest-ansible href "https://github.com/ansible/pytest-ansible"

  linkStyle 0,1,2,3,4,5,6,7,8,9 color:darkcyan

```

## TypeScript repositories

classDef tsclass fill:#f90,stroke:#f90,color:#333;
classDef containerclass fill:#060,stroke:#060,color:#fff;
classDef thirdpartyclass fill:#9f6,stroke:#9f6,color:#333;

```mermaid

graph TB;

  ansible-backstage-plugins:::tsclass;
  vscode-ansible:::tsclass == external ==> vscode-yaml;
  vscode-yaml:::tsclass;

 click ansible-backstage-plugins "https://github.com/ansible/ansible-backstage-plugins"
 click ansible-dev-environment "https://github.com/ansible/ansible-dev-environment"
 click community.molecule "https://github.com/ansible-collections/community.molecule"
 click vscode-ansible href "https://github.com/ansible/vscode-ansible"
 click vscode-yaml href "https://github.com/redhat-developer/vscode-yaml"
```

## Container Images

`community-ansible-dev-tools-image` **execution environment** is a development
**container image** that contains most of the most important tools used in the
development and testing of collections. Still, while we bundle several
collections in it, you need to be warned that **we might remove any included
collection without notice** if that prevents us from
building the container.

Below you can see the list of collections are currently included in the
container images but this list is subject to change, even on a minor release.
If a collections fails to install or causes installation failures, we will
release the container without it.

- ansible.posix
- ansible.windows
- awx.awx
- containers.podman
- kubernetes.core
- redhatinsights.insights
- theforeman.foreman

Some common command line **tools** are also included in order to help developers:

- git
- podman
- tar
- zsh (default shell)

```mermaid
graph TB;

ee("community-ansible-dev-tools-image<br/><i style="color: #0FF5">fedora-minimal based container</i>")
adt(ansible-dev-tools)
devspaces("ansible-devspaces<br/><i style="color: #0FF5">ubi8 based container</i>")
adt(ansible-dev-tools)

build --> ee;
build --> devspaces;

subgraph build
  collections
  adt
  tools
end

click adt "https://github.com/ansible/ansible-dev-tools"
click ee "https://github.com/ansible/ansible-dev-tools/pkgs/container/community-ansible-dev-tools"
click devspaces "https://github.com/ansible/ansible-dev-tools/pkgs/container/ansible-devspaces"

```
