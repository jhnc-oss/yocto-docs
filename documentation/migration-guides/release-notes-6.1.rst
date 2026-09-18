.. SPDX-License-Identifier: CC-BY-SA-2.0-UK

.. |yocto-codename| replace:: blacksail
.. |yocto-ver| replace:: 6.1
.. Note: anchors id below cannot contain substitutions so replace them with the
   value of |yocto-ver| above.

Release notes for |yocto-ver| (|yocto-codename|)
================================================

This document lists new features and enhancements for the Yocto Project
|yocto-ver| Release (codename "|yocto-codename|"). For a list of breaking
changes and migration guides, see the :doc:`/migration-guides/migration-6.1`
section.

New Features / Enhancements in |yocto-ver|
------------------------------------------

-  Linux kernel 7.2, gcc 16.2, glibc 2.44, LLVM 23.1.1, and over 300 other
   recipe upgrades

..
   Found in meta/lib/oe/sanity.py:check_sanity_everybuild

-  Minimum Python version required on the host: 3.9.

-  Kernel-related changes:

   -  :ref:`ref-classes-kernel-module-split`: Return list of values in
      ``extract_modinfo``
      (:oecore_rev:`43da9b93bdf40bc4f2fb81687e80b94571232d5b`)

   -  :ref:`ref-classes-kernel`: Disable module tarball deployment
      (:term:`MODULE_TARBALL_DEPLOY`) by default
      (:oecore_rev:`f51d48c9eb0bcb630525566091c66ec1af4d770e`)

   -  ``linux-yocto``: Remove CVE exclusion list as
      :ref:`ref-classes-sbom-cve-check` does this itself
      (:oecore_rev:`596ad6c397c5ca264c6c257283ea78095562e67c`)

   -  :ref:`ref-classes-kernel-fit-image`:

      -  Validate key files expected by
         ``mkimage`` for the selected algorithm
         (:oecore_rev:`7570ab598d10a08fa046d3e8f5897424193240e1`)

      -  Do not include ``kernel`` property in DTBO configuration sub-nodes
         (:oecore_rev:`06ed34005957a6afb88270603df5e545941546b0`)

      -  Fix operation with :term:`KERNEL_DTBVENDORED` set to "1"
         (:oecore_rev:`4297b94c3728cd2e320d75b68c508e12ab127719`)

      -  Add :term:`KERNEL_DTBVENDORED` support for :term:`FIT_CONF_DEFAULT_DTB`
         (:oecore_rev:`3bceb2dabeee13c0a80ddd74ea7ae991606d6772`)

      -  Make kernel entry point and load address optional
         (:oecore_rev:`54c4b4826a3a67fbfef23c93356bfd7e5cb097c1`)

      -  Avoid shadowing os stdlib import
         (:oecore_rev:`fe1b5a0e7a39619b648cc420ef6ea4033f8cc50d`)

      -  Introduce :term:`FIT_OS` variable to override 'os' field
         (:oecore_rev:`dc00e830c3f8e6876630a8c81e7d11c18e531a01`)

      -  Reference the boot script as "script"
         (:oecore_rev:`6bb0775e6ca56eb50119e9ad618dc5b865252f72`)

      -  Skip sign key check for PKCS #11 URI
         (:oecore_rev:`b606b7153001b76de3467e1fd8a66ae6396de5c1`)

      -  Don't add configuration hash node when signing is enabled
         (:oecore_rev:`7346ffe190482ee4932728ba8d8741be8bed1d5b`)

   -  :ref:`ref-classes-kernel-arch`: Add `Clang` toolchain support
      (:oecore_rev:`928d81916f6e3e30e486d63d92a02fd6dcb8a59b`)

   -  :ref:`ref-classes-kernel-arch`: Fall back to GNU ``ld`` unless
      ``ld-is-lld`` is in :term:`DISTRO_FEATURES`
      (:oecore_rev:`f92bfb5309d87aaf5f9683cd3e80e2a1884db6dc`)

   -  :ref:`ref-classes-kernel-yocto`: Set ``CLANG_FLAGS`` for kernel config
      checks when using Clang
      (:oecore_rev:`e88ee794a3f75d7754fe0a3b3c14877c60e7401f`)

   -  ``linux-yocto-dev``: Enable BSP config audit reporting (through
      :term:`KCONF_BSP_AUDIT_LEVEL`)
      (:oecore_rev:`c98e8391b3227d0bfefa9a2e3dee1143f1c54589`)

   -  ``kernel-devsrc``: Install sources required for BPF host tools
      (:oecore_rev:`cf5b6271a9f88f70b2eb9a4a77613ba70c408859`)

   -  :ref:`ref-classes-kernel-yocto-rust`: Add `Clang` toolchain check for
      ``riscv64``
      (:oecore_rev:`2b228490b7ef063cc54165fc2aa582a9abf9aa85`)

   -  ``lib/oe/kernel_module.py``: Re-sign kernel modules after package stripping process
      (:oecore_rev:`a608d107cca13587cd45be45ddfbc4bfdc228f2c`)

   -  :ref:`ref-classes-kernel-module-split`: Don't re-sign compressed kernel modules
      (:oecore_rev:`3483d029346c56050b40596cf4ad51de45fc63f8`)

