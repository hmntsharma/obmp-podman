# obmp-podman
Rootless Podman + Quadlet + Systemd deployment for OpenBMP

Podman deployment of [OpenBMP](https://github.com/OpenBMP/obmp-docker) as a non-root user, leveraing quadlets and systemd, previously tested [here](https://github.com/hmntsharma/openbmp-crpd) using docker.



* Copy all the quadlets to `~/.config/containers/systemd/`

* Reload systemd daemon (must be done after every update to the quadlets for the change to take effect) `systemctl --user daemon-reload`

* The systemd should be listed now `systemctl --user list-unit-files "dev_o*"`

    ```
    dev_obmp-collector-@.service                                           generated
    dev_obmp-kafka.service                                                 generated
    dev_obmp-podman-network-network.service                                generated
    dev_obmp-psql-app.service                                              generated
    dev_obmp-psql.service                                                  generated
    dev_obmp-whois.service                                                 generated
    dev_obmp-zookeeper.service                                             generated
    dev_oss-grafana.service                                                generated
    dev_oss-haproxy-collector.service                                      generated
    dev_oss-network-network.service                                        generated
    dev_oss-npm.service                                                    generated
    ```

* Start the services
    ```
    systemctl --user start dev_obmp-podman-network-network.service
    systemctl --user start dev_obmp-zookeeper.service
    systemctl --user start dev_obmp-kafka.service
    systemctl --user start dev_obmp-psql.service
    systemctl --user start dev_obmp-psql-app.service
    systemctl --user start dev_obmp-whois.service
    systemctl --user start dev_oss-network-network.service 
    systemctl --user start dev_oss-haproxy-collector.service
    systemctl --user start dev_oss-grafana.service
    systemctl --user start dev_oss-npm.service
    ```

* All the networks and the containers must be operational after this
    ```
    podman ps -a
    ```
    ```
    systemctl --user list-units "dev_o*"
    ```

* The service status can be checked as below, e.g.
    ```
    systemctl --user status dev_oss-haproxy-collector.service
    ```

* The service logs can be checked as below, e.g.
    ```
    journalctl --user -u dev_oss-haproxy-collector.service
    ```

    
**Reference Documents:**
* [OpenBMP](https://www.openbmp.org/)
* [Make systemd better for Podman with Quadlet](https://www.redhat.com/en/blog/quadlet-podman)
* [podman-systemd.unit - systemd units using Podman Quadlet](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html)
* [Nginx Proxy Manager](https://nginxproxymanager.com/)

