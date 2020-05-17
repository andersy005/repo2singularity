![GitHub Workflow Status](https://img.shields.io/github/workflow/status/andersy005/repo2singularity/CI?logo=github&style=for-the-badge)
![GitHub Workflow Status](https://img.shields.io/github/workflow/status/andersy005/repo2singularity/code-style?label=code%20style&logo=github&style=for-the-badge)

# Repo2singularity

- [Repo2singularity](#repo2singularity)
  - [Usage](#usage)
  - [Examples](#examples)
    - [Example 1: Building image for the first time](#example-1-building-image-for-the-first-time)
      - [Note on setting up an SSH tunnel](#note-on-setting-up-an-ssh-tunnel)
      - [Note on requirements](#note-on-requirements)
    - [Example 2: Build and Push image to  syslabs.cloud](#example-2-build-and-push-image-to-syslabscloud)
    - [Example 3: Pull a previously uploaded image from syslabs.cloud and run it locally](#example-3-pull-a-previously-uploaded-image-from-syslabscloud-and-run-it-locally)

Wrapper around [repo2docker](https://github.com/jupyter/repo2docker) producing Jupyter enabled Singularity images.

## Usage

```bash
$ repo2singularity --help
Usage: repo2singularity [OPTIONS] REPO

  Fetch a repository and build a singularity image.

Options:
  --ref TEXT                    Remote repository reference to build.
                                [default: master]

  --image-name TEXT             Name of image to be built. If unspecified will
                                be autogenerated.  [default: ]

  --push / --no-push            Push singularity image to image registry.
                                [default: False]

  --username-prefix TEXT        Username and prefix to use when constructing
                                image URI before pushing it to or pulling it
                                from the registry. For example, user/prefix:
                                `milkshake/chocolate`. Used in conjunction
                                with --push and --run (to pull existing
                                image).  [default: ]

  --run / --no-run              Run container after it has been built.
                                [default: False]

  --bind TEXT                   Volumes to mount inside the container, in form
                                src[:dest[:opts]], where src and dest are
                                outside and inside paths.  If dest is not
                                given, it is set equal to src. Mount options
                                ('opts') may be specified as 'ro' (read-only)
                                or 'rw' (read/write, which is the default)
                                [default: ]

  --json-logs / --no-json-logs  Emit JSON logs instead of human readable logs.
                                [default: False]

  --debug / --no-debug          Turn on debug logging.  [default: False]
  --version                     Print the repo2singularity version and exit.
  --install-completion          Install completion for the current shell.
  --show-completion             Show completion for the current shell, to copy
                                it or customize the installation.

  --help                        Show this message and exit.
```

## Examples

### Example 1: Building image for the first time

```bash
$ repo2singularity --run https://github.com/norvig/pytudes

[I 21:19:17.090 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[W 21:19:17.102 LabApp] No web browser found: could not locate runnable browser.
[C 21:19:17.102 LabApp]

    To access the notebook, open this file in a browser:
        file:///home/USERNAME/.local/share/jupyter/runtime/nbserver-17263-open.html
    Or copy and paste one of these URLs:
        http://HOSTNAME:43111/?token=eb5f94c4ffccd7fff5f2f3a4dfc3aa2fc9e361c1a529bd25
     or http://127.0.0.1:43111/?token=eb5f94c4ffccd7fff5f2f3a4dfc3aa2fc9e361c1a529bd25
```

#### Note on setting up an SSH tunnel

If you are running Singularity on an HPC system, you may need to set up an SSH tunnel to access the jupyter server. On the local machine, start an SSH tunnel:

```bash
ssh -N -L local-address:local-port:remote-address:remote-port remote-user@remote-host
```

#### Note on requirements

When building an image for the first time, **Docker** and [**Singularity**](https://github.com/hpcng/singularity) need to be running on your machine for this to work.

### Example 2: Build and Push image to  [syslabs.cloud](https://cloud.sylabs.io/library)

```bash
repo2singularity --push --username-prefix andersy005/test https://github.com/norvig/pytudes
```

### Example 3: Pull a previously uploaded image from [syslabs.cloud](https://cloud.sylabs.io/library) and run it locally

```bash
repo2singularity --pull --username-prefix andersy005/test --run  https://github.com/norvig/pytudes
```

Note: Pulling an existing singularity image from [syslabs.cloud](https://cloud.sylabs.io/) requires having **singularity** installed.
