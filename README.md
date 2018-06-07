# emMorphPy
A wrapper, a lemmatizer and REST API implemented in Python for ___emMorph__ (Humor) Hungarian morphological analyzer_ 

## Requirements

  - (Included in this repository) The compiled FST (hu.hfstol): go to https://github.com/dlt-rilmta/emMorph for compilation details
  - (Included in this repository) The lemmatizer config file: available at https://github.com/dlt-rilmta/hunlp-GATE/blob/master/Lang_Hungarian/resources/hfst/hfst-wrapper.props
  - _hfst-lookup 0.6 (hfst 3.13.0)_ or higher: On Ubuntu 18.04 LTS or higher just `sudo apt install hfst`
  - Python 3 (tested with 3.6)
  - Pip to install the additional requirements in requirements.txt
  - (Optional) a cloud service like [Heroku](https://heroku.com) for hosting the API

## Install on local machine

  - Clone the repository
  - Run: `sudo pip3 install -r requirements.txt`
  - Use from Python!

## Install to Heroku

  - Register
  - Download Heroku CLI
  - Login to Heroku from the CLI
  - Create an app
  - Clone the repository
  - Add Heroku as remote origin
  - Add APT buildpack: `heroku buildpacks:add --index 1 https://github.com/heroku/heroku-buildpack-apt`
  - Push the repository to Heroku!
  - Enjoy!

## Usage

  - From browser or anyhow through the REST API:
     - Lemmatization: https://emmorph.herokuapp.com/stem/működik
     - Detailed analyzis: https://emmorph.herokuapp.com/analyze/működik
     - Lemmatisation with the corresponding detailed analyzis: https://emmorph.herokuapp.com/dstem/működik

	```python
	>>> import requests
	>>> import json
	>>> word = 'működik'
	>>> json.loads(requests.get('https://emmorph.herokuapp.com/stem/' + word).text)[word]
	[['működik', '[/V][Prs.Def.3Pl]'], ['működik', '[/V][Prs.NDef.3Sg]']]
	>>> json.loads(requests.get('https://emmorph.herokuapp.com/analyze/' + word).text)[word]
	['működik[/V]=működ+ik[Prs.Def.3Pl]=ik', 'működik[/V]=működ+ik[Prs.NDef.3Sg]=ik']
	>>> json.loads(requests.get('https://emmorph.herokuapp.com/dstem/' + word).text)[word]
	[['működik', '[/V][Prs.Def.3Pl]', 'működik[/V]=működ+ik[Prs.Def.3Pl]=ik'], ['működik', '[/V][Prs.NDef.3Sg]', 'működik[/V]=működ+ik[Prs.NDef.3Sg]=ik']]
	```
 
  - From Python:

	```python
	>>> import emmorphpy.emmorphpy as emmorph
	>>> m = emmorph.EmMorphPy()
	>>> m.stem('működik')  # Returns list of lemmatisations (stem and tag pairs)
	[['működik', '[/V][Prs.Def.3Pl]'], ['működik', '[/V][Prs.NDef.3Sg]']]
	>>> m.analyze('működik')  # Returns list of detailed analyzes (word by morphemes)
	['működik[/V]=működ+ik[Prs.Def.3Pl]=ik', 'működik[/V]=működ+ik[Prs.NDef.3Sg]=ik']
	>>> m.dstem('működik')  # Returns list of lemmatisations with the corresponding detailed analyzes (stem, tag and detailed analyzes triples)
	[['működik', '[/V][Prs.Def.3Pl]', 'működik[/V]=működ+ik[Prs.Def.3Pl]=ik'], ['működik', '[/V][Prs.NDef.3Sg]', 'működik[/V]=működ+ik[Prs.NDef.3Sg]=ik']]
	```


## License

This Python wrapper, the lemmatizer implementation and the REST API is licensed under the LGPL 3.0 license.
The database and the lemmatizer configuration has its own license!
