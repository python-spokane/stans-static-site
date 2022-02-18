# Stan's Static Site 
A static website about Stan the Snakes's adventures created using Python's MkDocs

## TODO
- [x] Add MkDocs site
- [ ] Add info about Stan's latest adventures
- [ ] Add info about how to host on AWS, Azure, ...

## Getting Started

### Create virtual environment
```python
python -m virtualenv .venv
```

### Activate virtual environment  
Bash
```
source .venv/bin/activate
```

PowerShell
```
.\.venv\Scripts\activate.ps1
```

### Install dependencies
```
pip install -r requirements.txt
```

### Debug site
```
cd website
mkdocs serve
```

### Build site
```
cd website
mkdocs build
```

## AWS S3 Hosting
- [S3 Example Static Web Host](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-example-static-web-host.html)
- [Boto3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
