
<div align="center">
    <img src="docs/assets/banner.png" height=120 alt="banner"/>

-----


[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)
[![License](https://img.shields.io/badge/license-Apache%202-blue.svg)](LICENSE)



A longitudinal database of ML API predictions. 

[**Getting Started**](#%EF%B8%8F-quickstart)
| [**Website**](https://hapi-explore.github.io/)
| [**Contributing**](CONTRIBUTING.md)
| [**About**](#%EF%B8%8F-about)
</div>


## 💡 What is HAPI?
History of APIs (HAPI) is a large-scale, longitudinal database of commercial ML API predictions. It contains 1.7 million predictions collected from 2020 to 2022 and spanning APIs from Amazon, Google, IBM, and Microsoft. The database include diverse machine learning tasks including image tagging, speech recognition and text mining.



## ⚡️ Quickstart
We provide a lightweight python package for getting started with HAPI. 
```bash
pip install hapi @ git+https://github.com/lchen001/hapi@main
```

Import the library and download the data, optionally specifying the directory for the
the download. If the directory is not specified, the data will be downloaded to
`~/.hapi`.


```python
import hapi

hapi.config.data_dir = "/path/to/data/dir" 

hapi.download()
```

> You can permanently set the data directory by adding the variable `HAPI_DATA_DIR` to your environment.

Once we've downloaded the database, we can list the available APIs, datasets, and tasks with `hapi.summary()`. This returns a [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) with columns `task, dataset, api, date, path, cost_per_10k`. 
```python
df = hapi.summary()
```

To load the predictions into memory we use `hapi.get_predictions()`. The keyword arguments allow us to load predictions for a subset of tasks, datasets, apis and/or dates. 
```python
predictions =
hapi.get_predictions(task="mic", dataset="pascal", api=["google_mic", "ibm_mic"])
```

The predictions are returned as a dictionary mapping from `"{task}/{dataset}/{api}/{date}"` to lists of dictionaries, each with keys `"example_id"`, `"predicted_label"`, and `"confidence"`. For example:
```python
{
    "mic/pascal/google_mic/20-10-28": [
        {
            'confidence': 0.9798267782,
            'example_id': '2011_000494',
            'predicted_label': ['bird', 'bird']
        },
        ...
    ],
    "mic/pascal/microsoft_mic/20-10-28": [...],
    ...
}
```

To load the labels into memory we use `hapi.get_labels()`. The keyword arguments allow us to load labels for a subset of tasks and datasets.
```python
labels = hapi.get_labels(task="mic", dataset="pascal")
```

The labels are returned as a dictionary mapping from `"{task}/{dataset}"` to lists of dictionaries, each with keys `"example_id"` and `"true_label"`. 




## 💾  Manual Downloading
The database is stored in a GCP bucket named [`hapi-data`](https://console.cloud.google.com/storage/browser/hapi-data). All model predictions are stored in [`hapi.tar.gz`](https://storage.googleapis.com/hapi-data/hapi.tar.gz) (Compressed size: `205.3MB`, Full size: `1.2GB`). 
    
From the command line, you can download and extract the predictions with: 
```bash
    wget https://storage.googleapis.com/hapi-data/hapi.tar.gz && tar -xzvf hapi.tar.gz 
```
However, we recommend downloading using the Python API as described above. 


## ✉️ About
`HAPI` was developed at Stanford in the Zou Group. Reach out to Lingjiao Chen (lingjiao [at] stanford [dot] edu) and Sabri Eyuboglu (eyuboglu [at] stanford [dot] edu) if you would like to get involved!
