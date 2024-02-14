# Aws Cloudmap Ansible role
![Logo](logo.gif)

[![Build Status](https://app.travis-ci.com/idealista/aws_cloudmap_role.svg)](https://app.travis-ci.com/github/idealista/aws_cloudmap_role)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-idealista.aws_cloudmap_role-B62682.svg)](https://galaxy.ansible.com/idealista/aws_cloudmap_role)

This ansible role uses `aws-cli` to manage instances in [Aws Cloudmap](https://aws.amazon.com/cloud-map/) services. It has been tested for the following Debian versions:
- Bookworm
- Bulleye

This role has been generated using the [cookiecutter](https://github.com/cookiecutter/cookiecutter) tool, you can generate a similar role that fits your needs using the this [cookiecutter template](https://github.com/idealista/cookiecutter-ansible-role).

- [Getting Started](#getting-started)
	- [Prerequisities](#prerequisities)
	- [Installing](#installing)
- [Usage](#usage)
  - [Using Access Keys](#using-access-keys)
- [Testing](#testing)
- [Built With](#built-with)
- [Versioning](#versioning)
- [Authors](#authors)
- [License](#license)
- [Contributing](#contributing)

## Getting Started

These instructions will get you a copy of the role for your Ansible playbook. Once launched, it will install Aws Cloudmap in a Debian system.

### Prerequisities

Ansible 5.2.0 version installed.

Molecule 3.x.x version installed.

For testing purposes, [Molecule](https://molecule.readthedocs.io/) with [Docker](https://www.docker.com/) as driver and [Goss](https://github.com/goss-org/goss) as verifier.

### Installing

Create or add to your roles dependency file (e.g requirements.yml):

```
- src: idealista.aws_cloudmap_role
  version: x.x.x
  name: aws_cloudmap
  scp: git
```

Install the role with ansible-galaxy command:

```
ansible-galaxy install -p roles -r requirements.yml -f
```

Use in a playbook:

```
---

- hosts: someserver
  roles:
    - role: aws_cloudmap_role
```

## Usage

This role does not implement all the features provided by the AWS Cloud Map service and, for now, only manages the creation of HTTP services without health checks, as well as the registration/deregistration of instances with a custom list of attributes. Service removal is not implemented due to the shared nature of the use of Cloud Map, which means that some newly created services could be used by other teammates.

An existing namespace must exist prior to running it.

Look at the [defaults/main.yml](defaults/main.yml) file to see the possible configuration properties.

AWS credentials will be needed for using this role. If you are running it inside an EC2 instance with a correct IAM Role attached, odds are it will work flawlessly. If not, you will have to set some variables as stated in the [Using Access Keys](#using-access-keys) section to first create the `~/.aws/credentials` file.

Overall, the most important variable is `aws_cloudmap_instances`, which can be configured like this:

```
---

aws_cloudmap_instances:
  - instance_name: "my-instance"
    service_name: "node-exporter"
    action: "register"
    attributes:
      AWS_INSTANCE_IPV4: "127.0.0.1"  # Real ip of the host/endpoint specified in the 'instance_name' key
      AWS_INSTANCE_PORT: "9100"
      custom_attribute: "custom_value"
```

Action could be `register` or `deregister`, and it will do exactly what you think: register or deregister the *instance_name* _**"my-instance"**_ in the *service_name* _**"node-exporter"**_.

### Using Access Keys

For using access keys you can set the following variables:

- `aws_cloudmap_profile` (mandatory)
- `aws_cloudmap_access_key_id` (mandatory)
- `aws_cloudmap_secret_access_key` (mandatory)
- `aws_cloudmap_session_token` (optional)

By enabling `aws_cloudmap_set_credentials`, the file `~/.aws/credentials` will be created and populated with the access keys, allowing the role to connect to other regions and/or accounts.

## Testing

### Install dependencies

```sh
$ pipenv sync
```

For more information read the [pipenv docs](ipenv-fork.readthedocs.io/en/latest/).

### Testing

```sh
$ pipenv run molecule test 
```

You can run tests with real AWS credentials and variables by editing the file `molecule/default/group_vars/aws_cloudmap_group/main.yml`. Take a look at it to see some useful examples.

## Built With

![Ansible](https://img.shields.io/badge/ansible-5.2.0-green.svg)
![Molecule](https://img.shields.io/badge/molecule-3.5.2-green.svg)
![Goss](https://img.shields.io/badge/goss-0.3.16-green.svg)

## Versioning

For the versions available, see the [tags on this repository](https://github.com/idealista/aws_cloudmap_role/tags).

Additionaly you can see what change in each version in the [CHANGELOG.md](CHANGELOG.md) file.

## Authors

* **Idealista** - *Work with* - [idealista](https://github.com/idealista)

See also the list of [contributors](https://github.com/idealista/aws_cloudmap_role/contributors) who participated in this project.

## License

![Apache 2.0 License](https://img.shields.io/hexpm/l/plug.svg)

This project is licensed under the [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license - see the [LICENSE](LICENSE) file for details.

## Contributing

Please read [CONTRIBUTING.md](.github/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.
