# radon-decomposition-tool

| Items | Contents |
| --- | --- |
| **Short Description** | The decomposition tool is present to help RADON users in finding the optimal decomposition solution for an application based on the microservices architectural style and the serverless FaaS paradigm. It supports three typical usage scenarios: (1) architecture decomposition, (2) deployment optimization, (3) accuracy enhancement, (4) assignment consolidation. |
| **Documentation** | [D3.2 – Decomposition Tool I](http://radon-h2020.eu/wp-content/uploads/2020/01/D3.2-Decomposition-Tool-I.pdf) |
| **Stand-Alone Tutorial** | https://decomposition-tool.readthedocs.io/en/latest/ |
| **Video** | https://youtu.be/ZHD0t8HK7K0 |
| **Repository** | <ul><li>https://github.com/radon-h2020/radon-decomposition-tool</li><li>https://github.com/radon-h2020/radon-decomposition-plugin</li><li>https://github.com/radon-h2020/radon-decomposition-enhancement</li><li>https://github.com/radon-h2020/radon-decomposition-interference</li></ul> |
| **License** | [Imperial College's Intellectual Property](https://www.imperial.ac.uk/enterprise/business/industry-partnerships-and-commercialisation/technology-licensing/) |
| **Contact**| <ul><li>Lulai Zhu ([@zhululai](https://github.com/zhululai))</li><li>Giuliano Casale ([@gcasale](https://github.com/gcasale))</li></ul> |

## Public Access Server
The decomposition tool has been made available on an Amazon EC2 instance with a RESTful API, which can be accessed through the URL: `http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000`. The next table summarizes RESTful endpoints exposed by this public access server.

| Path | Method | Parameters | Body | Response | Description |
| --- | --- | --- | --- | --- | --- |
| `/file/{filename}` | POST |  | `file: octet-stream` |  | Upload a file to the server |
| `/file/{filename}` | GET |  |  |  | Download a file from the server |
| `/file/{filename}` | DELETE |  |  |  | Delete a file in the server |
| `/dec-tool/decompose` | PATCH | `model_filename: string` |  |  | Decompose the architecture of a RADON model |
| `/dec-tool/optimize` | PATCH | `model_filename: string` |  | `total_cost: number`, `measures: object array` | Optimize the deployment of a RADON model |
| `/dec-tool/enhance` | PATCH | `model_filename: string`, `data_filename: string` |  |  | Enhance the accuracy of a RADON model |
| `/dec-tool/consolidate` | PATCH | `model_filename: string`, `data_filename: string` |  |  | Consolidate the assignment of a RADON model |

## Demo Application Example
A demo application example (thumbnail generation) based on the definitions of TOSCA types specific to the decomposition tool is provided here. Two RADON models are included in this example, one with an open workload and the other with a closed workload. To try the decomposition tool on the former, please perform the following steps:
1. Clone the repository and enter the model directory

		git clone https://github.com/radon-h2020/radon-decomposition-tool.git && cd radon-decomposition-tool/demo-app
2. Upload the original model to the server:

		curl -X POST http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.tosca -F 'file=@open_model.tosca'
3. Optimize the deployment of the model:

		curl -X PATCH http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/dec-tool/optimize?filename=model.tosca
4. Back up the original model in place:

		cp open_model.tosca open_model.tosca.bkp
5. Download the resultant model from the server:

		curl -X GET http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.tosca -o open_model.tosca
It is ***HIGHLY RECOMMENDED*** to use a different filename for each uploaded model, e.g. putting a UUID into the filename (`model_5da82fdc-ae4c-48c4-ab5f-369a9a4fdee3.tosca`), so as to minimize the possibility of collisions between concurrent requests. Last but ***MOST IMPORTANTLY***, do not upload a model with any sensitive information, e.g. a pair of AWS access key ID and secrete access key. We currently cannot prevent other users from accessing your models in the server.
