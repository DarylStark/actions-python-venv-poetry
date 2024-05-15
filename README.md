# GitHub Action - Python VENV - Poetry

This GitHub Action gives your CI/CD pipeline a Python VENV with your Poetry dependencies installed. It will cache the VENV for subsequent jobs and runs, resulting in a more preformant pipeline. When the cache exists, nothing will be installed, and the cache will be used. When the cache does not exist, the VENV will be created and dependencies will be installed. By default, it will base the cache key on the hash of the `poetry.lock` file. This way, the cache is only recreated when your dependencies change.

To use this GitHub Action, add the following to your `.github/workflows/<workflow>.yml`:

```yaml
jobs:
  create-venv:
    name: Test the VENV action
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository
        uses: actions/checkout@v4
      - name: Set up Python Virtual Environment (Poetry)
        uses: DarylStark/actions-python-venv-poetry@v1
```

This will create a Python VENV with Poetry installed and your dependencies installed.

## Inputs

The following inputs are available:

| Input            | Required | Description                                                                                                      |
| ---------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `cache-key`      | No       | The cache key to use for the VENV. If not specified  it will use a value based on the contents of `poetry.lock`. |
| `python-version` | No       | The Python version to use. By default, this will be `3.12`.                                                      |
| `groups`         | No       | The Poetry dependency groups to install (example: `dev,test`)                                                    |

## Examples

To create a VENV for multiple Python versions:

```yaml
jobs:
  create-venv:
    name: Test the VENV action
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository
        uses: actions/checkout@v4
      - name: Set up Python Virtual Environment (Poetry)
        uses: DarylStark/actions-python-venv-poetry@dev
        with:
          python-version: ${{ matrix.python-version }}
          cache-key: python-venv-poetry-${{ hashFiles('./poetry.lock') }}-${{ matrix.python-version }}
    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11', '3.12']
```

To install specific dependency groups:

```yaml
jobs:
  create-venv:
    name: Test the VENV action
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository
        uses: actions/checkout@v4
      - name: Set up Python Virtual Environment (Poetry)
        uses: DarylStark/actions-python-venv-poetry@v1
        with:
          groups: "dev,test"
```