-  New core recipes:

   -  ``python3-vcs-versioning``: Added as a new dependency of the
      ``python3-setuptools-scm`` recipe
      (:oecore_rev:`507bb5234cc13377ba27b8708f798a0815c3485f`)

   -  ``cgo-helloworld``: A Go example recipe with `cgo
      <https://go.dev/wiki/cgo>`__ enabled
      (:oecore_rev:`204e4f7398ca0e8847a026c5b4f46685760af90a`)

   -  ``jansson``: Add a recipe from ``meta-oe``, as it is a new hard dependency
      of the ``igt-gpu-tools`` recipe
      (:oecore_rev:`2b6e7d092b4a21930186682ebf085848b2e49de5`)

   -  ``packagegroup-core-buildessential-rust``: A
      :ref:`ref-classes-packagegroup` to provide the on-target dependencies
      required for building `Rust`-based kernel modules, including the `Rust`
      toolchain, ``bindgen``, and ``libclang``
      (:oecore_rev:`500b45f8382a0f772c8ade37a46307fb671796b2`)

   -  ``nativesdk-packagegroup-sdk-host-rust``: Similar to
      ``packagegroup-core-buildessential-rust`` but for building `Rust`-based
      kernel modules in the SDK
      (:oecore_rev:`a05c097edb552c5a7bc0fd812be353f079f8d201`)

   -  ``nativesdk-packagegroup-sdk-host-clang``: :ref:`ref-classes-nativesdk`
      :ref:`ref-classes-packagegroup` providing the `Clang`, ``lld``, and
      ``llvm-bin`` dependencies needed to build external/out-of-tree modules in
      the SDK using the LLVM/Clang toolchain
      (:oecore_rev:`2b1db6ec316d6f92d1631b82e91b4c334962213e`)

   -  ``gstreamer1.0-plugins-rs``: GStreamer Rust plugins
      (:oecore_rev:`3a2b032f0fccc9dd636db82a63f67d644ad8bf51`)

   -  ``rust-source``: Recipe that fetches, unpacks, and patches the ``rustc``
      source tree into a "work-shared" location
      (:oecore_rev:`395fe161362c5b6d39a9b01e938d99567c6273c3`)

   -  ``libzip``: Added because it is a dependency of various tools
      (:oecore_rev:`f17e8750c39030fbd4bc41e22d4f5e80c4ccaf87`)

   -  ``mesa-libclc``: New recipe for forking ``libclc`` for ``Rusticl`` purposes in Mesa
      (:oecore_rev:`956aaf355dc875cdf4579884d187e9eb3a236f85`)

   -  ``libtracefs``: Added as ``powertop`` version 2.16 requires it
      (:oecore_rev:`3c3f22739573f91392330e202c0fa7e96c8dc7ab`)

   -  ``libfaketime``: Used for reproducibility testing
      (:oecore_rev:`f3a4faaf4c3cf78f74baa7e5a147749dfa6bbe47`)

   -  ``python3-tomlkit``: New dependency of ``hatchling``
      (:oecore_rev:`46d3b26a63703f4b5d055c0d9da3c7550161ec32`)

-  New core classes:

   -  :ref:`ref-classes-python_uv_build`: Used to build Python recipes with
      ``uv_build``, a slimmed down version of ``uv``
      (:oecore_rev:`c535a9f5dbdc5d8ddb4a2438604456b8084a73aa`)

   -  :ref:`ref-classes-upstream-stable-release-point`: Helper class for setting
      the :term:`UPSTREAM_STABLE_RELEASE_REGEX` variable with
      :term:`STABLE_VERSION_PARTS`
      (:oecore_rev:`a1e069d04cb13e990b362804bd56a4935338ef96`)

   -  :ref:`ref-classes-python_pbr`: Used to build Python recipes with the
      `Python Build Reasonableness <https://pypi.org/project/pbr/>`__ backend
      (:oecore_rev:`b9f3b369b8538a1cf5d4980c9010888d697eb2f1`)

-  New variables:

   -  :term:`UBOOT_FIT_CONF_DESC` allows configuring the description property
      of the configuration node of a U-Boot FIT image
      (:oecore_rev:`df16e4a3900f53245cc8d9a92e277aead56a7369`)

   -  :term:`KERNEL_TOOLCHAIN`: Allows specifying the toolchain to use to build
      the Linux kernel
      (:oecore_rev:`0e3756efe0923e621d88f61121cfa56b23454211`)

   -  :term:`FIT_OS`: Controls the ``os`` field of the FIT image built with the
      :ref:`ref-classes-kernel-fit-image` class
      (:oecore_rev:`dc00e830c3f8e6876630a8c81e7d11c18e531a01`)

   -  :term:`CARGO_PROFILE`: Allows selecting the :ref:`ref-classes-cargo`
      profile to use (``--profile``)
      (:oecore_rev:`d4de7d0256aca47cdb1df48640c37221deaa09d4`)

