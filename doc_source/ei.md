# Use Amazon SageMaker Elastic Inference \(EI\)<a name="ei"></a>

By using Amazon Elastic Inference \(EI\), you can speed up the throughput and decrease the latency of getting real\-time inferences from your deep learning models that are deployed as [Amazon SageMaker hosted models](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-hosting.html), but at a fraction of the cost of using a GPU instance for your endpoint\. EI allows you to add inference acceleration to a hosted endpoint for a fraction of the cost of using a full GPU instance\. Add an EI accelerator in one of the available sizes to a deployable model in addition to a CPU instance type, and then add that model as a production variant to an endpoint configuration that you use to deploy a hosted endpoint\. You can also add an EI accelerator to a Amazon SageMaker [notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) so that you can test and evaluate inference performance when you are building your models\. 

Elastic Inference is supported in EI\-enabled versions of TensorFlow and MXNet\. To use any other deep learning framework, export your model by using ONNX, and then import your model into MXNet\. You can then use your model with EI as an MXNet model\. For information about importing an ONNX model into MXNet, see [https://mxnet\.apache\.org/api/python/docs/tutorials/packages/onnx/super\_resolution\.html](https://mxnet.apache.org/api/python/docs/tutorials/packages/onnx/super_resolution.html)\.

**Topics**
+ [How EI Works](#ei-how-it-works)
+ [Choose an EI Accelerator Type](#ei-choose-type)
+ [Use EI in a Amazon SageMaker Notebook Instance](#ei-intro-notebook)
+ [Use EI on a Hosted Endpoint](#ei-intro-endpoint)
+ [Frameworks that Support EI](#ei-supported-frameworks)
+ [Use EI with Amazon SageMaker Built\-in Algorithms](#ei-built-in)
+ [EI Sample Notebooks](#ei-intro-sample-nb)
+ [Set Up to Use EI](ei-setup.md)
+ [Attach EI to a Notebook Instance](ei-notebook-instance.md)
+ [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)

## How EI Works<a name="ei-how-it-works"></a>

Amazon Elastic Inference accelerators are network attached devices that work along with Amazon SageMaker instances in your endpoint to accelerate your inference calls\. Elastic Inference accelerates inference by allowing you to attach fractional GPUs to any Amazon SageMaker instance\. You can select the client instance to run your application and attach an Elastic Inference accelerator to use the right amount of GPU acceleration for your inference needs\. Elastic Inference helps you lower your cost when not fully utilizing your GPU instance for inference\. We recommend trying Elastic Inference with your model using different CPU instances and accelerator sizes\.

The following EI accelerator types are available\. You can configure your endpoints or notebook instances with any EI accelerator type\.

In the table, the throughput in teraflops \(TFLOPS\) is listed for both single\-precision floating\-point \(F32\) and half\-precision floating\-point \(F16\) operations\. The memory in GB is also listed\.


| Accelerator Type | F32 Throughput in TFLOPS | F16 Throughput in TFLOPS | Memory in GB | 
| --- | --- | --- | --- | 
| ml\.eia2\.medium | 1 | 8 | 2 | 
| ml\.eia2\.large | 2 | 16 | 4 | 
| ml\.eia2\.xlarge | 4 | 32 | 8 | 
| ml\.eia1\.medium | 1 | 8 | 1 | 
| ml\.eia1\.large | 2 | 16 | 2 | 
| ml\.eia1\.xlarge | 4 | 32 | 4 | 

## Choose an EI Accelerator Type<a name="ei-choose-type"></a>

Consider the following factors when choosing an accelerator type for a hosted model:
+ Models, input tensors and batch sizes influence the amount of accelerator memory you need\. Start with an accelerator type that provides at least as much memory as the file size of your trained model\.
+ Demands on CPU compute resources, GPU\-based acceleration, and CPU memory vary significantly between different kinds of deep learning models\. The latency and throughput requirements of the application also determine the amount of compute and acceleration you need\. Thoroughly test different configurations of instance types and EI accelerator sizes to make sure you choose the configuration that best fits the performance needs of your application\.

For more information on selecting an EI accelerator, see:
+ [Amazon Elastic Inference Overview ](https://aws.amazon.com/machine-learning/elastic-inference/)
+ [Choosing an Instance and Accelerator Type for Your Model](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/before.html#getting-started-choosing)
+ [Optimizing costs in Amazon Elastic Inference with TensorFlow](https://aws.amazon.com/blogs/machine-learning/optimizing-costs-in-amazon-elastic-inference-with-amazon-tensorflow/)

## Use EI in a Amazon SageMaker Notebook Instance<a name="ei-intro-notebook"></a>

Typically, you build and test machine learning models in a Amazon SageMaker notebook before you deploy them for production\. You can attach EI to your notebook instance when you create the notebook instance\. You can set up an endpoint that is hosted locally on the notebook instance by using the local mode supported by TensorFlow and MXNet estimators and models in the Amazon SageMaker Python SDK to test inference performance\. For instructions on how to attach EI to a notebook instance and set up a local endpoint for inference, see [Attach EI to a Notebook Instance](ei-notebook-instance.md)\.

## Use EI on a Hosted Endpoint<a name="ei-intro-endpoint"></a>

When you are ready to deploy your model for production to provide inferences, you create a Amazon SageMaker hosted endpoint\. You can attach EI to the instance where your endpoint is hosted to increase its performance at providing inferences\. For instructions on how to attach EI to a hosted endpoint instance, see [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)\.

## Frameworks that Support EI<a name="ei-supported-frameworks"></a>

EI is designed to be used with AWS enhanced versions of TensorFlow or Apache MXNet machine learning frameworks\. These enhanced versions of the frameworks are automatically built into containers when you use the Amazon SageMaker Python SDK, or you can download them as binary files and import them in your own Docker containers\. You can download the EI\-enabled binary for TensorFlow from the Amazon S3 bucket at [ https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-tensorflow](                 https://s3.console.aws.amazon.com/s3/buckets/amazonei-tensorflow)\. For information about building a container that uses the EI\-enabled version of TensorFlow, see [https://github\.com/aws/sagemaker\-tensorflow\-container\#building\-the\-sagemaker\-elastic\-inference\-tensorflow\-serving\-container](https://github.com/aws/sagemaker-tensorflow-container#building-the-sagemaker-elastic-inference-tensorflow-serving-container)\. You can download the EI\-enabled binary for Apache MXNet from the public Amazon S3 bucket at [https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-apachemxnet](https://s3.console.aws.amazon.com/s3/buckets/amazonei-apachemxnet)\. For information about buildling a container that uses the EI\-enabled version of MXNet, see [https://github\.com/aws/sagemaker\-mxnet\-container\#building\-the\-sagemaker\-elastic\-inference\-mxnet\-container](https://github.com/aws/sagemaker-mxnet-container#building-the-sagemaker-elastic-inference-mxnet-container)\.

To use EI in a hosted endpoint, you can use any of the following, depending on your needs\.
+ SageMaker Python SDK TensorFlow \- if you want to use TensorFlow and you don't need to build a custom container\.
+ SageMaker Python SDK MXNet \- if you want to use MXNet and you don't need to build a custom container\.
+ The low\-level AWS Amazon SageMaker SDK for Python \(Boto 3\) \- if you need to build a custom container\.

Typically, you don't need to create a custom container unless your model is very complex and requires extensions to a framework that the Amazon SageMaker pre\-built containers do not support\.

## Use EI with Amazon SageMaker Built\-in Algorithms<a name="ei-built-in"></a>

Currently, the [Image Classification Algorithm](image-classification.md) and [Object Detection Algorithm](object-detection.md) built\-in algorithms support EI\. For an example that uses the Image Classification algorithm with EI, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/introduction\_to\_amazon\_algorithms/imageclassification\_caltech/Image\-classification\-fulltraining\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.ipynb)\.

## EI Sample Notebooks<a name="ei-intro-sample-nb"></a>

The following Sample notebooks provide examples of using EI in Amazon SageMaker:
+ [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/tensorflow\_iris\_dnn\_classifier\_using\_estimators/tensorflow\_iris\_dnn\_classifier\_using\_estimators\_elastic\_inference\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/tensorflow_iris_dnn_classifier_using_estimators/tensorflow_iris_dnn_classifier_using_estimators_elastic_inference.ipynb) 
+  [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/tensorflow\_iris\_dnn\_classifier\_using\_estimators/tensorflow\_iris\_dnn\_classifier\_using\_estimators\_elastic\_inference\_local\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/tensorflow_iris_dnn_classifier_using_estimators/tensorflow_iris_dnn_classifier_using_estimators_elastic_inference_local.ipynb) 
+ [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/mxnet\_mnist/mxnet\_mnist\_elastic\_inference\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference.ipynb)
+ [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/mxnet\_mnist/mxnet\_mnist\_elastic\_inference\_local\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference_local.ipynb) 
+ [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/tensorflow\_serving\_using\_elastic\_inference\_with\_your\_own\_model/tensorflow\_serving\_pretrained\_model\_elastic\_inference\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/tensorflow_serving_using_elastic_inference_with_your_own_model/tensorflow_serving_pretrained_model_elastic_inference.ipynb)