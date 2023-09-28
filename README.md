# COAST - Cost Optimization and Saving Toolkit for Amazon Managed Grafana

## Pre-requisites

- AWS Organization
- AWS IAM Identity Center

# Cloud Formation Template Deployment

## 1. Setup

The setup process will create the following resources, along with it's dependencies:

- A new AWS Cost and Usage Report (AWS CUR) (optional)
- An Amazon Athena database
- Glue/Lamda infrastructure to update Athena database with CUR
- An Amazon Managed Grafana workspace
- A Grafana Athena database datasource
- A Grafana dashboard

## 2. CUR report
In the Cloud Formation template you have the option to specify if you would like the template to create a new Cost and Usage Report (CUR).  If you already have an existing CUR and would like to use it, you can provide the report name to use as well.  The template will create supporting infrastructure required to update the Grafana datasource (Athena) with CUR data.  To learn more on the CUR/Athena integration see [here](https://docs.aws.amazon.com/cur/latest/userguide/use-athena-cf.html).  

#### Create New CUR
This option will create a new CUR and supporting infrastructure to hydrate the athena database with CUR data.  Note, it will take at least 24 hours for a new CUR to populate with data. 

- In CloudFormation, create a new stack from the cloudformation/coast-cfn.yaml
- Select 'true' in the CreateCURReport select form
- Enter a prefix path name for the new report in the CURReportPrefixName field
- Note, an entry in the CURReportName field is not needed for this option.  The template will generate a report name for you based on the CFN stack name
- Note, an S3 bucket name in CURDataBucketName is not needed for this option.  An S3 data bucket will be named and created based on the CFN stack name

#### Use Existing CUR
Use this setting only if you already have a CUR and you would like Grafana to use that existing CUR as the datasource.  This option will use your existing CUR and create supporting infrastructure to hydrate the Athena database with CUR data.

- Ensure your CUR bucket of your existing CUR report is located in the same region as this Cloud Formation stack ([see same region note in documentation](https://docs.aws.amazon.com/cur/latest/userguide/use-athena-cf.html)).  
- In CloudFormation, create a new stack from the cloudformation/coast-cfn.yaml
- Select 'false' in the CreateCURReport select form
- Enter the name of your existing CUR in the CURReportName field
- Enter the prefix path of the existing CUR in the CURReportPrefixName field
- Enter the S3 bucket of the existing CUR in the CURDataBucketName


### 4. Post Installation Steps
- Grafana workspaces require an identity provider (IdP) to enable users to log in to the workspace.
  - We recommend AWS IAM Identity Center.  Follow instructions in the [Grafana User Guide](https://docs.aws.amazon.com/grafana/latest/userguide/AMG-manage-users-and-groups-AMG.html) to setup user access.
  - Login with the identity to the COAST Grafana workspace URL
  - The dashboard will be automatically imported under the General folder in the Dashboards menu
