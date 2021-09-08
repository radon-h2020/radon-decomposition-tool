The decomposition tool has been made available on an Amazon EC2 instance with a RESTful API, which can be accessed through the URL: :code:`http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000`. The next table summarizes RESTful endpoints exposed by this public access server.

=============================== ======== =============================================================== ============================ ============================================================ =============================================
 Path                            Method   Parameters                                                      Body                          Response                                                     Description
=============================== ======== =============================================================== ============================ ============================================================ =============================================
 :code:`/file/{filename}`        POST                                                                     :code:`file: octet-stream`                                                                Upload a file to the server
 :code:`/file/{filename}`        GET                                                                                                                                                                Download a file from the server
 :code:`/file/{filename}`        DELETE                                                                                                                                                             Delete a file in the server
 :code:`/dec-tool/decompose`     PATCH    :code:`model_filename: string`                                                                                                                            Decompose the architecture of a RADON model
 :code:`/dec-tool/optimize`      PATCH    :code:`model_filename: string`                                                               :code:`total_cost: number`, :code:`measures: object array`   Optimize the deployment of a RADON model
 :code:`/dec-tool/enhance`       PATCH    :code:`model_filename: string`, :code:`data_filename: string`                                                                                             Enhance the accuracy of a RADON model
 :code:`/dec-tool/consolidate`   PATCH    :code:`model_filename: string`, :code:`data_filename: string`                                                                                             Consolidate the assignment of a RADON model
=============================== ======== =============================================================== ============================ ============================================================ =============================================

A demo application example (thumbnail generation) based on the definitions of TOSCA types specific to the decomposition tool is provided `here <https://github.com/radon-h2020/radon-decomposition-tool>`_. Two models are included in this example, one with an open workload and the other with a closed workload. To try the decomposition tool on the former, please perform the following steps:

1. Clone the repository and enter the model directory:

::

  git clone https://github.com/radon-h2020/radon-decomposition-tool.git && cd radon-decomposition-tool/demo-app

2. Upload the original model to the server:

::

  curl -X POST http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.tosca -F 'file=@open_model.tosca'

3. Optimize the deployment of the model:

::

  curl -X PATCH http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/dec-tool/optimize?filename=model.tosca

4. Back up the original model in place:

::

  cp open_model.tosca open_model.tosca.bkp

5. Download the resultant model from the server:

::

  curl -X GET http://ec2-108-128-104-167.eu-west-1.compute.amazonaws.com:9000/files/model.tosca -o open_model.tosca

Additional information about the resultant model will be reported upon completion of deployment optimization (step 3), including predictions of the total operating cost and performance measures under consideration. In this example, the decomposition tool will return the mean as well as the 90th, 95th and 99th percentiles of the predicted response time distribution for the `AwsLambdaFunction_0` node since a `MeanReponseTime` policy is attached to the `execute` entry of that node.

It is **HIGHLY RECOMMENDED** to use a different filename for each uploaded model, e.g. putting a UUID into the filename (`model_5da82fdc-ae4c-48c4-ab5f-369a9a4fdee3.tosca`), so as to minimize the possibility of collisions between concurrent requests. Last but **MOST IMPORTANTLY**, do not upload a model with any sensitive information, e.g. a pair of AWS access key ID and secrete access key. We currently cannot prevent other users from accessing your models in the server.
