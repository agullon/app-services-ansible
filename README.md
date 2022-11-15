# Red Hat Application Services Ansible Collection

Ansible Collection for Red Hat Application Services

## Prerequisites

- Python version 3.9 or higher. Installation can be [found here](https://www.python.org/downloads/).
- Ansible must be installed on the machine using a version of 2.9 or greater. Installation guides for Ansible can be [found here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible).
- This collection requires some additional dependencies to be installed:

```shell
pip install rhoas-sdks --force-reinstall
pip install python-dotenv
```

- Not a prerequisite but it is highly recommended to use a Python virtual environment. This collection will work best if it used within one. To create and activate a Python virtual environment, run the following command:

```shell
python3 -m venv rhoas
. rhoas/bin/activate
```

## Installing collection

Once the prerequisites are fulfilled, installation of the RHOAS collection is as follows:

```shell
ansible-galaxy collection install rhoas.rhoas
```

Once the prerequisites and the RHOAS collection are installed the collection can be used as follows.

## Using collection

Once the collection is installed, documentation for each module can be found by running the following command:

```shell
ansible-doc rhoas.rhoas.<module_name>
```

For example:

```shell
ansible-doc rhoas.rhoas.create_kafka
```

This will output the documentation for the `create_kafka` module. It shows the required and optional parameters for the module, along with examples of how to use the module and the output of the module.

An example Ansible playbook which demonstrates the RHOAS modules abilities can be [found here](https://github.com/redhat-developer/app-services-ansible/blob/2a47a44a92d871f99bba2ced38ff770f00e3c3da/run_rhosak_test.yml).

The playbook and all modules require an 'OFFLINE_TOKEN' to be used with for authentication with the Red Hat OpenShift Application Services API. The token can be passed in like this:

```yaml
...
  tasks:
  - name: Create kafka
    rhoas.rhoas.create_kafka:
      name: "unique-kafka-name"
      openshift_offline_token: "OFFLINE_TOKEN"
...
```

If the token is not passed in, the module will attempt to read it from the environment variable `OFFLINE_TOKEN`. This token is used to authenticate the user. The token is an OpenShift Token and can be found [here](https://console.redhat.com/openshift/token)

Two further environment variables are used for the collection to work. These environment variables can be placed in a `.env` file.
They are:

- `API_BASE_HOST` - The base host for the API. This is the base URL for the API. For example, `https://api.openshift.com`
- `SSO_BASE_HOST` - The base host for the SSO. This is the base URL for the SSO. For example, `https://sso.redhat.com/auth/realms/redhat-external`

If neither of these environment variables are set, the collection will default to the URLs as noted in the examples.

The collection can be used in a playbook like this:

```shell
ansible-playbook run_rhosak_test.yml
```

This playbook, if run as is, will do the following:

- Provision a Red Hat OpenShift Streams for Apache Kafka instance.
- Create a Service Account.
- Create an Access Control List (ACL) for the Service Account with some default values and bind that ACL to the Kafka instance and Service Account.
- Create two Kafka topics.
  - One with some examples of different configurations available.
  - One with default values as provided by the API.
- Update topic one's configuration.
- Delete both created topics.
- Delete the service account.
- Deprovision the Red Hat OpenShift Streams for Apache Kafka instance.

The playbook will then proceed to delete the created resources and as such, some alteration of the playbook will be required to keep the resources created.
