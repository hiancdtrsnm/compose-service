Compose-Service
=========

The role is used to deploy a service that is defined in a docker-compose file. The role will pull the docker image, create the service and start the service. The role will also create [Caddy](https://caddyserver.com/) configuration for the service and reload the Caddy service if the Caddy service is running. A docker network is created to add the possible service and made visible to the other services. The network is called `services` by default but is set using the variable `network_name`. The role will also create a `.env` file with the environment variables that are passed to the `docker-compose` file. The role will also delete the `.env` file if the `delete_env` variable is set to `true`.

Requirements
------------

This role requires the docker and docker-compose to be installed on the target machine. The `Caddy` is an optional requirement. The role will always create the `Caddy` configuration file.
But if your running `Caddy` it should import the configuration file. But if `Caddy` is running, the role will automatically reload the `Caddy` service.

Role Variables
--------------

The role has the following variables:

* `app_port`: The port that the service is running on. This is used to create the `Caddy` configuration file. If the `app_port` is not set, the role will not create the `Caddy` configuration file. The `docker-compose` file should expose the port that the service is running on or accept a APP_PORT environment variable to set it.
* `remote_dir`: The directory where the service would be located in the remote machine. The default value is `~/services`.
* `url`: The url that the service is running on. This is used to create the `Caddy` configuration file. If the `url` is not set, the role will not create the `Caddy` configuration file. The `docker-compose` file should expose the port that the service is running on or accept a URL environment variable to set it.
* `service_path`: The path to the service in the local machine. This variable is required.
* `compose_variables`: The variables that would be passed to the `docker-compose` as environment variables. The default value is `{}`.
* `sudo_docker`: A boolean value that determines if the role should run the docker commands with `sudo`. The default value is `false`.
* `delete_env`: A boolean value that determines if the role should delete the environment file after the service has been created. The default value is `false`.
* `expose_port`: A boolean value that determines if `docker-compose` should expose the port that the service is running on. When the value is `false`, the port is passed only with the localhost access ("localhost:APP_PORT"). Allowing only the localhost to access the service. The default value is `false`.
* `network_name`: The name of the network that the service would be connected to. The default value is `services`.

Dependencies
------------

For now, no dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: pi
      name: "Homepage"
      roles:
        - hiancd.compose-service
      vars:
        url: "home.pi.home.local"
        service_path: "./services/homepage"
        app_port: 3030 # This should be include to set Caddy
        sudo_docker: true

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