-  :term:`OpenEmbedded-Core (OE-Core)` library changes:

   -  ``oe/lsb``: Only read ``/etc/os-release``
      (:oecore_rev:`1331c5f2c49e448bc32ec364b13fecfdcf5e05f4`)

   -  ``oe/utils``: Drop ``all_distro_features()``
      (:oecore_rev:`28d32a940ff46ca80db75e8ed24a3e26eec95599`)

-  Global configuration changes:

   -  ``bitbake.conf``:

      -  Define a new ``firmwaredir`` variable (which by default points to
         ``/lib/firmware``). This variable was adopted in several recipes to
         replace ``${nonarch_base_libdir}/firmware``
         (:oecore_rev:`2b75c7ba5e3aa6fc57d7b4afe59e2277b4d87de1`)

      -  Drop ``opencl`` from :term:`DISTRO_FEATURES_FILTER_NATIVE` and
         :term:`DISTRO_FEATURES_FILTER_NATIVESDK`
         (:oecore_rev:`f810998ac90f7a3e20e26555f39d754b16ce8b69`)

   -  :ref:`ref-classes-useradd`: Add support for the :term:`USERMOD_PARAM`
      variable, acting as a replacement of the ``GROUPMEMS_PARAM`` variable
      (:oecore_rev:`b8da733ab12c64503a353d5ceb2eb63fed95d851`,
      :oecore_rev:`cec67e24ac94554e092f8ab18b42e09b4feba77e`)

      Show a deprecation warning if ``GROUPMEMS_PARAM`` is used
      (:oecore_rev:`06f48de92f4b8d7116cd1ce8ba5bc0bd7f8eda9e`)

   -  :ref:`ref-classes-useradd`: Switch from ``--root`` to ``--prefix`` option
      (:oecore_rev:`a7b846ba7d6d63a5e59939d75d9c5fe3e4cbb0e9`)

-  BitBake changes:

   -  The ``git-make-shallow`` utility script was dropped along with its test
      cases. It was replaced by Git native shallow fetches
      :bitbake_rev:`0223ec6f6319b58b740dbbb463dcd190bc85339a`

   -  Replace ``codegen`` with ``ast.unparse()``
      (:bitbake_rev:`76dd6d69c59c1686be2dfb3fc5b72c2a9df34871`)

   -  After the removal of the previous obsolete ``bb.fetch`` module, the
      ``bb.fetch2`` module was renamed to ``bb.fetch``. A warning will be
      printed if ``bb.fetch2`` is used in layers
      (:bitbake_rev:`34ea7cb48090f1ee962d40aca3ebbaf1679b6b23`)

   -  ``fetch/git``:

      -  Fix leaking of temporary directory
         (:bitbake_rev:`c7ebe03c7ebe266795d20c5b722129a0fad86668`)

      -  Fix trailing slash in clone command causing double slash in alternates
         (:bitbake_rev:`797b0a348d7426d03459e577feacd3488fdeee47`)

      -  Accept SHA256 revisions
         (:bitbake_rev:`707ba7e3f218e9d9fff2649ca4be11e2cf3b45ac`)

   -  Unpack RPMs with ``--no-absolute-filenames``
      (:bitbake_rev:`1b1a71586aa93678c1d9ca40ef2c6fa518f89356`)

   -  ``tinfoil``:

      -  Only allow one process progress bar at once
         (:bitbake_rev:`d6bc0e5ec549a4f984cb3d470dd3c04d0ea46fde`)

      -  Add a prepared task runner, to execute a single recipe task and nothing
         else
         (:bitbake_rev:`280a3b379bc11567ac1397e25f164297d65c70eb`)

   -  ``knotty``: Show elapsed time on the task progress bar
      (:bitbake_rev:`b8dfa3567db01d129fc5bda4285fcb36883b0f12`)

   -  ``bitbake-setup``:

      -  Add a "notes" item to ``bitbake-setup`` configuration
         files which create a "conf-notes.txt" file in the :term:`Build Directory`
         (:bitbake_rev:`020a5ba24c7df53eacf834deb87216053ccd38db`)

      -  Add a ``--version`` option
         (:bitbake_rev:`0e39acaaebb726a1e5524fdd28749f747b6ccf63`)

      -  Add PyPI packaging for ``bitbake-setup``, allowing to install the tool
         with ``pip``
         (:bitbake_rev:`6b51b93a77adb363e9da19244064486f968a547e`)

      -  Use the :term:`BitBake` built-in fetcher for buildtools installer download
         (:bitbake_rev:`e8d531cec67644ee4340c6b99365454349ebc246`)

      -  ``default-registry``: Use the
         :ref:`ref-fragments-core-yocto-monitor-disk-space` fragment by default
         (:bitbake_rev:`975b322cfa6349e751e21717c3e2e8526494108c`)

      -  Print a note about fetcher log if something fails there
         (:bitbake_rev:`3860fabb2d0dcdb280ad268c08d6f6f27a417141`)

   -  ``lib``: Drop ``pyinotify``, as it wasn't used by :term:`BitBake` anymore
      (:bitbake_rev:`d20e7dcbd8b6b47273222b5281b956aab58b3351`)

   -  ``lib``: Rename ``fetch2`` as ``fetch`` and invert compatibility shim to
      make ``bb.fetch2`` equal to ``bb.fetch``
      (:bitbake_rev:`34ea7cb48090f1ee962d40aca3ebbaf1679b6b23`)

   -  ``fetch``: Switch to shared locking for read accesses
      (:bitbake_rev:`e8f8ab657e521f6409f0ad783b420e6ca559f317`)

   -  ``fetch/crate``: Support user-defined protocols
      (:bitbake_rev:`22a993aed05baecb06bcf9b6875eb43bf543a1a0`)

   -  ``fetch``: Export ``AWS_SHARED_CREDENTIALS_FILE`` for S3 fetches,
      allowing to use a credential file different than ``~/.aws/credentials``
      (:bitbake_rev:`0d6aa2b58aa2f253c91c3874f42e2051f8b56d56`)

   -  ``fetch/{npm,npmsw}``: Re-enable fetchers now that checksums come from
      :term:`SRC_URI`, and restore the fetcher's test coverage
      (:bitbake_rev:`9e45e00a2009f28829417c9517f5ab623f66dd5d`,
      :bitbake_rev:`f7608f3195af9abb405af9be95286e2267221331`)

   -  ``fetch/wget``:

      -  Handle long filenames correctly
         (:bitbake_rev:`da1fcdf15c647a2a718ae921e261a76e451b442a`)

      -  Reuse cached HTTPS connections
         (:bitbake_rev:`2c236d8bdc51742cbc5a97e30fb126d82d4a908a`)

      -  Cache SSL context instead of recreating every time
         (:bitbake_rev:`625b25ad1c115d0a732330c71a79d323e09b8848`)

   -  ``fetch/gitsm``: Store the original URL data to fix relative ``gitsm://`` paths
      (:bitbake_rev:`58872250f72f5760b4126552bdec5497f5a1a1f4`)

   -  Add ``pyproject.toml`` and ``vendor.txt`` for vendoring
      (:bitbake_rev:`70b54fdace8597043ff70e23737273a17d303a70`)

   -  ``data``: Return a list from exported_vars()
      (:bitbake_rev:`c870f5bd96ad02efc02c58133c27bdae3a300e3e`)

   -  ``asyncrpc``: Add Task Group
      (:bitbake_rev:`4fc36e6b93bb85040526954371ed53a6f8b71ea7`)

   -  ``hashserv``:

      -  Client: Add asynchronous streaming API
         (:bitbake_rev:`544d9f1badcc51843f1448be7c73974b5aea0c3f`)

      -  Server: Add queued streaming API
         (:bitbake_rev:`9e0f0649de0f956c942f5fe42df13ba094e8c92b`)

