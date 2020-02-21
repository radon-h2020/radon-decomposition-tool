# radon-decomposition-tool

| Items | Contents |
| --- | --- |
| **Description** | The decomposition tool is present to help RADON users in finding the optimal decomposition solution for an application based on the microservices architectural style and the serverless FaaS paradigm. |
| **Maintainer(s)**| <ul><li>Lulai Zhu (<lulai.zhu15@imperial.ac.uk>)</li><li>Giuliano Casale (<giuliano.casale@imperial.ac.uk>)</li></ul> |

## Public Access Server
The decomposition tool has been made available on an Amazon EC2 instance with a RESTful API, which can be access through the URL: `http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000`. The next table summarizes RESTful endpoints exposed by this public access server.

| Path | Method | Params | Body | Response | Description |
| --- | --- | --- | --- | --- | --- |
| `/files/{filename}` | POST |  |`file: formdata` |  | Upload a file to the server |
| `/files/{filename}` | GET |  |  |  | Download a file from the server |
| `/files/{filename}` | DELETE |  |  |  | Delete a file in the server |
| `/files` | GET |  |  | `filenames: array` | List all the files in the server |
| `/dec-tool/optimize` | PATCH | `filename: string` |  | `total_cost: number` | Optimize the deployment of a RADON model |

## Demo Application Example
The repository provides a demo application example (thumbnail generation) based on the definitions of TOSCA types specific to the decomposition tool. Two RADON models are included in this example, one with an open workload and the other with a closed workload. To try the decomposition tool on the former, please perform the following steps:
1. Upload the original model to the server:

		curl -X POST http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.yml -F 'file=@open_model.yml'
2. Optimize the deployment of the model:

		curl -X PATCH http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/dec-tool/optimize?filename=model.yml
2. Download the resultant model from the server:

		curl -X GET http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.yml -o open_model.yml
It is ***HIGHLY RECOMMENDED*** to use a different filename for each uploaded model, e.g. putting a UUID into the filename (`model_5da82fdc-ae4c-48c4-ab5f-369a9a4fdee3.yml`), so as to minimize the possibility of collisions between concurrent requests. Last but ***MOST IMPORTANTLY***, do not upload a model with any sensitive information, e.g. a pair of AWS access key id and secrete access key. We currently cannot prevent other users from accessing your models in the server.
