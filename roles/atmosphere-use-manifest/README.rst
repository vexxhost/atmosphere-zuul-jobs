Deploy from the release manifest built for this buildset

Downloads the ``Release manifest`` and ``atmosphere-release`` artifacts
returned by a job using the ``atmosphere-build-manifest`` role, starts a
private registry on the node with TLS (a throwaway CA) and password
authentication, pushes the manifest to it and checks that the pushed artifact
has the digest the build job reported. Atmosphere then reads the manifest from
the registry by reference, the same way deployments read published releases.

The job must depend on the job that built the manifest, and pass these job
variables to the Atmosphere deployment:

.. code-block:: yaml

   atmosphere_release_manifest: atmosphere-ci-registry:5443/vexxhost/atmosphere-release:ci
   atmosphere_release_manifest_ca_file: /usr/local/share/ca-certificates/atmosphere-ci-registry.crt
   atmosphere_image_mirrors:
     - source: atmosphere-ci-registry:5443
       mirror: atmosphere-ci-registry:5443

The mirror rule keeps ``atmosphere_image_prefix`` from being applied to the
manifest reference.

**Role Variables**

.. zuul:rolevar:: atmosphere_use_manifest_reference
   :default: atmosphere-ci-registry:5443/vexxhost/atmosphere-release:ci

   Where the manifest is pushed. The registry host name is added to
   ``/etc/hosts``.

.. zuul:rolevar:: atmosphere_use_manifest_ca_file
   :default: /usr/local/share/ca-certificates/atmosphere-ci-registry.crt

   Where the registry CA certificate is installed.