-  SPDX-related changes:

   -  Add SHA 512 support
      (:oecore_rev:`383a5a63963a063b3cec0f38dcba1c394911c3a5`)

   -  Add custom annotations to recipe packages
      (:oecore_rev:`e5a4a7d7c1916d88456838fbb31ee87d6a1e48ab`)

   -  ``common-licenses``: Add SPDX License Exceptions
      (:oecore_rev:`9c8d126ac9bb6f88a77ed34278f4c3502646260a`)

   -  ``lib/oe``: Add SPDX license library
      (:oecore_rev:`51c793022081ce3c404cb0f8b3fa18551c88f187`)

   -  ``lib/oe/license``: Rework package licenses matching
      (:oecore_rev:`e3c5cb744195f519e9e3b52a68326dcff7749620`)

   -  ``lib/oe/spdx30_tasks``: Fix image deploy dir
      (:oecore_rev:`fe3a72172d7ad67cd1f54e6a1e8337567ccdc042`)

-  :ref:`ref-classes-sbom-cve-check`-related changes:

   -  ``sbom-cve-check`` recipe: Enable offline mode
      (:oecore_rev:`db2e0fdf82af5b50053840e522bf72550d1cbe66`)

-  Packaging changes:

   -  ``package_manager/ipk``: Skip checksums for unsigned local feeds
      (:oecore_rev:`3f50b047b17392a11a4a2587d44c1db8fb652506`)

   -  :ref:`ref-classes-package_rpm`: Drop external dependency generator to support RPM 6
      (:oecore_rev:`093ae9987bd2de38a3a9627fcbc09745f0a8b67f`)

   -  ``libarchive``: Add RPM format reader to support RPM 6
      (:oecore_rev:`8401bac0ca8ab969e9a2861b116896e1f9c8b9f6`)

   -  No longer modify paths in ``save_debugsources_info()``
      (:oecore_rev:`b567c2f0d91fbf36886d0c3a6f6ea90c07771044`)

