# Create a Custom Worker Template \(Console\)<a name="create-worker-template-console"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

You can use a worker template to customize the interface and instructions that your workers see when working on your tasks\. Use the procedures in this topic to create a worker task template in the Amazon SageMaker console\. A starter template is provided for Amazon Textract and Amazon Rekognition tasks\. Using this procedures to create a worker template will allow you to choose this template when creating a flow definition in the console\. The procedure will also produce a worker template ARN that can be used to create a flow definition using the API operation [ `CreateFlowDefinition`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html)\.

If you are creating a template for an Amazon Textract or Amazon Rekognition task type, and you want to preview your worker template, you will need to attach the policy described in [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai) to the IAM role that you use in this procedure\. 

**To create a worker task template in the Amazon SageMaker console**

1. Open the Amazon SageMaker console at [https://console.aws.amazon.com/](https://console.aws.amazon.com/)\.

1. In Amazon Augmented AI in the left navigation pane, choose **Worker task templates**\.

1. Choose **Create template**\.

1. In **Template name**, enter a unique name\.

1. \(Optional\) Enter an **IAM role** that grants A2I the permissions necessary to call services on your behalf\. 

1. In **Template type**, choose a template type from the drop\-down menu\. If you are creating a template for a **Textract\-form extraction** or **Rekognition\-image moderation** task, choose the appropriate option\. 

1. Enter your custom template elements as follows:
   + If you selected the Amazon Textract or Amazon Rekognition task template, the **Template editor** autopopulates with a default template that you can customize\. 
   + If you are using a custom template, enter your predefined template in the editor\. 

1. \(Optional\) To complete this step, you must provided an IAM role ARN with permission to read Amazon S3 objects that get rendered on your user interface in **Step 5**\. 

   You can only preview your template if you are creating templates for Amazon Textract or Amazon Rekognition\. 

   Choose **See preview** to preview the interface and instructions that workers will see\. This is an interactive preview\. After you complete the sample task and choose **Submit**, you see the resulting output from the task that you just performed\. 

   If you are creating a worker task template for a custom task type, you can preview your worker task UI using `RenderUiTemplate`\. For more information, see [Preview a Worker Task Template](a2i-custom-templates.md#a2i-preview-your-custom-template)\.

1. When you're satisfied with your template, choose **Create**\.

After you've created your template, you can select that template when you create a human review workflow in the console\. Your template also appears in the Amazon Augmented AI section of the Amazon SageMaker console under **Worker task templates**\. Choose your template to view its ARN\.