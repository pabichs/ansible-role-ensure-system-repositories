ensure_system_repositories
=========

This role acts as a proxy to the `ansible.builtin.apt_repository` and `ansible.builtin.yum_repository`.
Passes all arguments to the underlying package manager module.

Requirements
------------

None.

Role Variables
--------------

| Variable                    | Required | Default     | Description                                                                                          |
|-----------------------------|----------|-------------|------------------------------------------------------------------------------------------------------|
| `create_keyrings_directory` | no       | `true`      | Controls whether to clreate `/etc/apt/keyrings` directory or not. Works only on `apt`-using systems. | 
| `system_repositories`       | no       | _undefined_ | List of system repositories to create. Passes all arguments to `ansible.builtin.apt_repository` or `ansible.builtin.yum_repository` depending on system's package manager. |                                                              


Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: pabichs.ensure_system_repositories
      vars:
        system_repositories:
          - key_url: "https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key"
            key_dest: "/etc/apt/keyrings/kubernetes-archive-keyring.asc"
            repo: "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.asc] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /"
          - key_url: "https://download.docker.com/linux/ubuntu/gpg"
            key_dest: "/etc/apt/keyrings/docker-archive-keyring.asc"
            repo: "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker-archive-keyring.asc] https://download.docker.com/linux/ubuntu focal stable"
            state: absent
```

```yaml
- hosts: servers
  roles:
    - role: pabichs.ensure_system_repositories
      vars:
        system_repositories:
          - name: kubernetes
            description: Kubernetes
            baseurl: https://pkgs.k8s.io/core:/stable:/v1.28/rpm/
            enabled: true
            gpgcheck: true
            gpgkey: https://pkgs.k8s.io/core:/stable:/v1.28/rpm/repodata/repomd.xml.key
            exclude: kubelet kubeadm kubectl
```

License
-------

BSD, MIT

Author Information
------------------

Created in 2023