-  QEMU / ``runqemu`` changes:

   -  ``runqemu-export-rootfs``: Set :term:`PSEUDO_INCLUDE_PATHS` for ``unfsd``
      (:oecore_rev:`76b8b8320436b086059b20813de6523161a5a666`)

   -  ``runqemu-extract-sdk``: Refactor in Python
      (:oecore_rev:`78c176df185b49f7d7720145e85089c3556c9833`)

   -  ``qemu``: Set the compilation progress bar flag
      (:oecore_rev:`21994d6de3f753728ec2bc1ed11d2e819442a3f7`)

   -  ``qemu``: Add ``ppc64le`` support to :term:`COMPATIBLE_HOST`
      (:oecore_rev:`77a329d18a068dcc7d3b24c1e90826dfde45d2d1`)

   -  ``python3-qemu-qmp``: Added as a new dependency of QEMU
      (:oecore_rev:`5babf4e57c20eb54674c323b3527f9b922f17e1e`)

-  Documentation changes:

   -  New document on :doc:`shared state signing </security-manual/sstate-signing>`

   -  New document on :doc:`setting up a shared state mirror </dev-manual/sstate-mirrors-setup>`

   -  New document on how security is handled in builds:
      :doc:`/security-manual/build-process-security`

-  Go changes:

   -  :ref:`ref-classes-go-vendor`: Remove vendor symlink
      (:oecore_rev:`d3cbc285a257f132a17ec042ddb11eef136c6d2b`)

-  Testing-related changes:

   - :ref:`ref-classes-ptest` support was added for the following recipes:

      -  ``go`` (:oecore_rev:`9ca41405e6bca276468a3b6f67eaa328b8016485`)
      -  ``libatomic-ops`` (:oecore_rev:`d899b7f50aad0c09d2db16dc83627fc1898bfab2`)
      -  ``libcap`` (:oecore_rev:`bfc0a651f5bdf57080f811fd0c63b80f179f7563`)
      -  ``libxslt`` (:oecore_rev:`fa4328b70af3e6bad42970322a5874c192761c6d`)
      -  ``python3-certifi`` (:oecore_rev:`0aa251fa23646440182f5507215a91381ddeef5c`)
      -  ``python3-pyopenssl`` (:oecore_rev:`3fdfff9b7e873904674eb64d345334e017bea10d`)
      -  ``python3-vcs-versioning`` (:oecore_rev:`90c8bc12b2af0df3ea73d3e036e114bc683bdb6c`)
      -  ``wget`` (:oecore_rev:`bb24e394d151293be52e110d898e398a95c32ea0`)
      -  ``zstd`` (:oecore_rev:`f3e3ccba7f8090cc6e8752faed6b973409ae0832`)

   -  ``meta-selftest``: Add a ``usegroup-deponly`` recipe to test
      :term:`USERADD_DEPENDS` only
      (:oecore_rev:`36eac58184a22ba972f8613bd9f564e0f6bd1680`)

   -  ``python3``: Use a ``SKIPPED_TESTS`` variable instead of test skip patches
      (:oecore_rev:`a8b2baa6020f96468a98200619ec37c460694c4c`)

   -  ``oeqa/oelib``: Add ``runcmd`` tests
      (:oecore_rev:`b364c44de83bcf73c621972ad5e09e46ccb9d412`)

   -  ``oeqa/selftest/pkgdata``: Add a test for variable consistency in
      :ref:`ref-classes-multilib*`
      (:oecore_rev:`b937278645b4e170c821910c559bfb7be7be0f44`)

   -  ``oeqa/selftest/kernelmodulesplit``: Test cross-recipe kernel module dependencies
      (:oecore_rev:`6ca1f213e2718b95c52a1e643b80d17bb9317da5`)

   -  ``oe-selftest``: :ref:`ref-classes-kernel-fit-image`: Add tests for
      :term:`KERNEL_DTBVENDORED`
      (:oecore_rev:`de2e11e63624f86583875163e66f63ea7457c121`)

   -  :ref:`ref-classes-testimage`: Handle bootlog variants on failed QEMU tests
      (:oecore_rev:`7a6596925cb0eb8dd48a0362a51875cdd60152bc`)

   -  ``oeqa/selftest``: Add :ref:`ref-classes-go-vendor` selftest suite
      (:oecore_rev:`79d251a1893865ad126aabf3599173729a6321a7`)

   -  ``oeqa/selftest/clang``: Add selftests for Clang/LLVM/LLD test suites
      (:oecore_rev:`8e7a293bcf050ff79f5a42d9048ab58960dc335c`)

   -  ``oe-selftest``: ``devtool``: Add test for ``add``/``finish`` workflow
      (:oecore_rev:`36472a7b495eabe5a038644e943931cf6913cf15`)

