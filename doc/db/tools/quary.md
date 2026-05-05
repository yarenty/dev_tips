# Quary


https://github.com/quarylabs/quary


## With Quary, engineers can:
- 🔌 Connect to their Database
- 📖 Write SQL queries to transform, organize, and document tables in a database
- 📊 Create charts, dashboards and reports (in development)
- 🧪 Test, collaborate & refactor iteratively through version control
- 🚀 Deploy the organised, documented model back up to the database


## Asset Types in Quary
Define and manage the following asset types as code:

- Sources: Define the external data sources that feed into Quary, such as database tables, flat files, or APIs (with DuckDB).
- Models: Transform raw data from sources into analysis-ready datasets using SQL, this lets engineers split complex queries into atomic components.
- Charts: Create visual representations of your data using SQL.
- 🚧 Dashboards (WIP): Combine multiple charts into a single view, allowing engineers to monitor and analyze data in one place.
- 🚧 Reports (WIP): Create detailed reports to share insights and findings with your team or stakeholders.

## 🚀 Getting Started
### Installation
Quary is a VSCode Extension (Interface) & Rust-based CLI (Core)

### Extension
The VSCode extension can be installed here. Note that it depends on the CLI being installed.

[https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension](https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension)

### CLI

Homebrew installation
```shell
brew install quarylabs/quary/quary
```
Linux/Mac through curl
Quary can be installed using curl on Linux/Mac using the following command:

```shell
curl -fsSL https://raw.githubusercontent.com/quarylabs/quary/main/install.sh | bash
```

Other installations
Other builds are available in the releases page to download.

## Usage
Once installed, a sample project can be created and run as follows:
```shell
mkdir example # create an empty project folder
cd example
quary init    # initialize DuckDB demo project with sample data
quary compile # validate the project structure and model references without database
quary build   # build and execute the model views/seeds against target database
quary test -s   # run defined tests against target database
```
