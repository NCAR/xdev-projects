# Project: Single File Catalog

[TOC]

## Overview

The current spec requires that the `catalog_file` point to a `csv file`. In some cases, it would be useful to embed the catalog "table" in the catalog itself. A so called single-file-catalog. STAC has an extension that does this (see [here](https://github.com/radiantearth/stac-spec/tree/master/extensions/single-file-stac)).




## Deliverables - Functionality

- Make `catalog_file` key optional and support a key `catalog_dict` which is a json dictionary that represents the data that would otherwise be in the csv. Exactly one of the two keys would be required but the catalog creator could choose. 

The `catalog_dict` dictionary can be expressed as 

**Option 1) dict**: dict like ```{column -> {index -> value}}```:

```yaml
{
    "esmcat_version":"0.1.0",
    "id":"aws-cesm1-le",
    "description":"This is an ESM collection for CESM1 Large Ensemble Zarr dataset publicly available on Amazon S3 (us-west-2 region)",
    "catalog_dict":{
        'component':{
            0:'atm',
            1:'atm',
            2:'atm',
            3:'atm',
            4:'atm'
        },
        'frequency':{
            0:'daily',
            1:'daily',
            2:'daily',
            3:'daily',
            4:'daily'
        },
        'experiment':{
            0:'20C',
            1:'20C',
            2:'20C',
            3:'20C',
            4:'20C'
        },
        'variable':{
            0:'FLNS',
            1:'FLNSC',
            2:'FLUT',
            3:'FSNS',
            4:'FSNSC'
        },
        'path':{
            0:'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLNS.zarr',
            1:'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLNSC.zarr',
            2:'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLUT.zarr',
            3:'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FSNS.zarr',
            4:'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FSNSC.zarr'
        }
    },
    "attributes":[
        {
            "column_name":"component",
            "vocabulary":""
        },
        {
            "column_name":"frequency",
            "vocabulary":""
        },
        {
            "column_name":"experiment",
            "vocabulary":""
        },
        {
            "column_name":"variable",
            "vocabulary":""
        }
    ],
    "assets":{
        "column_name":"path",
        "format":"zarr"
    },
    "aggregation_control":{
        "variable_column_name":"variable",
        "groupby_attrs":[
            "component",
            "experiment",
            "frequency"
        ],
        "aggregations":[
            {
                "type":"union",
                "attribute_name":"variable",
                "options":{
                    "compat":"override"
                }
            }
        ]
    }
}
```

or 

**Option 2) records**: ```[{column -> value}, ... , {column -> value}]```

```yaml 
{
    "esmcat_version":"0.1.0",
    "id":"aws-cesm1-le",
    "description":"This is an ESM collection for CESM1 Large Ensemble Zarr dataset publicly available on Amazon S3 (us-west-2 region)",
    "catalog_dict":[
        {
            'component':'atm',
            'frequency':'daily',
            'experiment':'20C',
            'variable':'FLNS',
            'path':'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLNS.zarr'
        },
        {
            'component':'atm',
            'frequency':'daily',
            'experiment':'20C',
            'variable':'FLNSC',
            'path':'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLNSC.zarr'
        },
        {
            'component':'atm',
            'frequency':'daily',
            'experiment':'20C',
            'variable':'FLUT',
            'path':'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FLUT.zarr'
        },
        {
            'component':'atm',
            'frequency':'daily',
            'experiment':'20C',
            'variable':'FSNS',
            'path':'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FSNS.zarr'
        },
        {
            'component':'atm',
            'frequency':'daily',
            'experiment':'20C',
            'variable':'FSNSC',
            'path':'s3://ncar-cesm-lens/atm/daily/cesmLE-20C-FSNSC.zarr'
        }
    ],
    "attributes":[
        {
            "column_name":"component",
            "vocabulary":""
        },
        {
            "column_name":"frequency",
            "vocabulary":""
        },
        {
            "column_name":"experiment",
            "vocabulary":""
        },
        {
            "column_name":"variable",
            "vocabulary":""
        }
    ],
    "assets":{
        "column_name":"path",
        "format":"zarr"
    },
    "aggregation_control":{
        "variable_column_name":"variable",
        "groupby_attrs":[
            "component",
            "experiment",
            "frequency"
        ],
        "aggregations":[
            {
                "type":"union",
                "attribute_name":"variable",
                "options":{
                    "compat":"override"
                }
            }
        ]
    }
}
```



### esm-collection-spec side

- [ ] Update the [specification file](https://github.com/NCAR/esm-collection-spec/blob/master/collection-spec/collection-spec.md)
- [ ] Update the [validator script](https://github.com/NCAR/esm-collection-spec/blob/master/esmcol_validator/validator.py)

### intake-esm side

- [ ] Update/add a new argument `catalog_type` to [`serialize() method`](https://github.com/NCAR/intake-esm/blob/master/intake_esm/core.py#L131)???

  - Accepted values for `catalog_type` would include `dict` for `catalog_dict`     key and `file` for `catalog_file` key.
  - Errors if `catalog_type` not in `{"dict", "file"}`
  - Should we add an `orient` argument to control the type of the values of the dictionary??? For instance, `orient='dict'` would yield option 1) and `orient='records'` would yield option 2) described above. 

```python
   def serialize(self, name, catalog_type='file', 
                  orient='records', directory=None):
       ...
```

        
- [ ] Update [`_fetch_catalog()` method](https://github.com/NCAR/intake-esm/blob/master/intake_esm/core.py#L126) so that it can support reading the catalog from dictionary specified in `catalog_dict`:

```python

    @lru_cache(maxsize=None)
    def _fetch_catalog(self):
        """Get the catalog content and cache it.
        """
        
        if 'catalog_file' in self._col_data:
            return pd.read_csv(self._col_data['catalog_file'])
        else:
            return pd.DataFrame(self._col_data['catalog_dict'])
       ...
```


## Milestones - Metrics (TODO)

## Timeline (TODO)


## References

- [https://github.com/NCAR/esm-collection-spec/issues/13](https://github.com/NCAR/esm-collection-spec/issues/13)

- [https://github.com/NCAR/intake-esm/pull/179#issuecomment-553630201](https://github.com/NCAR/intake-esm/pull/179#issuecomment-553630201)

- [https://github.com/NCAR/intake-esm/issues/166](https://github.com/NCAR/intake-esm/issues/166)




###### tags: `intake-esm` `esm-collection-spec`