-  ``devtool``-related changes:

   -  Disable GPG signing when setting up source tree repos
      (:oecore_rev:`b5c84b07b87eafb4f68f7662b6cf26d8b73e3247`)

   -  ``upgrade`` command: Extract changelog between versions
      (:oecore_rev:`ddd54d6d9760e6c049ef82f250c81a10592d409d`)

   -  Fix ``finish``/``update-recipe`` for recipes using :term:`AUTOREV`
      (:oecore_rev:`ae7be93cfd63088f6f160e75a99d9eaca1e1bd7f`)

   -  Fix ``update-recipe``/``finish`` ``--initial-rev`` override
      (:oecore_rev:`62abb60b899e63ea6db5c745f4b1b6af0d4b40f1`)

   -  Guess ``srcrev`` update mode for ``gitsm://`` recipes too
      (:oecore_rev:`9307c37634c56f7df19ca5a93689e2fe6213f10e`)

   -  ``upgrade``: Ignore changelogs from 3rd party
      (:oecore_rev:`1c9d4bb853e457ce533edecd2ddddf1f6ff8a8f1`)

   -  ``deploy-target``: Fix running ``strip`` under ``pseudo``
      (:oecore_rev:`a48a731a2beb7833177400dc9422b1621d064e35`)

   -  ``ide-sdk``:

      -  Add `LLDB` support for `Clang` toolchain
         (:oecore_rev:`e81d8971f756963850be7dee7b05e5579bfe7bbf`)

      -  Default to all modified recipes
         (:oecore_rev:`4c1c0ff4512fd7882f8e0916bc19441e91e5a6c3`)

      -  Auto-write image debug settings to ``bbappend``
         (:oecore_rev:`929a8c198df54f022f1ba65d8931f9d285a0e763`)

      -  Support ``runqemu slirp``
         (:oecore_rev:`5cc15eccbcde40ecdb3ff04ea7c08e0e3dda2d9d`)

      -  Support NFS rootfs
         (:oecore_rev:`4ab284aadca01f4fcb262e99f3d3e48b86121689`)

      -  Add clangd support for VSCode IntelliSense
         (:oecore_rev:`10a0a09003cbfed7fb180fc21d56f021e9dd33f9`)

      -  Support clangd for non-clang toolchain recipes
         (:oecore_rev:`da9ac674451616dcb7c6f8d59e15a459c9fb37d6`)

      -  Format C/C++ with clangd when .clang-format is present
         (:oecore_rev:`5c13e480ad926ebd67f5f0c44e676a3ac2c482d7`)

      -  Support LLDB ATTACH mode
         (:oecore_rev:`6a4227b03a2c515cd97b33f4bf2b9ae65bc5d82c`)

      -  Allow --ide to select multiple plugins
         (:oecore_rev:`8aa5c039973d614f7ebe557e5cd58537bf012ebc`)

   -  Fix file copy on ``finish --force``
      (:oecore_rev:`b8fda5dd1571bba32f894339e83f43d1872db986`)

   -  ``deploy-target``: Add ``--package``/``--file-glob`` filters
      (:oecore_rev:`cf6d07f8d8417b9c241b8e2ba1dbc1fd1e3998ff`)

   -  ``deploy``: Make ``pseudo`` calls independent of bitbake.conf
      (:oecore_rev:`462019b92fb023768a17df5162975e65e60fda47`)

   -  ``deploy``: Allow deploying directly into a local rootfs directory
      (:oecore_rev:`763accbf1ddc558175413b80fdfdc643b8d4ec47`)

-  Utility script changes:

   -  ``scripts/cve-json-to-text.py``: Simplify ``getopt`` argument parsing
      (:oecore_rev:`a92dfe569844189344fbb5ea0521b2bf7dbf0623`)

   -  ``upgrade`` command: Add a ``--stable`` option
      (:oecore_rev:`1e86aa039108621b2af734ef358a1e9d3c4d88d8`)

   -  ``oe-pkgdata-util``: Fix empty ``runtime-rprovides`` directory handling
      (:oecore_rev:`678c1c2077316b6b81ba9be000528b50dca19ca6`,
      :oecore_rev:`dbca656205a7d9a9a9b0aa25b4ad6562af9c5180`)

   -  ``recipeutils``: Add optional ``stable_upgrade`` parameter to
      ``get_recipe_upgrade_status``, to add the possibility of doing stable
      version upgrades of recipes
      (:oecore_rev:`1ed8fdda035dcc21f3df71c0c996973224f4f683`)

   -  ``scripts/pull-spdx-licenses.py``: Add exceptions to the script that
      updates the SPDX license files
      (:oecore_rev:`4190cdd6ceff9b88005f0845866a78eb407b0aa5`)

   -  ``install-buildtools``:

      -  Auto-discover environment setup script via glob
         (:oecore_rev:`1105378e967a812f3bdc3fcc25bb7fd5350cca6c`)

      -  Add a ``--local-file`` option
         (:oecore_rev:`e160ff4510de712ba43e37c8f2bfb1a57580bfaf`)

   -  ``scripts/contrib``: Add ``reuse-get-license.py``, a script that extracts
      the :term:`LICENSE` and :term:`LIC_FILES_CHKSUM` for projects that use
      `reuse <https://reuse.software/>`__
      (:oecore_rev:`8ec5b74051308d0882e2018bbec3be06958dae03`)

   -  ``scripts/contrib``: Add helper to report and fix missing :term:`SRC_URI`
      ``;tag=`` parameters
      (:oecore_rev:`d9a75fa667a89f9dfd81b8fdcea74c5b9048689b`)

   -  ``resulttool``: Add support for ptests results for both `Musl` and `Glibc`
      (:oecore_rev:`c0508fbea8d26242e0c761d412d727943f9d6355`)

   -  ``scripts/oe-depends-dot``: Give the ``dotfile`` argument a sensible default
      (:oecore_rev:`88620a3d917ec23ea3a4760a622bf319a3fe3d84`)

   -  ``sstate-cache-management.py``: Also check the :term:`SSTATE_DIR`
      environment variable for ``--cache-dir``
      (:oecore_rev:`ad44f5edfb440a97ad8add379489b649ae29ab1b`)

