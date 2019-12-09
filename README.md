freehck.k8s_flannel
=========

Deploy Flannel CNI definition into k8s

Description
-----------

This role contains a bunch of Flannel CNI definitions, and run `kubectl apply -f <definition>` to apply one them. It is suggested, that the user is able to use kubectl (f.e. because it has correct `.kube/config` file).

I collected some Flannel CNI definitions here, and will update when it's needed.

Role Variables
--------------

`k8s_flannel_ver`: version of flannel to deploy

`k8s_flannel_network_pod_cidr`: pod network cidr, default is `10.244.0.0/16`

`k8s_flannel_iface`: network interface flannel will use for nodes inter-communication

Example
-------

    - hosts: k8s-master
      become: true
      vars:
	    # common params
        k8s_ver: "1.16.2-00"
        k8s_node_ip: "{{ ansible_host }}"
        k8s_is_master: false
        # k8s_base is an implicit dependency
        k8s_base_node_ip: "{{ k8s_node_ip }}"
        k8s_base_ver: "{{ k8s_ver }}"
        # k8s_init is an implicit dependency
        k8s_init_cidr: "192.168.0.0/16"
        k8s_init_node_ip: "{{ ansible_host }}"
        k8s_init_node_name: "{{ inventory_hostname }}"
        # k8s_join is an implicit dependency
        k8s_join_is_master: "{{ k8s_is_master | default('false') }}"
        # this role configuration
        #k8s_flannel_ver: "v0.11.0"
        k8s_flannel_ver: "master-2019-12-09"
        k8s_flannel_network_pod_cidr: "192.168.0.0/16"
        k8s_flannel_iface: "enp0s8"  # optional, mainly for vagrant
      roles:
        - role: freehck.k8s_base
        - role: freehck.k8s_init
        - role: freehck.k8s_flannel

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/):

`ansible-galaxy install freehck.k8s_flannel`

License
-------

MIT

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.ru>
