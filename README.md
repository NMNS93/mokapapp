# Moka Panel App (mokapapp)

![MokaPapp Logo](_assets/mokapapp.png)

`mokapapp` imports [Panel App](https://www.genomicsengland.co.uk/about-genomics-england/panelapp/) panels into the Viapath Genetics LIMS system (Moka).

Simply [install](#installation), create a [configuration file](#configuration-file) and run:

```bash
$ mokapapp-import -c config.ini
```

## Features

* Deactviates any deprecated Panel App panels in the Moka database
* Builds MokaPanel objects with a unique hash for each panel's green and amber genes, required for import into Moka.
* Performs data validation checks before import:
  * Are all panel hashes in dbo.Item?
  * Are all panel versions in dbo.Item?
  * Are all panel HGNC IDs in dbo.GenesHGNC_Current?
* Raises an error if the Moka HGNC snapshot is outdated

## Installation

Clone this repository and install with `pip` (tested on a Linux OS with python v3.6.5):

```
$ git clone https://github.com/moka-guys/mokapapp.git
$ pip install ./mokapapp
```

## Usage

```
usage: mokapapp-import [-h] [-c CONFIG] [--logfile LOGFILE] [--head HEAD]

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG, --config CONFIG
                        A mokapapp config.ini file
  --logfile LOGFILE     A file to write application log outputs
  --head HEAD           An integer limit for the number of PanelApp panels to process
```

## Config file

`mokapapp-import` requires a `config.ini` file with Moka database connection details:

```bash
# Example config.ini
[mokadb]
server = 10.10.10.10
db = dbname
user = username
password = password
```

## Tests

Unit tests in the `tests/` directory are run using `pytest`. An *auth.py* file containing test server details must be present in  `tests/`:
```python
# Example auth.py
MOKADB = {
    'server': '10.10.10.100',
    'db': 'database',
    'user': 'username',
    'password': 'password'
}
```

Once this file has been created run all tests:
> $ pytest .

## Notes

* Panel App gene confidence levels are converted to Green, Amber and Red colours as specified in Appendix A p.29 of the [Panel App Handbook v5.5](https://panelapp.genomicsengland.co.uk/media/files/PanelAppHandbookVersion55.pdf).

## License

MIT License © 2019 Nana Mensah