-  Clang/LLVM-related changes:

   -  ``clang``: Enable cmake flags for llvm, clang, lld tests
      (:oecore_rev:`c926c4f9f7ecf22698b7426609f8806feee175ca`)

-  Patchtest-related changes:

-  :ref:`ref-classes-insane` / :ref:`ref-classes-sanity` classes-related changes:

   -  Add a check that :term:`SOURCE_MIRROR_URL` is defined when
      :ref:`ref-classes-own-mirrors` is used
      (:oecore_rev:`6d989b9d15266ca0b3650c93ac961cffc3e82c14`)

   -  Add a check for build host ``HOME`` directory in packaged files
      (:oecore_rev:`81bbe42edbed590389a1b0ed4b1a58a6738dc760`)

   -  Allow exceptions in ``buildpaths`` ``HOME`` checks
      (:oecore_rev:`ee29a9132a2566ba03f28c5042554a5faf9d233e`)

   -  :ref:`ref-classes-sanity`: Warn on `Cargo` config files outside of the
      build tree
      (:oecore_rev:`77516d3c4fe4291eb947c2f7c636a362c041c911`)

   -  Increase shebang size limit
      (:oecore_rev:`c205e77fc394b50ddf4f4e0bec29bb1c8eec44d6`)

   -  Improve ``HOME`` checks exceptions
      (:oecore_rev:`f6b9494eac4d4da3ba72303ca5de82d5f60e48f0`)

-  Security changes:

-  :ref:`ref-classes-sbom-cve-check`-related changes:

-  New :term:`PACKAGECONFIG` options for individual recipes:

   -  ``curl``: ``libpsl``, ``libssh``
   -  ``dhcpcd``: ``seccomp``
   -  ``ethtool``: ``pretty-dump``
   -  ``ffmpeg``: ``libdav1d``
   -  ``iproute2``: ``dcb``, ``dpll``
   -  ``libical``: ``introspection``, ``vala``
   -  ``libportal``: ``gtk4``
   -  ``librsvg``: ``avif``, ``gdkpixbuf``
   -  ``p11-kit``: ``systemd``, ``trust``
   -  ``pciutils``: ``dns``, ``shared``
   -  ``perf``: ``bpf-skel``, ``llvm``
   -  ``seatd``: ``manpages``
   -  ``strace``: ``libdw``
   -  ``vim``: ``desktop``
   -  ``webkitgtk``: ``bubblewrap``, ``librice``, ``webrtc``
   -  ``wpa-supplicant``: ``suiteb``, ``mbo``, ``wnm``

-  systemd-related changes:

   -  ``systemd-tools-native``: Add ``systemd-hwdb``
      (:oecore_rev:`2a7c2ddd739db59bc14d781aa919bdd4724b365e`)

   -  ``systemd``: Enable ``coredump`` in :term:`PACKAGECONFIG` by default
      (:oecore_rev:`1086a56d222fac8c602f086cc3d6467b34f4d61e`)

-  U-Boot-related changes:

   -  Add support for `Clang` toolchain
      (:oecore_rev:`321421117de088e6b23da9c746d16df0c6081323`)

   -  :ref:`ref-classes-uboot-sign`: List the TEE loadable behind U-Boot
      (:oecore_rev:`a9a20a0c95eba16e0dce0abe5e9a3998e4520386`)

-  Busybox-related changes:

   -  ``fdisk``: Enable GPT support
      (:oecore_rev:`d0914b892b9236ad18ba29d4807d5adbbae992e6`)

   -  Enable ``xxd``
      (:oecore_rev:`2852025109cb23577897461f4d2bdfd213602461`)

   -  Enable ``findfs`` for initramfs ``LABEL`` support
      (:oecore_rev:`c2b681e39421ae5b4fea917b3c3ed69bd1020b6c`)

-  ``initramfs-framework``-related changes:

   -  ``overlayroot``: Use ``switch_root`` instead of ``chroot``
      (:oecore_rev:`848c368291b59ce6e928cdc973f94b34a6cdaa12`)

   -  Add opt-in root-only ``udev`` trigger
      (:oecore_rev:`d3a196f5742425fb24cfc21bb380f3e96c49253c`)

   -  Support ``LABEL`` with root-only ``udev`` trigger
      (:oecore_rev:`fda394c2b91b3c66545f75dc13f7e8649e3cdb6d`)

   -  Avoid external commands in rootfs resolution
      (:oecore_rev:`d6e8e722763f87ceef9c0dbd1ec7f38088e7f0df`)

   -  Allow opting out of ``efivarfs`` mount
      (:oecore_rev:`9147744020f9b948f656a116ac03a9656e5a254b`)

