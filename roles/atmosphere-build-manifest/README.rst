Build the Atmosphere release manifest for the change under test

Builds ``atmosphere-release`` from the Atmosphere checkout, generates the
release manifest the change would publish, and returns both as Zuul artifacts
named ``atmosphere-release`` and ``Release manifest``, with the digest of the
artifact that pushing the manifest produces in
``atmosphere_release_manifest_digest``. Jobs that depend on this one use the
``atmosphere-use-manifest`` role to deploy exactly this manifest.

The manifest is generated once per buildset, so every job that depends on it
deploys the same images, and ``atmosphere-release publish`` can find it after
the change merges.

**Role Variables**

.. zuul:rolevar:: atmosphere_build_manifest_src_dir
   :default: {{ zuul.projects['github.com/vexxhost/atmosphere'].src_dir }}

   The Atmosphere checkout to build the manifest from.

.. zuul:rolevar:: atmosphere_build_manifest_resolve_prefix
   :default: {{ atmosphere_image_prefix | default('') }}

   Pull-through cache prefix used to resolve image tags, to avoid upstream
   rate limits. The manifest still records the original references.

.. zuul:rolevar:: atmosphere_build_manifest_go_version
   :default: 1.24.4

   Go version installed with ``ensure-go``.
