# gg-cicd

Clone the repo with submodules:

```bash
git clone --recurse-submodules
```

Follow <https://aws.amazon.com/blogs/iot/implementing-a-ci-cd-pipeline-for-aws-iot-greengrass-projects/>

* Deploy the `test group` CF template [gg-cicd-environment-stack.yml](gg-cicd-environment-stack.yml) <https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-iot-blog-assets.s3.amazonaws.com/cicd-pipeline-aws-iot-greengrass/gg-cicd-environment-stack.yml&stackName=gg-cicd-test-environment&param_CoreName=gg-cicd-test>

```bash
aws cloudformation deploy --template-file gg-cicd-environment-stack.yml --stack-name gg-cicd-test-environment --capabilities CAPABILITY_IAM --parameter-overrides CoreName=gg-cicd-test myKeyPair=<your-key>
```

* Deploy `prod group` CF template <https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-iot-blog-assets.s3.amazonaws.com/cicd-pipeline-aws-iot-greengrass/gg-cicd-environment-stack.yml&stackName=gg-cicd-prod-environment&param_CoreName=gg-cicd-prod>

```bash
aws cloudformation deploy --template-file gg-cicd-environment-stack.yml --stack-name gg-cicd-prod-environment --capabilities CAPABILITY_IAM --parameter-overrides CoreName=gg-cicd-prod myKeyPair=<your-key>
```

* Deploy the CI/CD pipeline CF template <https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-iot-blog-assets.s3.amazonaws.com/cicd-pipeline-aws-iot-greengrass/gg-cicd-pipeline-stack.yml&stackName=gg-cicd-pipeline>

```bash
aws cloudformation deploy --template-file gg-cicd-pipeline-stack.yml --stack-name gg-cicd-pipeline --capabilities CAPABILITY_IAM --parameter-overrides CoreName=gg-cicd-prod 
```

```bash
wget https://aws-iot-blog-assets.s3.amazonaws.com/cicd-pipeline-aws-iot-greengrass/greengrass-cicd-project.zip
git clone <codecommit-repo>
unzip greengrass-cicd-project.zip -d greengrass-cicd-project
```

Deploy the project:

```bash
cd greengrass-cicd-project/test
./setup_parameter_store.sh
cd ..
```

Next, push the project code to your repo to kick off the pipeline:

```bash
git add .
git commit -m 'adding project code'
git push -u origin master
```

Check the deployment is successful and go to "Interacting with the deployed Lambda function" in the blog post.

Deploy the SageMaker CF <https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=ggmlworkshop&templateURL=https://s3-us-west-2.amazonaws.com/iotworkshop/lab43-sagemaker.json> from <https://iot.awsworkshops.com/aws-greengrass-ml/lab43-mlsagemakerworkshop/>

```bash
aws cloudformation deploy --template-file lab43-sagemaker.json --stack-name ggmlworkshop --capabilities CAPABILITY_IAM
```

## Resources

- <https://aws.amazon.com/blogs/iot/implementing-a-ci-cd-pipeline-for-aws-iot-greengrass-projects/>

- <https://iot.awsworkshops.com/aws-greengrass-ml/lab43-mlsagemakerworkshop/>

## Maintenance

Add CodeCommit as an origin to the module:

```bash
git remote add codecommit <code-commit-repo>
```

Push the changes in the submodule to CodeCommit and to the GitHub:

```bash
git push codecommit main:master
git push origin
```

```bash
git submodule update --remote
git submodule update --remote --rebase
git submodule update --init --recursive
```

Unstuck deletion of custom resources <https://aws.amazon.com/premiumsupport/knowledge-center/cloudformation-lambda-resource-delete/>