-  Miscellaneous changes:

   -  ``pulseaudio``: Split pactl into a dedicated client subpackage
      (:oecore_rev:`31dd308b3fe68a6142738100e8ad18a09bccbce6`)

   -  ``u-boot-tools``: Add dependency on ``libyaml`` for ``dtschema`` validation
      (:oecore_rev:`02e09e036e5d037b29f2a53d63c2231535da1a5e`)

   -  ``p11-kit``: Rewrite packaging to provide additional ``p11-kit-modules``
      and ``p11-kit-remote`` packages
      (:oecore_rev:`7511a8624e79ab5c9fc242bcf3c6b74c828c8584`)

   -  ``harfbuzz``: Improve packaging
      (:oecore_rev:`fb39870f27b19af790244a20ae9887923df8e464`)

   -  :ref:`ref-classes-sstate`: Detect broken shared state paths containing
      :term:`TMPDIR`
      (:oecore_rev:`907af8fb448e2f9ecf8e0439f2d8c7c397fb873f`)

   -  ``gcr``: Package the ssh-agent into a separate package
      (:oecore_rev:`c9579094da0a6a178cab9f370ab7cb0e414d6ca9`)

   -  ``glibc``: Disable automatic ``libatomic`` linking
      (:oecore_rev:`677f0acc96072e65442158519589cea4294a88f9`)

   -  :ref:`ref-classes-uboot-sign`: Sign SPL FIT into a copy of the SPL DTB
      (:oecore_rev:`79584fe7e7efec4fc9153217d74b1d48774df911`)

   -  :ref:`ref-classes-archiver`: Properly remove artifacts when configuration changes
      (:oecore_rev:`4b0ac92f28caa8b7ae7d645afbeff1ebc34b36dc`)

   -  ``wget``: Disable NTLM support
      (:oecore_rev:`4eb7e98020eb5b87990f5fd5929adf7e333dc038`)

   -  ``vex``: Drop obsolete conflict check with ``cve-check`` class
      (:oecore_rev:`ddb00a3eaeb5fa7c84636aad9fb841f61cd99fa7`)

   -  :ref:`ref-classes-toaster`: Support layers that are not Git repositories
      (:oecore_rev:`3fb96cc91dc4e625502751b64ac982e5ba6f0cb8`)

   -  ``classes/meson``: Use ninja explicitly when compiling
      (:oecore_rev:`9e1513b29849c56fe372e0d73ce229cebed32d9e`)

   -  ``webkitgtk``: Allow using `Clang` to compile for arm target
      (:oecore_rev:`77023812cf13a96d67aef7ad9a743f66861bd759`)

   -  :ref:`ref-classes-report-error`: Add ``site.conf``/``toolcfg.conf`` into error report
      (:oecore_rev:`40ca5fc9098be25574130a3810560219b8ee9d07`)

   -  :ref:`ref-classes-cml1`: Use :term:`KCONFIG_CONFIG_ROOTDIR` in savedefconfig
      (:oecore_rev:`46964503b6340d4244d1c19e90808f399a3c862c`)

   -  :ref:`ref-classes-uki`: Make `initramfs` optional
      (:oecore_rev:`909d3ba1fa925c105681c091af00965fc4aac191`)

   -  ``cpan_build``: Disable ``.packlist`` and HTML doc
      (:oecore_rev:`3c2bb7bce1da68e31a60de044ee1dd9abb2eadc1`)

   -  :ref:`ref-classes-useradd`: Drop ``groupmems`` from :term:`sysroot` setup,
      as ``groupmems`` was `removed <https://github.com/shadow-maint/shadow/pull/1601>`__
      from `shadow` version 4.20.0
      (:oecore_rev:`de0bf5771644edbe317ee9ad89cca4bc8836cf12`)

   -  :ref:`ref-classes-image_types`: Make ``oe_mkext234fs`` reproducible
      (:oecore_rev:`8d1d548e36fa8426f20f43a514f0dd60b699f86b`)

   -  :ref:`ref-classes-pypi`: Only set ``downloadprefix`` in :term:`SRC_URI` if needed
      (:oecore_rev:`be391d8a8b7d4898063ceae1fbd707bfd2218be3`)

Known Issues in |yocto-ver|
---------------------------

N/A

Recipe License changes in |yocto-ver|
-------------------------------------

..
   Going through commits on OE-Core filtered by License-Update:
   git log -U0 --patch --grep "License-Update:" yocto-6.0..origin/master

Security Fixes in |yocto-ver|
-----------------------------

..
   Generated with documentation/tools/gen-cve-release-notes

Recipe Upgrades in |yocto-ver|
------------------------------

..
   Generated with https://layers.openembedded.org/layerindex/branch_comparison
   With "rST" output selected

Contributors to |yocto-ver|
---------------------------

..
   List obtained with the following shell snippet:

      authors=""
      for repo in openembedded-core yocto-docs bitbake meta-yocto; do
         authors="${authors}\n$(git --no-pager -C $repo log --format="-  %an" yocto-6.0..origin/master)"
      done
      echo $authors | sort | uniq

   Email addresses and duplicates removed.

Thanks to the following people who contributed to this release:

Repositories / Downloads for Yocto-|yocto-ver|
----------------------------------------------
